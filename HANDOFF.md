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

## 3. Design Inspection Checklist V7.0 (Design Ops - Figma Focus)

### Visual & Contrast
- [ ] **Normal text:** Contrast ratio ≥ **4.5:1** checked (WCAG 1.4.3 AA).
- [ ] **Large text** (≥ 18pt or ≥ 14pt bold): Contrast ratio ≥ **3:1** checked.
- [ ] **UI components** (borders, interactive states, functional icons): Contrast ≥ **3:1** checked (WCAG 1.4.11).
- [ ] **Backgrounds:** Visual text on gradients or image background areas overlay-protected and tested at lowest contrast pixel.
- [ ] **Beyond Color:** Visual states, status cues, links, and errors differentiated by underlines, icons, or shapes, not just color.

### Target Size & Focus State
- [ ] **Primary touch targets:** Minimum visual layout size of **44×44px** designed (web & iOS best practice).
- [ ] **Spacing margins:** At least 8dp spacing on Android components or 1px on iOS to prevent miss-touches (WCAG 2.5.8).
- [ ] **Focus indicator:** High-contrast focus state (:focus visual outline) designed and verified for all interactive controls (minimum 3:1 contrast ratio).
- [ ] **Adjacent targets:** Dual controls pointing to the exact same landing page combined visually into a single larger touch container.

### Structure & Conceptual Navigation
- [ ] **Visual Hierarchy:** Typographic heading hierarchy mapped clearly (concept H1 → H2 → H3 layout).
- [ ] **Section Landmarks:** Clear visual segregation of header, navigation menus, main content, and footer regions.
- [ ] **Reading Flow:** Conceptual L-to-R or visual grid sequence verified for natural scanning flow.
- [ ] **Keyboard Skip Flow:** Visual and functional bypass action planned for keyboard/switch-control users at the top of the interface.
- [ ] **Page Title:** Unique and descriptive visual page/screen label defined.

### Form & State Mockups
- [ ] **Field Labels:** Persistent visual label designed for every single input field (do not hide instruction details purely inside placeholders).
- [ ] **Field States:** Hover, Focus, Disabled, Active, and Error visual states fully designed.
- [ ] **Inline Error Layout:** Error message mockups designed close to their specific inputs, using descriptive help copy and supporting warning icons.
- [ ] **Radio/Checkbox Groups:** Wrapped visually under clear, concise group headers.

### Conceptual ARIA & Interaction Behavior
- [ ] **Alternative Text Spec:** Design notes include suggested alt descriptions for informative illustrations/graphics, and explicitly marks decorative assets.
- [ ] **Icon-Only Labels:** Annotations explicitly state accessible descriptive names for icon-only actions (such as carousel arrows prev/next).
- [ ] **Modal Behaviors:** Dimmed backdrop overlay designed and closed buttons clearly highlighted. Focus flow order annotated.
- [ ] **Rich Widgets:** Complex tab-changes, auto-advancing carousels, and sliding accordions have clearly planned interactive states and visual play/pause controls.

### Media Design
- [ ] **Video overlays:** Graphic captions overlay layout designed for video elements.
- [ ] **Transcripts:** Visual entry points (buttons/links) for complete audio/video text transcripts planned.
- [ ] **Autoplay:** Visual toggle/pause action designed for any auto-advancing media assets.

---

## Navigation
- [SKILL.md](SKILL.md) - Skills Framework.
- [RESOURCES.md](RESOURCES.md) - Technical Library.
