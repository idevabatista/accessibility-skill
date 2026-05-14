# Accessibility Handoff Guide (V6.0)
**Bridge Design-to-Engineering: W3C ARIA Patterns + BBC + WebAIM**

> Updated 2026-05-14 with specifications extracted from W3C APG, WebAIM and BBC Mobile Guidelines.

**Do not implement without consulting the keyboard patterns below.**

---

## 1. Component Interaction Patterns (W3C APG — Full Spec)

### Button

| ARIA Attribute | Value | When to Use |
| :--- | :--- | :--- |
| `role="button"` | — | On custom `<div>` or `<span>` elements (prefer native `<button>`) |
| `aria-pressed` | `"true"` / `"false"` | Toggle buttons |
| `aria-expanded` | `"true"` / `"false"` | Accordion headers, disclosure widgets |
| `aria-haspopup` | `"dialog"` / `"menu"` / `"listbox"` | Buttons that open popups |
| `aria-label` | descriptive text | **Required** for icon-only buttons |

**Keyboard:** `Enter` or `Space` activates.
**Anti-pattern:** Never use `<div role="button">` without `tabindex="0"` + `keydown` handlers for Enter/Space.

---

### Modal Dialog (W3C APG — Full Specification)

**Minimum HTML structure:**
```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p id="dialog-desc">This action cannot be undone.</p>
  <!-- content -->
  <button>Cancel</button>
  <button>Confirm</button>
</div>
```

**Required ARIA attributes:**

| Attribute | Rule |
| :--- | :--- |
| `role="dialog"` | **Required** on the modal container |
| `aria-modal="true"` | **Required** — only when code prevents interaction outside AND CSS obscures the background |
| `aria-labelledby` | **Required** — references the `id` of the visible title |
| `aria-label` | Alternative when no visible title is present |
| `aria-describedby` | **Optional** — omit if content has complex structure (lists/tables) |

**Keyboard behaviour (W3C APG mandatory):**

| Key | Behaviour |
| :--- | :--- |
| `Tab` | Focus on next focusable element INSIDE the dialog. On last → returns to first (loop) |
| `Shift + Tab` | Focus on previous element INSIDE the dialog. On first → goes to last (loop) |
| `Escape` | Closes the modal |

**Focus management protocol:**

| Scenario | Where to place focus on open |
| :--- | :--- |
| Simple content | First interactive element |
| Complex content (lists/tables) | Static element with `tabindex="-1"` at the start of the content |
| Long content (first button outside viewport) | `tabindex="-1"` on the title or opening paragraph |
| Irreversible destructive action (delete, payment) | Least destructive action (e.g. "Cancel") |
| Informational dialog | Most frequently used element (e.g. "OK") |

**On close:** Focus returns to the **trigger element** that opened the modal.
**Required:** Include a visible button (X or "Cancel") that closes the modal.

---

### Tabs

```html
<div role="tablist" aria-label="Product sections">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">Specifications</button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2">Reviews</button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">...</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>...</div>
```

| Attribute | Element | Value |
| :--- | :--- | :--- |
| `role="tablist"` | Tab container | — |
| `role="tab"` | Each tab | — |
| `aria-selected` | Each tab | `"true"` / `"false"` |
| `aria-controls` | Each tab | ID of corresponding panel |
| `role="tabpanel"` | Each panel | — |
| `aria-labelledby` | Each panel | ID of corresponding tab |

**Keyboard:**

| Key | Behaviour |
| :--- | :--- |
| `Tab` | Enters the group (focuses the active tab) / exits the group |
| `←` / `→` | Navigates and **activates** the previous/next tab |
| `Home` | First tab |
| `End` | Last tab |

> **Critical distinction:** This pattern applies to "application tabs" that dynamically change content. If the "tabs" are links to different pages, use `Tab + Enter` (standard link behaviour).

---

### Accordion

```html
<h3>
  <button aria-expanded="false" aria-controls="panel-1">
    Section 1
  </button>
</h3>
<div id="panel-1" hidden>
  <!-- content -->
</div>
```

| Attribute | Element | Value |
| :--- | :--- | :--- |
| `aria-expanded` | Header `<button>` | `"true"` / `"false"` |
| `aria-controls` | Header `<button>` | ID of the panel |
| `hidden` or `aria-hidden` | Panel | When closed |

**Keyboard:** `Space` / `Enter` → toggle. `↑` `↓` → navigate between headers.

---

### Slider

**Keyboard:**

| Key | Behaviour |
| :--- | :--- |
| `←` `→` or `↑` `↓` | Increment / decrement value |
| `Home` | Minimum value |
| `End` | Maximum value |
| `PageUp` / `PageDown` | Larger step (e.g. 10%) |

**Double-headed slider:** `Tab` / `Shift+Tab` alternates between the two handles.

---

### Select / Combobox

**Keyboard (native select):**

| Key | Behaviour |
| :--- | :--- |
| `↑` `↓` | Navigate options |
| `Enter` | Select and close |
| `Escape` | Close without selecting |
| Typing | Filter / jump to option |

> **Anti-pattern:** Avoid `<select>` with `onChange` that triggers navigation (jump menu). Replace with `<select>` + a separate "Go" button — arrowing must not trigger actions.

---

### Live Regions (Dynamic Content)

| `aria-live` value | When to Use | Example |
| :--- | :--- | :--- |
| `"polite"` | Non-critical updates, after a pause | Upload status, chat messages, stock updates |
| `"assertive"` | Critical errors — interrupts current reading | Security validation error, payment failure |
| `"off"` | Disables announcements | Irrelevant changes |

**Equivalent roles:**
- `role="alert"` = `aria-live="assertive"` + `aria-atomic="true"`
- `role="log"` = Live region with history (chat)
- `role="status"` = `aria-live="polite"` (for status messages)

> **Implementation rule:** `aria-live` must be defined at **initial page load**. Injecting the attribute via JavaScript after load is unreliable across assistive technologies.

---

### Media (Video/Audio)

| Requirement | Level | Handoff Spec |
| :--- | :--- | :--- |
| **Synchronised captions** | WCAG 1.2.2 (A) | VTT or SRT file |
| **Audio description** | WCAG 1.2.5 (AA) | Specify if a separate AD audio track is needed |
| **Text transcript** | WCAG 1.2.1 (A) | For podcasts and standalone audio |
| **Accessible controls** | WCAG 4.1.2 (A) | All controls operable by keyboard + `aria-label` |
| **No autoplay with audio** | WCAG 1.4.2 (AA) | Autoplay only without sound, with a visible stop control |
| **Volume control** | BBC guideline | Independent control, separate from system volume |

---

## 2. Error Management Protocol (Handoff Spec)

### HTML Reference Pattern (BBC + WebAIM)

```html
<!-- 1. aria-live region for error announcements (must exist at page load) -->
<div aria-live="polite" id="form-errors" class="sr-only"></div>

<!-- 2. Form -->
<form>
  <div class="field-group">
    <label for="email">Email <span aria-hidden="true">*</span></label>
    <!-- aria-required announces "required" to screen readers -->
    <input
      id="email"
      type="email"
      aria-required="true"
      aria-invalid="true"
      aria-describedby="email-error"
    >
    <!-- Inline error message -->
    <span id="email-error" role="alert">
      Error: Enter a valid email address (e.g. name@domain.com)
    </span>
  </div>
</form>
```

### Validation Protocol (3 mandatory steps)
1. **Announce:** Error visible + announced by screen reader (`aria-live` region or `role="alert"`).
2. **Identify:** Indicate specifically which field needs correction (`aria-invalid="true"` + inline message).
3. **Allow correction:** Move focus to the first invalid field + keep the form data intact.

---

## 3. Design Inspection Checklist V6.0 (Design Ops)

### Visual & Contrast
- [ ] **Normal text:** Contrast ≥ **4.5:1** (WCAG 1.4.3 AA)
- [ ] **Large text** (≥ 18pt or ≥ 14pt bold): Contrast ≥ **3:1**
- [ ] **UI components** (input borders, functional icons): Contrast ≥ **3:1** (WCAG 1.4.11)
- [ ] **Gradients and background images:** Text on gradient tested at the lowest-contrast pixel
- [ ] **Colour as sole differentiator:** Combined with icon, shape or underline (links, errors, states)

### Target Size & Focus
- [ ] **Primary buttons/links:** Minimum **44×44 px** (recommended for all)
- [ ] **Absolute minimum (WCAG 2.5.8):** 24×24 px with 24 px free space around
- [ ] **Native iOS:** 44×44 pt | **Android:** 48×48 dp with 8 dp between controls | **BBC Mobile:** 7×7 mm physical minimum
- [ ] **Focus ring:** Visible with ≥ 3:1 contrast against the adjacent background
- [ ] **Focus ring:** Do not use `outline: none` without an equivalent visible substitute
- [ ] **Adjacent links to the same destination:** Merged into a single touch target

### Structure & Navigation
- [ ] **Skip link:** "Skip to main content" visible at the top of the page
- [ ] **Landmarks:** `<main>`, `<nav>`, `<header>`, `<footer>` defined
- [ ] **Multiple navs:** Differentiated by `aria-label`
- [ ] **Heading hierarchy H1→H2→H3:** Preserved without skipping levels
- [ ] **Page/screen title:** Unique and descriptive per page
- [ ] **Tab order:** Documented and follows logical visual flow (left→right, top→bottom)
- [ ] **Positive tabindex (>0):** Never used — destroys natural order

### Forms & Inputs
- [ ] **Labels:** Programmatically associated with every input (`<label for>` or `aria-labelledby`)
- [ ] **Required fields:** `required` or `aria-required="true"` (not just a visual asterisk)
- [ ] **Errors:** `aria-invalid="true"` + `aria-describedby` pointing to the error message
- [ ] **Error message:** Visible + announced by SR + specifies what to correct
- [ ] **Autocomplete:** Attribute defined for personal data fields (WCAG 1.3.5)
- [ ] **Correct input type:** `type="email"`, `type="tel"`, `type="number"` where applicable

### ARIA & Rich Components
- [ ] **Alt text:** Defined for all functional images (null `alt=""` for decorative)
- [ ] **Icon-only buttons:** `aria-label` defined
- [ ] **Modals:** `role="dialog"`, `aria-modal="true"`, `aria-labelledby`, focus trap, focus returned on close
- [ ] **Tabs:** `role="tablist"`, `role="tab"`, `aria-selected`, `role="tabpanel"` properly structured
- [ ] **Accordions:** `aria-expanded` updated on toggle
- [ ] **Live regions:** `aria-live` defined at page load for dynamic regions
- [ ] **Critical errors:** `role="alert"` or `aria-live="assertive"` (use sparingly)

### Media
- [ ] **Videos:** Captions (VTT/SRT) specified
- [ ] **Audio:** Transcript available
- [ ] **Audio description:** Specified if there is visual information not covered by narration
- [ ] **Player controls:** All keyboard accessible + labelled

---

## Navigation
- [SKILL.md](SKILL.md) - Skills Framework.
- [RESOURCES.md](RESOURCES.md) - Technical Library.
