# Accessibility Knowledge Base & Resources (V6.0)
**The Knowledge Ecosystem for Product Teams**

> Updated 2026-05-14 with resources extracted from WebAIM, BBC, W3C APG, Deque axe and WCAG 2.2.

---

## 1. Technical Implementation References

### International (Gold Standard)

- **[W3C ARIA APG — Patterns](https://www.w3.org/WAI/ARIA/apg/patterns/):** ⭐ **Primary technical reference.** Defines the complete behaviour of each component (Roles, States, Keyboard Interaction).
- **[W3C APG — Dialog (Modal)](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/):** Full specification for focus behaviour, keyboard interaction and ARIA for modals.
- **[WCAG 2.2 Quick Reference](https://www.w3.org/WAI/WCAG22/quickref/):** Official success criteria guide. Includes WCAG 2.5.8 (Target Size AA) and 2.4.11 (Focus Appearance).
- **[WebAIM — Keyboard Accessibility](https://webaim.org/techniques/keyboard/):** Complete keyboard pattern table by component type. Reference for manual testing.
- **[WebAIM — Alternative Text](https://webaim.org/techniques/alttext/):** Decision framework for alt text with examples covering functional, decorative, complex images, CSS images and logos.
- **[WebAIM — Form Validation & Error Recovery](https://webaim.org/techniques/formvalidation/):** Three approaches for accessible error management (Error alert + focus, Errors on top, Inline errors).
- **[WebAIM — Accessible Forms](https://webaim.org/techniques/forms/controls):** Labels, fieldsets, required, aria-invalid, autocomplete. Complete reference by control type.
- **[WebAIM — Introduction to ARIA](https://webaim.org/techniques/aria/):** 5 Rules of ARIA Use, Landmark Roles, Live Regions, Labels and Descriptions.
- **[WAVE (WebAIM)](https://wave.webaim.org/):** Automated accessibility evaluation tool. Detects structural errors, contrast issues, missing labels, etc.
- **[Deque axe Platform](https://www.deque.com/axe/):** Full suite of automated testing tools. Integrates with browser DevTools, CI/CD pipelines and design workflows.

### BBC Accessibility Guidelines

- **[BBC Mobile Accessibility Guidelines](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/):** BBC mobile accessibility guidelines covering: Target Touch Size, Colour Contrast, Error Messages, Focus Management, Form Controls, Audio & Video. Open Government Licence — free to use.
- **[BBC — Target Touch Size](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/target-touch-size/):** Minimum: 7×7 mm physical. iOS: 44×44 pt. Android: 48×48 dp.
- **[BBC — Colour Contrast](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/colour-contrast/):** Minimum 4.5:1 WCAG AA for normal text. Includes testing procedure with eyedropper.
- **[BBC — Error Messages and Correction](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/error-messages-and-correction/):** BBC protocol: aria-live region + aria-invalid + aria-describedby + focus on first invalid field.
- **[BBC GEL — Design for Touch](http://www.bbc.co.uk/gel/guidelines/how-to-design-for-touch):** Visual and touch interaction guidelines.

### Platform-Specific References

- **[Apple iOS Human Interface Guidelines — Accessibility](https://developer.apple.com/documentation/accessibility):** iOS/macOS accessibility requirements. Target size: 44×44 pt.
- **[Android Material Design — Touch Target Size](https://material.io/guidelines/layout/metrics-keylines.html#metrics-keylines-touch-target-size):** Requirement: 48×48 dp with 8 dp between controls.
- **[appt.org — Guide for Making Apps Accessible](https://appt.org/en/):** Practical guide focused on mobile applications.

---

## 2. Accessibility Engineering Glossary

| Term | Technical Definition |
| :--- | :--- |
| **Focus Trapping** | Mechanism that prevents focus from leaving a Modal until it is closed. Implemented via a Tab/Shift+Tab loop within the container. |
| **Live Regions** | Areas that notify screen readers about dynamic changes (`aria-live`). Values: `off`, `polite`, `assertive`. Must be defined at page load. |
| **Role** | The semantic purpose of an element (e.g. `button`, `tab`, `dialog`, `alert`). Defines how assistive technology interprets the element. |
| **States & Properties** | Dynamic attributes (`aria-checked`, `aria-expanded`, `aria-invalid`) and relational attributes (`aria-labelledby`, `aria-describedby`). |
| **AOM** | *Accessibility Object Model*. The interface representation exposed to assistive technologies — the "accessibility shadow DOM". |
| **APCA** | *Accessible Perceptual Contrast Algorithm*. Perceptual contrast algorithm — the future standard in WCAG 3.0. More accurate than the current WCAG ratio. |
| **Focus Indicator** | Visual indicator of the keyboard-focused element. Must have minimum 3:1 contrast against the adjacent background (WCAG 2.4.11 AA in WCAG 2.2). Never use `outline: none` without a substitute. |
| **Target Size** | Minimum clickable/tappable area. WCAG 2.5.8 (AA): 24×24 px with 24 px free space around. Best practice: 44×44 px (iOS HIG, WebAIM). |
| **Accessible Name** | The name screen readers announce to identify an element. Computed by: `aria-labelledby` > `aria-label` > `<label>` > `alt` attribute > element text. |
| **Focus Management** | Practice of programmatically controlling where focus moves after events (modal open, form submission, navigation). Critical for keyboard UX. |
| **aria-modal** | Attribute that informs AT that content outside the dialog is inert. Only use when code *actually* prevents outside interaction and CSS obscures the background. |
| **aria-invalid** | State indicating an invalid form field. Announced as "invalid" by screen readers. Has no visual impact — requires additional CSS. |
| **aria-required** | Indicates a required field. Functional equivalent of HTML `required`. Screen readers announce "required". |
| **aria-describedby** | Associates secondary description content with an element (e.g. inline error message, format instructions). Read after the label. |
| **tabindex="0"** | Makes an element keyboard-focusable in natural document order. Use only on custom interactive widgets. |
| **tabindex="-1"** | Makes an element focusable only programmatically (JavaScript `focus()`). Useful for error messages and content areas needing initial focus. |
| **Positive tabindex** | `tabindex="1"` or higher — **never use**. Destroys natural navigation order and creates hard-to-maintain focus bugs. |
| **Jump Menu** | `<select>` with `onChange` that triggers navigation. **Anti-pattern** — arrowing through options fires accidental actions. Replace with `<select>` + a submit button. |
| **Skip Link** | "Skip to main content" link at the top of the page. Allows keyboard users to bypass navigation menus. |
| **WCAG 2.5.8** | Target Size (Minimum) — new AA criterion in WCAG 2.2. Minimum: 24×24 px with 24 px free space around, or 44×44 px without spacing constraint. |
| **WCAG 2.4.11** | Focus Appearance — new AA criterion in WCAG 2.2. Focus indicator with minimum area and 3:1 contrast. |

---

## 3. QA & Testing Toolkit

### Automated Tools
- **[WAVE](https://wave.webaim.org/):** Browser extension + API. Detects errors, alerts and structure. Visual and intuitive.
- **[axe DevTools (Deque)](https://www.deque.com/axe/):** Chrome/Firefox extension. Integrates with Cypress, Jest, Storybook. Rules based on WCAG 2.2.
- **[WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker):** Web tool for quick contrast ratio verification.

### Desktop Tools
- **TPG Colour Contrast Analyser:** Desktop app with eyedropper to measure contrast on any screen (ideal for gradients and images).
- **NVDA** (free): Windows screen reader. Pair with Firefox for reference testing.
- **JAWS** (licence): Windows screen reader. Pair with Chrome.

### Mobile Tools
- **VoiceOver (iOS/macOS):** Native Apple screen reader. Essential for Apple application testing.
- **TalkBack (Android):** Native Android screen reader.
- **Android Accessibility Scanner:** Target size and contrast audit on Android.

---

## 4. WCAG 2.2 Key Success Criteria Reference

| SC | Name | Level | Design Relevance |
| :--- | :--- | :--- | :--- |
| 1.1.1 | Non-text Content | A | Alt text on images |
| 1.2.2 | Captions (Prerecorded) | A | Captions on video |
| 1.2.5 | Audio Description | AA | Audio description |
| 1.3.1 | Info and Relationships | A | Semantic HTML, landmarks, tables |
| 1.3.5 | Identify Input Purpose | AA | Autocomplete on forms |
| 1.4.3 | Contrast (Minimum) | AA | 4.5:1 text, 3:1 large text |
| 1.4.6 | Contrast (Enhanced) | AAA | 7:1 text, 4.5:1 large text |
| 1.4.11 | Non-text Contrast | AA | 3:1 for UI borders and icons |
| 2.1.1 | Keyboard | A | Everything operable by keyboard |
| 2.4.1 | Bypass Blocks | A | Skip links |
| 2.4.6 | Headings and Labels | AA | Descriptive headings |
| 2.4.7 | Focus Visible | AA | Visible focus indicator |
| 2.4.11 | Focus Appearance | AA *(WCAG 2.2 new)* | Focus ring with 3:1 contrast and minimum area |
| 2.5.8 | Target Size (Minimum) | AA *(WCAG 2.2 new)* | 24×24 px with 24 px free space around |
| 3.3.1 | Error Identification | A | Identify fields with errors |
| 3.3.2 | Labels or Instructions | A | Labels and completion instructions |
| 3.3.3 | Error Suggestion | AA | Suggest how to correct the error |
| 4.1.2 | Name, Role, Value | A | Correct ARIA on custom components |
| 4.1.3 | Status Messages | AA | `role="status"` / `aria-live` for status messages |

---

## Navigation
- [SKILL.md](SKILL.md) - Skills Framework.
- [HANDOFF.md](HANDOFF.md) - Engineering Specifications.
