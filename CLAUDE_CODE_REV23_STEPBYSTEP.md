# Claude Code — REV_23 Step-by-Step Implementation Guide
# 5 Steps | Commit After Each | Wait for Confirmation Before Proceeding

---

## MASTER RULES — READ BEFORE STARTING

1. Work only on `index.html`. Do not touch any other file.
2. Do not rewrite, reformat, or restructure any existing code.
3. After completing each step: stage, commit, then STOP and ask:
   **"Step X complete and committed. Please verify and reply CONTINUE to proceed to Step Y."**
4. Do not proceed to the next step until the user explicitly replies "CONTINUE".
5. If any find target cannot be located exactly, STOP and report it — do not guess or improvise.

---

## STEP 1 — Remove Fried Rice cards from Noodles section

### What to do
Delete exactly 3 lines from the Noodles section HTML.

### Find and delete these 3 lines (they appear consecutively after the Veg Noodles card):
```html
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-orange"></div><span class="ml-4 font-bold text-lg">CH Fried Rice</span><div class="flex items-center gap-4"><button onclick="change('Rice_Chx', -1)" class="control-btn bg-slate-100">-</button><span id="qty-Rice_Chx" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('Rice_Chx', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-yellow"></div><span class="ml-4 font-bold text-lg">Egg Fried Rice</span><div class="flex items-center gap-4"><button onclick="change('Rice_Egg', -1)" class="control-btn bg-slate-100">-</button><span id="qty-Rice_Egg" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('Rice_Egg', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-green"></div><span class="ml-4 font-bold text-lg">Veg Fried Rice</span><div class="flex items-center gap-4"><button onclick="change('Rice_Veg', -1)" class="control-btn bg-slate-100">-</button><span id="qty-Rice_Veg" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('Rice_Veg', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
```

### Verify before committing
- [ ] Noodles section now has exactly 3 items: Chicken, Egg, Veg
- [ ] No Rice_Chx, Rice_Egg, Rice_Veg id attributes exist anywhere in the HTML body

### Commit
```
git add index.html
git commit -m "REV_23 Step 1: Remove fried rice cards from noodles section"
```

### Then say exactly:
**"Step 1 complete and committed. Noodles section now has 3 items only. Please verify and reply CONTINUE to proceed to Step 2 (Replace Martabak with Fried Rice section)."**

---

## STEP 2 — Replace Martabak section with Fried Rice section

### What to do
Replace the entire Martabak section HTML with a new Fried Rice section. No size buttons. Flat $20 price.

### Find this entire block:
```html
            <section>
                <h2 class="category-header text-xl font-black text-slate-700">MARTABAK</h2>
                <div class="space-y-2">
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-orange"></div><span class="ml-4 font-bold text-lg">Chicken</span><div class="flex items-center gap-2"><div class="flex gap-1 mr-2" id="M_CHX_S"><button onclick="setSize('M_CHX', 22)" class="size-btn active">S</button><button onclick="setSize('M_CHX', 25)" class="size-btn">M</button><button onclick="setSize('M_CHX', 30)" class="size-btn">L</button></div><button onclick="change('M_CHX', -1)" class="control-btn bg-slate-100">-</button><span id="qty-M_CHX" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('M_CHX', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-yellow"></div><span class="ml-4 font-bold text-lg">Egg</span><div class="flex items-center gap-2"><div class="flex gap-1 mr-2" id="M_EGG_S"><button onclick="setSize('M_EGG', 22)" class="size-btn active">S</button><button onclick="setSize('M_EGG', 25)" class="size-btn">M</button><button onclick="setSize('M_EGG', 30)" class="size-btn">L</button></div><button onclick="change('M_EGG', -1)" class="control-btn bg-slate-100">-</button><span id="qty-M_EGG" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('M_EGG', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-dark-green"></div><span class="ml-4 font-bold text-lg">Spinach</span><div class="flex items-center gap-2"><div class="flex gap-1 mr-2" id="M_SPI_S"><button onclick="setSize('M_SPI', 22)" class="size-btn active">S</button><button onclick="setSize('M_SPI', 25)" class="size-btn">M</button><button onclick="setSize('M_SPI', 30)" class="size-btn">L</button></div><button onclick="change('M_SPI', -1)" class="control-btn bg-slate-100">-</button><span id="qty-M_SPI" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('M_SPI', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                </div>
            </section>
```

### Replace with exactly this:
```html
            <section>
                <h2 class="category-header text-xl font-black text-slate-700">FRIED RICE ($20)</h2>
                <div class="space-y-2">
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-orange"></div><span class="ml-4 font-bold text-lg">CFR <span class="text-sm text-slate-400">$20</span></span><div class="flex items-center gap-4"><button onclick="change('FR_CHX', -1)" class="control-btn bg-slate-100">-</button><span id="qty-FR_CHX" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('FR_CHX', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-yellow"></div><span class="ml-4 font-bold text-lg">EFR <span class="text-sm text-slate-400">$20</span></span><div class="flex items-center gap-4"><button onclick="change('FR_EGG', -1)" class="control-btn bg-slate-100">-</button><span id="qty-FR_EGG" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('FR_EGG', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                    <div class="item-card p-3 rounded-xl"><div class="h-bar bg-dark-green"></div><span class="ml-4 font-bold text-lg">VFR <span class="text-sm text-slate-400">$20</span></span><div class="flex items-center gap-4"><button onclick="change('FR_VEG', -1)" class="control-btn bg-slate-100">-</button><span id="qty-FR_VEG" class="text-4xl font-black w-10 text-center">0</span><button onclick="change('FR_VEG', 1)" class="control-btn bg-slate-900 text-white">+</button></div></div>
                </div>
            </section>
```

### Verify before committing
- [ ] Section heading reads "FRIED RICE ($20)"
- [ ] 3 items: CFR, EFR, VFR — each with $20 label
- [ ] No S/M/L size buttons anywhere in this section
- [ ] No `setSize`, `M_CHX`, `M_EGG`, `M_SPI`, `M_CHX_S`, `M_EGG_S`, `M_SPI_S` anywhere in the HTML body
- [ ] `qty-FR_CHX`, `qty-FR_EGG`, `qty-FR_VEG` span IDs present

### Commit
```
git add index.html
git commit -m "REV_23 Step 2: Replace Martabak section with Fried Rice (CFR/EFR/VFR)"
```

### Then say exactly:
**"Step 2 complete and committed. Martabak section replaced with Fried Rice. Please verify and reply CONTINUE to proceed to Step 3 (Update JavaScript — prices, friendlyNames, remove Martabak functions)."**

---

## STEP 3 — Update JavaScript: prices, friendlyNames, remove Martabak code

### This step has 6 sub-changes. Do all 6 before committing.

---

### 3a. Update prices map — remove Martabak keys and Rice keys, add Fried Rice keys

Find:
```javascript
            'M_CHX': 22, 'M_EGG': 22, 'M_SPI': 22,
```
Replace with:
```javascript
            'FR_CHX': 20, 'FR_EGG': 20, 'FR_VEG': 20,
```

Then find and delete this line (Rice keys added in previous session):
```javascript
            'Rice_Chx': 20, 'Rice_Egg': 20, 'Rice_Veg': 20,
```

---

### 3b. Update friendlyNames — remove Martabak comment, remove Rice keys, add Fried Rice keys

Find and delete this comment line:
```javascript
            // Martabak names are dynamic (see getMartabakName)
```

Find and delete these 3 lines if they exist (Rice keys from previous session):
```javascript
            'Rice_Chx':  'CH Fr Rice',
            'Rice_Egg':  'Egg Fr Rice',
            'Rice_Veg':  'Veg Fr Rice',
```

Find this line:
```javascript
            'CH_PB':     'Pav Bhaji',
```
Insert these 3 lines immediately before it:
```javascript
            'FR_CHX':    'CH Fr Rice',
            'FR_EGG':    'Egg Fr Rice',
            'FR_VEG':    'Veg Fr Rice',
```

---

### 3c. Delete getMartabakName() function entirely

Find and delete this entire function including its comment:
```javascript
        // Returns current size label for Martabak items based on selected price
        function getMartabakName(key) {
            const sizeMap = { 22: 'S', 25: 'M', 30: 'L' };
            const size = sizeMap[prices[key]] || 'S';
            if (key === 'M_CHX') return `CH Mrtbk ${size}`;
            if (key === 'M_EGG') return `Egg Mrtbk ${size}`;
            if (key === 'M_SPI') return `Spinach Chz Mrtbk ${size}`;
        }
```

---

### 3d. Simplify getItemName() — remove Martabak special case

Find:
```javascript
        // Returns display name for any key, handling dynamic Martabak names
        function getItemName(key) {
            if (key === 'M_CHX' || key === 'M_EGG' || key === 'M_SPI') {
                return getMartabakName(key);
            }
            return friendlyNames[key] || key;
        }
```
Replace with:
```javascript
        // Returns display name for any key
        function getItemName(key) {
            return friendlyNames[key] || key;
        }
```

---

### 3e. Remove Martabak size reset from printOrder() setTimeout block

Find and delete these 5 lines inside the setTimeout reset block in printOrder():
```javascript
                // Reset Martabak size buttons to default (S)
                document.querySelectorAll('.size-btn').forEach(b => b.classList.remove('active'));
                document.querySelectorAll('#M_CHX_S .size-btn:first-child, #M_EGG_S .size-btn:first-child, #M_SPI_S .size-btn:first-child')
                    .forEach(b => b.classList.add('active'));
                prices['M_CHX'] = 22; prices['M_EGG'] = 22; prices['M_SPI'] = 22;
```

---

### 3f. Remove Martabak size restoration from loadState()

Inside loadState(), find and delete this entire block:
```javascript
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
```

Also inside loadState(), find and delete these lines:
```javascript
            // Only restore Martabak prices — never override base menu prices
            ['M_CHX', 'M_EGG', 'M_SPI'].forEach(key => {
                if (parsedPrices[key]) prices[key] = parsedPrices[key];
            });
```

---

### Verify before committing (all 6 sub-changes)
- [ ] `prices` map has `FR_CHX`, `FR_EGG`, `FR_VEG` at $20 — no `M_CHX`, `M_EGG`, `M_SPI`, no `Rice_` keys
- [ ] `friendlyNames` has `FR_CHX`, `FR_EGG`, `FR_VEG` entries — no Martabak comment, no Rice_ keys
- [ ] `getMartabakName()` function does not exist anywhere in the file
- [ ] `getItemName()` is exactly 3 lines — no Martabak conditional
- [ ] No `.size-btn` reset code in printOrder() setTimeout block
- [ ] No Martabak size restoration in loadState()
- [ ] `setSize()` function may still exist in the file — that is acceptable, do not delete it

### Commit
```
git add index.html
git commit -m "REV_23 Step 3: Remove all Martabak JS — prices, friendlyNames, getMartabakName, loadState cleanup"
```

### Then say exactly:
**"Step 3 complete and committed. All Martabak JavaScript removed and Fried Rice keys added. Please verify and reply CONTINUE to proceed to Step 4 (Rename C.T Wrap to 65 Wrap)."**

---

## STEP 4 — Rename C.T Wrap to 65 Wrap

### What to do
One label change in the Wraps section HTML. Nothing else.

### Find:
```html
<span class="ml-4 font-bold text-lg">C.T Wrap</span>
```

### Replace with:
```html
<span class="ml-4 font-bold text-lg">65 Wrap</span>
```

### Verify before committing
- [ ] Wraps section first item shows "65 Wrap"
- [ ] `Wrap_CT` key is unchanged — only the display label changed
- [ ] C.R.T Wrap and M.T Wrap labels are untouched

### Commit
```
git add index.html
git commit -m "REV_23 Step 4: Rename C.T Wrap to 65 Wrap"
```

### Then say exactly:
**"Step 4 complete and committed. C.T Wrap renamed to 65 Wrap. Please verify and reply CONTINUE to proceed to Step 5 (Update noodle print copy logic)."**

---

## STEP 5 — Update noodle print copy logic

### New rule to implement:
- Noodles only (no other items) → 2 copies
- Noodles + other items → 3 copies
- No noodles → 2 copies

### This step has 3 sub-changes.

---

### 5a. Remove Rice keys from noodleKeys array

Find:
```javascript
        const noodleKeys = ['Noodle_Chx', 'Noodle_Egg', 'Noodle_Veg', 'Rice_Chx', 'Rice_Egg', 'Rice_Veg'];
```
Replace with:
```javascript
        const noodleKeys = ['Noodle_Chx', 'Noodle_Egg', 'Noodle_Veg'];
```

If the Rice keys are not present and it already reads:
```javascript
        const noodleKeys = ['Noodle_Chx', 'Noodle_Egg', 'Noodle_Veg'];
```
Leave it unchanged and move to 5b.

---

### 5b. Add hasNoodlesWithOtherItems() function

Find the existing hasNoodles() function:
```javascript
        function hasNoodles() {
            return noodleKeys.some(k => (orderData[k] || 0) > 0);
        }
```
Replace with:
```javascript
        function hasNoodles() {
            return noodleKeys.some(k => (orderData[k] || 0) > 0);
        }

        function hasNoodlesWithOtherItems() {
            const noodlesPresent = noodleKeys.some(k => (orderData[k] || 0) > 0);
            if (!noodlesPresent) return false;
            const otherPresent = Object.keys(orderData).some(k => !noodleKeys.includes(k) && (orderData[k] || 0) > 0);
            return noodlesPresent && otherPresent;
        }
```

---

### 5c. Update updateTotal() — print button label

Find:
```javascript
            btn.innerText = hasNoodles() ? 'Print 3 Copies' : 'Print 2 Copies';
```
Replace with:
```javascript
            btn.innerText = hasNoodlesWithOtherItems() ? 'Print 3 Copies' : 'Print 2 Copies';
```

---

### 5d. Update printOrder() — copy decision logic

Find the if/else block inside printOrder() that builds fullJob by calling generateReceipt(). It will look like one of these two patterns depending on whether the anchor element fix has been applied:

**Pattern A (original):**
```javascript
            if (hasNoodles()) {
```

**Pattern B (anchor fix applied):**
```javascript
            if (hasNoodles()) {
```

In both cases, find `if (hasNoodles())` inside printOrder() and replace only that condition:

Change:
```javascript
            if (hasNoodles()) {
                // 3-copy noodle rule: Main Kitchen + Noodle Kitchen + Customer Receipt
```
To:
```javascript
            if (hasNoodlesWithOtherItems()) {
                // 3-copy rule: noodles AND other items present
```

Do not touch the generateReceipt() calls, the fullJob assembly, or anything else in that block.

---

### Verify before committing
- [ ] `noodleKeys` has exactly 3 keys — no Rice_ keys
- [ ] `hasNoodlesWithOtherItems()` function exists immediately after `hasNoodles()`
- [ ] `updateTotal()` uses `hasNoodlesWithOtherItems()` for button label
- [ ] `printOrder()` uses `hasNoodlesWithOtherItems()` for copy decision
- [ ] `hasNoodles()` function still exists unchanged (it may be used elsewhere)
- [ ] Test logic mentally:
  - Noodle only in cart → `hasNoodlesWithOtherItems()` returns false → Print 2 Copies ✓
  - Noodle + Tikka in cart → returns true → Print 3 Copies ✓
  - Only Tikka in cart → returns false → Print 2 Copies ✓

### Commit
```
git add index.html
git commit -m "REV_23 Step 5: Update noodle copy logic — 3 copies only when noodles + other items combined"
```

### Then say exactly:
**"Step 5 complete and committed. All REV_23 changes are done. Full summary: (1) Rice cards removed from noodles, (2) Martabak replaced with Fried Rice CFR/EFR/VFR, (3) All Martabak JS cleaned up, (4) C.T Wrap renamed to 65 Wrap, (5) Noodle copy rule updated. Please do a final verify and confirm."**

---

## FINAL VERIFICATION — Run after Step 5

Search the file for each of these strings — none should exist:
- `M_CHX` — must not exist
- `M_EGG` — must not exist
- `M_SPI` — must not exist
- `Rice_Chx` — must not exist
- `Rice_Egg` — must not exist
- `Rice_Veg` — must not exist
- `getMartabakName` — must not exist
- `Martabak` — must not exist
- `size-btn` — must not exist (unless setSize() function body still present — acceptable)
- `C.T Wrap` — must not exist

These strings MUST exist:
- `FR_CHX` — must appear in HTML, prices, and friendlyNames
- `FR_EGG` — must appear in HTML, prices, and friendlyNames
- `FR_VEG` — must appear in HTML, prices, and friendlyNames
- `65 Wrap` — must appear once in HTML
- `hasNoodlesWithOtherItems` — must appear 3 times (definition + updateTotal + printOrder)
- `FRIED RICE` — must appear once in HTML section header

---

*REV_23 | 5 steps | 5 commits | Base: REV_22 branch*
