# Accessibility Skills Taxonomy (V6.0 — Crawled Edition)
**Governance Framework for Design Ops & Product**

> Updated 2026-05-14 with data extracted from: WebAIM, BBC Mobile Accessibility Guidelines, W3C APG and Deque axe.
> This framework maps technical competencies to **WCAG 2.1/2.2** and **W3C ARIA Patterns**.

---

## 1. Strategic Compliance Mapping

| Category | Core Skill | WCAG Level | Product Impact | Technical Ref. |
| :--- | :--- | :--- | :--- | :--- |
| **Structure** | 1. Hierarchy, Semantics & Landmarks | **A / AA** | Structural Navigation & SEO | WCAG 1.3.1, 2.4.1, 2.4.6 |
| **Structure** | 2. Lists and Data Tables | **A** | Data Integrity | WCAG 1.3.1 |
| **Design** | 3. Contrast and Discernibility | **AA / AAA** | Universal Readability | WCAG 1.4.3, 1.4.6, 1.4.11 |
| **Interaction** | 4. Focus & Target Size | **A / AA** | Keyboard & Touch Precision | WCAG 2.1.1, 2.4.7, 2.5.8 |
| **Forms** | 5. Input & Error Management | **A / AA** | Conversion & UX | WCAG 3.3.1, 3.3.2, 3.3.3, 1.3.5 |
| **Content** | 6. Alternative Text | **A** | Visual Inclusion | WCAG 1.1.1 |
| **Content** | 7. Accessible Media (Video/Audio) | **A / AA** | Sensory Inclusion | WCAG 1.2.2, 1.2.5 |
| **Dynamic** | 8. ARIA & Rich Components | **A / AA** | Interface Robustness | W3C ARIA APG |

---

## 2. Detailed Competencies

### 1. Hierarchy, Semantics & Landmarks

- **Why it matters:** Defines the Accessibility Tree. Enables users to understand structure and jump between sections. Screen readers announce landmarks and allow shortcut navigation.
- **Technical Acceptance Criteria:**
  - **Required landmarks:** `<main>`, `<nav>`, `<header>`, `<aside>`, `<footer>`. Use `<form role="search">` for search regions.
  - **Multiple regions of the same type** must have `aria-label` or `aria-labelledby` to differentiate them (e.g. two `<nav aria-label="Main navigation">`).
  - **Generic region:** Use `role="region"` + `aria-label` for significant sections without a native landmark.
  - **Headings:** Logical hierarchy H1 → H2 → H3. Never skip levels for aesthetic reasons. Every page/screen must have a unique, descriptive `<title>`.
  - **Skip Links:** "Skip to main content" link visible at the top of the page (BBC & WebAIM requirement).
  - **Reading order:** Source code must reflect logical visual order (left→right, top→bottom).
  - **ARIA Rule #1 (WebAIM):** Always prefer native HTML elements. Use ARIA only when HTML is insufficient.
  - **ARIA Rule #2:** Do not override native semantics unnecessarily (e.g. `<ul role="navigation">` destroys list benefits).
- **How to Test:** Audit landmarks with a screen reader (NVDA/JAWS/VoiceOver). Check heading outline. Navigate using only `Tab` and `Shift+Tab`.

---

### 2. Lists and Data Tables

- **Why it matters:** Communicates the number of items and relationships between complex data. Screen readers announce "List, X items".
- **Technical Acceptance Criteria:**
  - **Lists:** Use `<ul>`/`<ol>` for groups of 2+ items. Never use CSS `list-style: none` on functional lists without a role.
  - **Tables:** Use `<th scope="col|row">` and `<caption>`. Never use tables for layout.
  - **Grouping:** `<optgroup>` in selects — note: inconsistent screen reader support (WebAIM). Do not rely on it for critical context.
  - **Complex tables:** Use `aria-labelledby` to concatenate multiple headers.
- **How to Test:** Check screen reader announcements ("List, 5 items"). Navigate table cells with arrow keys.

---

### 3. Contrast and Discernibility

- **Why it matters:** Ensures readability under adverse conditions, for users with low vision, colour blindness, and in high-luminance environments.
- **Technical Acceptance Criteria (BBC + WCAG):**

| Content Type | Minimum Ratio (WCAG AA) | Enhanced Ratio (WCAG AAA) |
| :--- | :--- | :--- |
| Normal text (< 18pt / < 14pt bold) | **4.5:1** | 7:1 |
| Large text (≥ 18pt / ≥ 14pt bold) | **3:1** | 4.5:1 |
| UI components (input borders, functional icons) | **3:1** | — |
| Inline links differentiated by colour only | **3:1** vs. surrounding text | — |

  - **Gradients and images:** Apply a semi-transparent overlay or text-shadow. The measurement point is the lowest-contrast pixel in the text area (BBC guideline).
  - **Colour must not be the only differentiator** — always combine with icon, underline, or shape (links, errors, states).
  - **Text images:** Avoid. If necessary, apply 4.5:1.
  - **QA Tools:** WebAIM Contrast Checker, TPG Colour Contrast Analyser, axe DevTools.
- **How to Test:** Run Colour Contrast Analyser. Inspect colour via Dev Tools. Test with colour-blindness simulation.

---

### 4. Focus & Target Size

- **Why it matters:** Critical for keyboard and mobile users. Without a visible focus indicator, keyboard users are "lost" in the interface.
- **Technical Acceptance Criteria:**

#### Focus Indicator
  - **Prohibited:** `outline: 0` or `outline: none` on focusable elements without an equivalent visible substitute (WebAIM).
  - **Minimum standard:** The indicator must have at least **3:1** contrast against the adjacent background (WCAG 2.4.11 — AA in WCAG 2.2).
  - **Recommended:** Add a visible `background-color` or `border` in addition to the native outline.
  - **ARIA Rule #4:** Focusable elements (reachable via `Tab`) must never have `aria-hidden="true"`.

#### Target Size (BBC + WCAG 2.5.8)

| Platform | Minimum Size | Rule |
| :--- | :--- | :--- |
| **Web (WCAG 2.5.8 AA)** | **24×24 CSS px** with 24px free space around | Minimum criterion |
| **Web (best practice)** | **44×44 CSS px** | Recommended by WebAIM & iOS HIG |
| **iOS** | **44×44 pt** with at least 1px between targets | Apple HIG requirement |
| **Android** | **48×48 dp** with at least 8dp between controls | Material Design |
| **BBC Mobile (physical)** | **7×7 mm** minimum | Smallest average human finger |
| **BBC Mobile (recommended)** | **9–10 mm** | Full accessibility coverage |

  - **CSS reference:** `button { box-sizing: border-box; min-width: 44px; min-height: 44px; }`
  - **Grouping:** Adjacent links pointing to the same destination should be merged into a single touch target (BBC guideline).
  - **tabindex="0":** Makes an element keyboard-focusable (use only on custom interactive widgets).
  - **tabindex="-1":** Element focusable only programmatically (useful for error messages and dialogs).
- **How to Test:** Navigate exclusively with `Tab`. Audit touch targets in Figma (measure in px/pt/dp). Validate visible focus ring.

---

### 5. Input & Error Management

- **Why it matters:** Reduces friction and prevents task abandonment. Users with cognitive disabilities depend on clear messages and guided correction.
- **Technical Acceptance Criteria:**

#### Labeling
  - `<label for="id">` required and programmatically associated with every `<input>`.
  - Clicking the label must activate/focus the control (quick association test — WebAIM).
  - Groups of checkboxes/radio buttons: use `<fieldset>` + `<legend>` (legend should be brief).
  - **Never nest fieldsets** — causes unexpected behaviour in screen readers.

#### Required & Invalid
  - Use the `required` HTML attribute OR `aria-required="true"` — screen readers announce "required".
  - **Note:** An asterisk (*) as the only indicator of a required field is insufficient.
  - After validation with errors: apply `aria-invalid="true"` to the field.
  - Use `aria-describedby` to associate the field with its inline error message.
  - Use `autocomplete` on fields with a known input purpose (WCAG 1.3.5).

#### Error Recovery (3 approaches — WebAIM)
1. **Error alert, then focus:** Accessible custom modal → announce error → focus on the problematic field.
2. **Errors on top:** Error summary above the form with `focus()`. List all errors with links to each field.
3. **Inline errors:** Messages in the context of each field via `aria-describedby`. Focus on the first invalid field.
4. **Errors on top + Inline (combined):** Most robust approach (WebAIM best practice).

#### BBC Error Protocol
  - On form submit: use an `aria-live` region with a list of invalid fields above the form.
  - Move focus to the error message on submit.
  - `aria-invalid="true"` + `aria-describedby` + inline visual cues on each invalid field.

#### Input Types
  - Use `<input type="tel">`, `type="email"`, `type="number">`, `type="date">` — triggers correct mobile keyboard and native browser validation.
  - **Avoid** multi-select `<select>` — inconsistent keyboard support across browsers. Prefer a group of checkboxes.
  - **Avoid reset buttons** — easy to trigger accidentally (WebAIM citing NN/g).

- **How to Test:** Submit the form with incorrect data. Check whether errors are announced. Test with keyboard only.

---

### 6. Alternative Text

- **Why it matters:** Delivers image value to screen reader users. Impacts SEO. Critical when images fail to load.
- **Technical Acceptance Criteria (WebAIM Alt Text Framework):**

| Image Type | `alt` Attribute | Criterion |
| :--- | :--- | :--- |
| Informative image | Descriptive and concise | e.g. `alt="Astronaut Ellen Ochoa"` |
| Decorative image | `alt=""` (null) | Never omit the attribute |
| Linked image (sole link content) | Describe the function/destination | e.g. `alt="View Ellen Ochoa's profile"` |
| Redundant image (content already in adjacent text) | `alt=""` (null) | Avoid redundancy |
| Image button (`<input type="image">`) | Describe the action | e.g. `alt="Submit search"` |
| Complex image (chart, map) | Brief alt + link to detailed description | — |
| Logo linked to homepage | Company name | e.g. `alt="Acme Company"` |
| CSS/background image | Do not use for informative content | — |

  - Do not include "image of…" or "graphic of…" (screen readers already announce "graphic").
  - `longdesc` is **deprecated** — do not use.
- **How to Test:** Disable images and verify the page context remains clear.

---

### 7. Accessible Media (Video & Audio)

- **Why it matters:** Inclusion for deaf, blind, and hearing/visually impaired users.
- **Technical Acceptance Criteria:**

| Requirement | WCAG Level | Application |
| :--- | :--- | :--- |
| **Synchronised captions** | **A (WCAG 1.2.2)** | All relevant audio in pre-recorded video |
| **Text transcripts** | **A (WCAG 1.2.1)** | Podcasts and standalone audio |
| **Audio description** | **AA (WCAG 1.2.5)** | Visual actions not covered by narration |
| **Accessible player controls** | **A (WCAG 4.1.2)** | All controls operable by keyboard + `aria-label` |
| **No autoplay with audio** | **AA (WCAG 1.4.2)** | Autoplay only without sound, with a visible stop control |

  - **Caption format:** Provide SRT or VTT files.
  - **Autoplay (BBC rule):** Video may autoplay without sound if a visible pause/stop control is present.
- **How to Test:** Watch without sound → validate captions. Test all player controls with keyboard.

---

### 8. ARIA & Rich Components (W3C APG)

- **Why it matters:** Communicates states and behaviours for complex UIs (SPAs). ARIA extends HTML's semantic vocabulary for cases not natively covered.
- **5 Core Rules of ARIA Use (WebAIM):**
  1. **Rule #1:** If a native HTML element exists, use it. Use ARIA only when HTML is insufficient.
  2. **Rule #2:** Do not change native semantics.
  3. **Rule #3:** All interactive ARIA controls must be keyboard operable.
  4. **Rule #4:** Interactive controls must not be hidden (`aria-hidden="true"` on focusable elements is prohibited).
  5. **Rule #5:** Every interactive element must have a descriptive accessible name.

- **Criteria by Component:**

#### Button
  - **ARIA:** `role="button"`. Toggles: `aria-pressed="true/false"`. With popup: `aria-haspopup`.
  - **Disclosure/Accordion:** `aria-expanded="true|false"`.
  - **Keyboard:** `Enter` or `Space` activates.
  - **Handoff:** `aria-label` required for icon-only buttons.

#### Modal Dialog (W3C APG)
  - **ARIA:** `role="dialog"`, `aria-modal="true"`, `aria-labelledby`.
  - **Keyboard:** `Tab` loops within the dialog. `Shift+Tab` navigates in reverse. `Escape` closes.
  - **Focus placement:** First focusable element (simple content) / `tabindex="-1"` on a static element (complex content).
  - On close: focus **returns to the trigger element**.

#### Tabs
  - **ARIA:** `role="tablist"`, `role="tab"`, `aria-selected="true/false"`, `role="tabpanel"`.
  - **Keyboard:** `←` `→` navigates and activates tabs. `Tab` enters/exits the group.

#### Accordion
  - **ARIA:** Header as `<button aria-expanded="true|false" aria-controls="panel-id">`.
  - **Keyboard:** `Space/Enter` → toggle. `↑` `↓` → navigate between headers.

#### Slider
  - **Keyboard:** `←` `→` increments/decrements. `Home/End` → min/max. `PageUp/Down` → larger step.

#### Live Regions (Dynamic Content)
  - `aria-live="polite"`: Announce after a pause. Use for: status, notifications.
  - `aria-live="assertive"`: Announce immediately. Use **only for critical errors**.
  - `role="alert"`: Equivalent to `aria-live="assertive"`.
  - **BBC rule:** `aria-live` must be defined at page load — injecting it later via JS is unreliable.

#### ARIA Labelling (Priority Hierarchy)
  - `aria-labelledby` > `aria-label` > `<label>` > `alt` > element text content.
  - `aria-label` overrides visual labels — ensure the accessible name includes the visible text (WCAG 2.5.3).
  - `aria-describedby` → read after the label — use for instructions and inline error messages.

- **How to Test:** Screen reader validation (NVDA/JAWS/VoiceOver). axe DevTools. WAVE.

---

## 3. QA Tools Reference

| Tool | Type | Primary Use |
| :--- | :--- | :--- |
| **[WAVE (WebAIM)](https://wave.webaim.org/)** | Browser extension / API | Automated detection: errors, alerts, structure |
| **[axe DevTools (Deque)](https://www.deque.com/axe/)** | Browser extension / CI-CD | Automated tests, pipeline integration |
| **[WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker)** | Web tool | Contrast ratio verification |
| **TPG Colour Contrast Analyser** | Desktop app | Eyedropper contrast analysis on any screen |
| **NVDA / JAWS** | Screen reader (Windows) | Real announcement testing |
| **VoiceOver** | Screen reader (iOS/macOS) | Real mobile/desktop Apple testing |
| **Android Accessibility Scanner** | Mobile app | Target size and contrast audit on Android |

---

## Navigation
- [RESOURCES.md](RESOURCES.md) - Reference Library.
- [HANDOFF.md](HANDOFF.md) - Engineering Specifications.
