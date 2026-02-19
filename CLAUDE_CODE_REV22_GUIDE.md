# Deccan Flavours POS — REV_22 Claude Code Implementation Guide

## CRITICAL RULES — READ BEFORE TOUCHING ANYTHING

1. **Single file only.** All changes happen inside `index.html`. Do not create any new files.
2. **Do not rewrite.** Every change below is a targeted edit. Do not restructure, reformat, or move any existing code.
3. **Do not touch the following — ever:**
   - Any ESC/POS constants (`DOUBLE_SIZE`, `TALL_BOLD`, `NORMAL`, etc.)
   - `generateReceipt()` function body
   - `formatItemLine()` function
   - `wordWrap()` function
   - `getMartabakName()` and `getItemName()` functions
   - `friendlyNames` map
   - `prices` map values
   - All HTML item cards, grid layout, color bars, category sections
   - `hasNoodles()` function
   - `updateTotal()` function body
   - The RawBT handoff line: `window.location.href = 'rawbt:base64,...'`
4. **Apply changes in the exact order listed** — Sections B → C → F → G → H → I.
5. **After all changes, verify** the 6 checks listed at the bottom of this guide.

---

## SECTION B — 4-Digit Order Number + Daily Auto-Reset

### B1. HTML Change — Line 47

Find this exact line:
```html
<span class="text-7xl font-black text-slate-900" id="order-num">#001</span>
```

Replace with:
```html
<span class="text-7xl font-black text-slate-900" id="order-num">#0001</span>
```

**That's the only HTML change in this section.**

---

### B2. Script Change — Inside `printOrder()` reset setTimeout

Find this exact line inside the `setTimeout` callback (currently line 360):
```javascript
document.getElementById('order-num').innerText = `#${num.toString().padStart(3, '0')}`;
```

Replace with:
```javascript
const newOrderNum = num.toString().padStart(4, '0');
document.getElementById('order-num').innerText = `#${newOrderNum}`;
localStorage.setItem('df_orderNum', newOrderNum);
```

---

### B3. New Function — `getInitialOrderNum()`

Add this function to the **new functions block** you will create at the bottom of the `<script>` tag (before the page load sequence). Full instructions for where to place all new functions are in Section I.

```javascript
function getInitialOrderNum() {
    const today = new Date().toDateString();
    const savedDate = localStorage.getItem('df_date');
    if (savedDate !== today) {
        // New day — reset counter
        localStorage.setItem('df_date', today);
        localStorage.setItem('df_orderNum', '0001');
        return '0001';
    }
    return localStorage.getItem('df_orderNum') || '0001';
}
```

---

## SECTION C — localStorage State Persistence

### C1. New Function — `saveState()` (debounced)

Add to new functions block:

```javascript
let _saveTimer = null;
function saveState() {
    clearTimeout(_saveTimer);
    _saveTimer = setTimeout(() => {
        try {
            localStorage.setItem('df_orderData', JSON.stringify(orderData));
            localStorage.setItem('df_prices', JSON.stringify(prices));
        } catch(e) { /* silent fail on QuotaExceededError */ }
    }, 300);
}
```

---

### C2. New Function — `loadState()`

Add to new functions block:

```javascript
function loadState() {
    try {
        const savedData = localStorage.getItem('df_orderData');
        if (savedData) {
            const parsed = JSON.parse(savedData);
            Object.assign(orderData, parsed);
            // Update all qty displays on screen
            for (let k in orderData) {
                const el = document.getElementById('qty-' + k);
                if (el) el.innerText = orderData[k];
            }
        }

        const savedPrices = localStorage.getItem('df_prices');
        if (savedPrices) {
            const parsedPrices = JSON.parse(savedPrices);
            // Only restore Martabak prices — never override base menu prices
            ['M_CHX', 'M_EGG', 'M_SPI'].forEach(key => {
                if (parsedPrices[key]) prices[key] = parsedPrices[key];
            });
            // Re-apply active class to Martabak size buttons
            const sizeMap = { 22: 0, 25: 1, 30: 2 }; // index of S/M/L button
            ['M_CHX', 'M_EGG', 'M_SPI'].forEach(key => {
                const containerId = key.replace('_', '_') + '_S';
                // Map key to container id: M_CHX → M_CHX_S, M_EGG → M_EGG_S, M_SPI → M_SPI_S
                const containerIdFixed = key + '_S';
                const container = document.getElementById(containerIdFixed);
                if (container) {
                    const btns = container.querySelectorAll('.size-btn');
                    btns.forEach(b => b.classList.remove('active'));
                    const idx = sizeMap[prices[key]];
                    if (idx !== undefined && btns[idx]) btns[idx].classList.add('active');
                }
            });
        }
    } catch(e) { /* corrupt data — silently ignore, start fresh */ }
}
```

---

### C3. Call `saveState()` at end of `change()`

Find the existing `change()` function:
```javascript
function change(id, delta) {
    orderData[id] = Math.max(0, (orderData[id] || 0) + delta);
    document.getElementById('qty-' + id).innerText = orderData[id];
    updateTotal();
}
```

Replace with:
```javascript
function change(id, delta) {
    orderData[id] = Math.max(0, (orderData[id] || 0) + delta);
    document.getElementById('qty-' + id).innerText = orderData[id];
    updateTotal();
    saveState();
}
```

---

### C4. Call `saveState()` at end of `setSize()`

Find the existing `setSize()` function:
```javascript
function setSize(itemId, price) {
    prices[itemId] = price;
    const container = event.target.parentElement;
    container.querySelectorAll('.size-btn').forEach(b => b.classList.remove('active'));
    event.target.classList.add('active');
    updateTotal();
}
```

Replace with:
```javascript
function setSize(itemId, price) {
    prices[itemId] = price;
    const container = event.target.parentElement;
    container.querySelectorAll('.size-btn').forEach(b => b.classList.remove('active'));
    event.target.classList.add('active');
    updateTotal();
    saveState();
}
```

---

### C5. Call `saveState()` inside `printOrder()` reset block

Inside the `setTimeout` callback in `printOrder()`, immediately after the Martabak price reset lines, add one call to `saveState()`:

Find this block (it ends the setTimeout):
```javascript
                prices['M_CHX'] = 22; prices['M_EGG'] = 22; prices['M_SPI'] = 22;
                updateTotal();
            }, 1000);
```

Replace with:
```javascript
                prices['M_CHX'] = 22; prices['M_EGG'] = 22; prices['M_SPI'] = 22;
                updateTotal();
                saveState();
            }, 1500);
```

**Note:** Timeout is changed from `1000` to `1500` here. This is intentional — it provides the debounce buffer required by Section G.

---

## SECTION F — Wake Lock (Screen Stay-Awake)

### F1. New Function — `requestWakeLock()`

Add to new functions block:

```javascript
async function requestWakeLock() {
    if (!('wakeLock' in navigator)) {
        console.warn('Wake Lock API not supported on this device.');
        showWakeLockWarning(false);
        return;
    }
    try {
        window._wakeLock = await navigator.wakeLock.request('screen');

        // Listen for sentinel release (browser/OS can release without visibility change)
        window._wakeLock.addEventListener('release', () => {
            window._wakeLock = null;
            showWakeLockWarning(false);
        });

        showWakeLockWarning(true); // Wake lock is active
    } catch(e) {
        console.warn('Wake lock request failed:', e.message);
        showWakeLockWarning(false);
    }
}

// Re-acquire on tab becoming visible (handles both tab-switch and sentinel release)
document.addEventListener('visibilitychange', async () => {
    if (document.visibilityState === 'visible') {
        if (!window._wakeLock || window._wakeLock.released) {
            await requestWakeLock();
        }
    }
});

function showWakeLockWarning(isActive) {
    const indicator = document.getElementById('wakelock-indicator');
    if (!indicator) return;
    if (isActive) {
        indicator.textContent = '🟢';
        indicator.title = 'Screen will stay on';
    } else {
        indicator.textContent = '🔴';
        indicator.title = 'Screen may sleep — tap to retry';
        indicator.onclick = () => requestWakeLock();
    }
}
```

---

### F2. HTML Change — Add Wake Lock Indicator to Header

Find this exact block in the HTML header (lines 42–49):
```html
    <div class="flex justify-between items-center mb-6">
        <div>
            <h1 class="text-3xl font-black text-slate-900 italic uppercase">Ramadan POS</h1>
        </div>
        <div class="text-right">
            <span class="text-7xl font-black text-slate-900" id="order-num">#0001</span>
        </div>
    </div>
```

Replace with:
```html
    <div class="flex justify-between items-center mb-6">
        <div class="flex items-center gap-2">
            <h1 class="text-3xl font-black text-slate-900 italic uppercase">Ramadan POS</h1>
            <span id="wakelock-indicator" style="font-size:1.2rem;cursor:pointer;" title="Wake lock status">⚪</span>
        </div>
        <div class="text-right">
            <span class="text-7xl font-black text-slate-900" id="order-num">#0001</span>
        </div>
    </div>
```

**What this does:** A small dot sits next to the title. 🟢 = screen will stay on. 🔴 = wake lock lost (tap it to retry). ⚪ = initialising. Invisible to customers, immediately useful to staff.

---

## SECTION G — Print Debounce Guard

### G1. Add `isPrinting` variable

Find this line at the top of the `<script>` block:
```javascript
let orderData = {};
```

Replace with:
```javascript
let orderData = {};
let isPrinting = false;
```

---

### G2. Add guard at the START of `printOrder()`

Find the opening of `printOrder()`:
```javascript
        function printOrder() {
            const total = parseFloat(document.getElementById('grand-total').innerText.replace('$', ''));
            if (total === 0) return;
```

Replace with:
```javascript
        function printOrder() {
            if (isPrinting) return;
            const total = parseFloat(document.getElementById('grand-total').innerText.replace('$', ''));
            if (total === 0) return;
            isPrinting = true;
            document.getElementById('print-btn').disabled = true;
```

---

### G3. Release guard at END of `printOrder()` reset block

Inside the setTimeout callback, the very last two lines before the closing `}, 1500);` should now be:

```javascript
                isPrinting = false;
                document.getElementById('print-btn').disabled = false;
                saveState();
            }, 1500);
```

**Full final setTimeout block for reference — make sure it looks exactly like this:**
```javascript
            setTimeout(() => {
                let num = parseInt(document.getElementById('order-num').innerText.replace('#', '')) + 1;
                const newOrderNum = num.toString().padStart(4, '0');
                document.getElementById('order-num').innerText = `#${newOrderNum}`;
                localStorage.setItem('df_orderNum', newOrderNum);
                for (let k in orderData) {
                    orderData[k] = 0;
                    let el = document.getElementById('qty-' + k);
                    if (el) el.innerText = 0;
                }
                // Reset Martabak size buttons to default (S)
                document.querySelectorAll('.size-btn').forEach(b => b.classList.remove('active'));
                document.querySelectorAll('#M_CHX_S .size-btn:first-child, #M_EGG_S .size-btn:first-child, #M_SPI_S .size-btn:first-child')
                    .forEach(b => b.classList.add('active'));
                prices['M_CHX'] = 22; prices['M_EGG'] = 22; prices['M_SPI'] = 22;
                updateTotal();
                saveState();
                isPrinting = false;
                document.getElementById('print-btn').disabled = false;
                updateReprintBtn();
            }, 1500);
```

---

## SECTION H — Reprint Last Receipt

### H1. Save last print job inside `printOrder()` BEFORE the setTimeout

Find this line in `printOrder()`:
```javascript
            window.location.href = 'rawbt:base64,' + btoa(unescape(encodeURIComponent(fullJob)));
```

Replace with:
```javascript
            // Capture order number before reset for reprint label
            const currentOrderNum = document.getElementById('order-num').innerText;
            // Save last job in memory (primary) and localStorage (page-reload survival)
            window._lastPrintJob = fullJob;
            window._lastPrintJobNum = currentOrderNum;
            try {
                localStorage.setItem('df_lastJob', fullJob);
                localStorage.setItem('df_lastJobNum', currentOrderNum);
            } catch(e) { /* silent fail — in-memory copy still works */ }

            window.location.href = 'rawbt:base64,' + btoa(unescape(encodeURIComponent(fullJob)));
```

---

### H2. New Function — `reprintLast()`

Add to new functions block:

```javascript
function reprintLast() {
    // Prefer in-memory copy (faster, no encoding issues)
    const job = window._lastPrintJob || localStorage.getItem('df_lastJob');
    if (!job) {
        alert('No previous receipt to reprint.');
        return;
    }
    window.location.href = 'rawbt:base64,' + btoa(unescape(encodeURIComponent(job)));
    // Does NOT increment order number, reset cart, or call saveState()
}
```

---

### H3. New Function — `updateReprintBtn()`

Add to new functions block:

```javascript
function updateReprintBtn() {
    const btn = document.getElementById('reprint-btn');
    if (!btn) return;
    const lastNum = window._lastPrintJobNum || localStorage.getItem('df_lastJobNum');
    if (lastNum) {
        btn.textContent = `↩ Reprint ${lastNum}`;
        btn.style.display = 'inline-flex';
    } else {
        btn.style.display = 'none';
    }
}
```

---

### H4. HTML Change — Add Reprint Button to Footer

Find the entire footer bar:
```html
    <div class="fixed bottom-0 left-0 right-0 bg-white border-t border-slate-200 p-6 flex justify-between items-center shadow-2xl">
        <div class="flex items-baseline gap-4">
            <span class="text-slate-400 font-black uppercase text-xs tracking-widest">Total</span>
            <span id="grand-total" class="text-6xl font-black text-slate-900">$0.00</span>
        </div>
        <button id="print-btn" onclick="printOrder()" class="bg-slate-900 text-white px-12 py-6 rounded-2xl text-3xl font-black uppercase tracking-widest active:scale-95 transition-transform">
            Print 2 Copies
        </button>
    </div>
```

Replace with:
```html
    <div class="fixed bottom-0 left-0 right-0 bg-white border-t border-slate-200 p-6 flex justify-between items-center shadow-2xl">
        <div class="flex items-baseline gap-4">
            <span class="text-slate-400 font-black uppercase text-xs tracking-widest">Total</span>
            <span id="grand-total" class="text-6xl font-black text-slate-900">$0.00</span>
        </div>
        <div class="flex items-center gap-4">
            <button id="reprint-btn" onclick="reprintLast()" style="display:none;background:white;color:#0f172a;border:2px solid #0f172a;padding:12px 24px;border-radius:16px;font-size:1.1rem;font-weight:900;align-items:center;cursor:pointer;">
                ↩ Reprint
            </button>
            <button id="print-btn" onclick="printOrder()" class="bg-slate-900 text-white px-12 py-6 rounded-2xl text-3xl font-black uppercase tracking-widest active:scale-95 transition-transform">
                Print 2 Copies
            </button>
        </div>
    </div>
```

---

## SECTION I — Page Load Sequence (Final Step)

### I1. Replace the existing page load call

Find this line at the very bottom of the `<script>` block (currently the last line before `</script>`):
```javascript
        updateTotal(); // initialise button state
```

Replace it entirely with:
```javascript
        // ── PAGE LOAD SEQUENCE ─────────────────────────────────────────
        // Order matters — do not reorder these calls.
        (function init() {
            // 1. Set order number (handles daily reset)
            const orderNum = getInitialOrderNum();
            document.getElementById('order-num').innerText = `#${orderNum}`;

            // 2. Restore cart state from localStorage
            loadState();

            // 3. Recalculate total from restored state
            updateTotal();

            // 4. Request wake lock (keep screen on)
            requestWakeLock();

            // 5. Show reprint button if a last job exists
            updateReprintBtn();
        })();
        // ───────────────────────────────────────────────────────────────
```

---

## POST-IMPLEMENTATION VERIFICATION CHECKLIST

After applying all changes, verify each of the following before closing the file:

**[ ] 1. Order number format**
Search for `#001` in the file. It must not exist anywhere. Only `#0001` should appear (in the HTML span and in `padStart(4, '0')`).

**[ ] 2. setTimeout duration**
Search for `}, 1000)` inside `printOrder()`. It must not exist. Only `}, 1500)` should be present.

**[ ] 3. `isPrinting` guard**
Confirm `isPrinting = false` appears at the top of the script AND inside the setTimeout reset block. It must appear in both places.

**[ ] 4. `saveState()` calls**
Confirm `saveState()` is called in: `change()`, `setSize()`, and inside the setTimeout reset block. Three locations total.

**[ ] 5. Wake lock indicator**
Confirm `id="wakelock-indicator"` exists in the header HTML and `showWakeLockWarning()` exists in the script.

**[ ] 6. Reprint button**
Confirm `id="reprint-btn"` exists in the footer HTML with `style="display:none"` as default. Confirm `reprintLast()` and `updateReprintBtn()` functions exist in the script. Confirm `updateReprintBtn()` is called both in the page load sequence and inside the setTimeout reset block.

---

## WHAT NOT TO DO — EXPLICIT PROHIBITIONS

- Do **not** add `async` to `printOrder()` — the RawBT URI handoff must remain synchronous.
- Do **not** move the `window.location.href` rawbt line — it must fire before any async operations.
- Do **not** change any value inside `friendlyNames` or `prices` maps.
- Do **not** change the `formatItemLine()`, `wordWrap()`, `generateReceipt()` functions.
- Do **not** add any `console.log` statements that reference `orderData` in a loop — at 2500 orders/day the DevTools overhead is measurable.
- Do **not** split into multiple files.

---

*Guide version: REV_22 | Base file: index.html | Date: February 19, 2026*
