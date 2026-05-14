# Accessibility Skills Taxonomy (V7.0)
**Governance Framework for Design Ops & Product**

> Updated 2026-05-14. Sources: WebAIM, BBC Mobile Accessibility Guidelines, W3C APG, Deque axe, React Aria, Radix UI, WCAG 3 / Silver direction.
> This framework maps technical competencies to **WCAG 2.1/2.2**, **W3C ARIA Patterns**, and **modern frontend accessibility**.

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
| **Framework** | 9. Modern Framework Accessibility | **A / AA** | SPA & SSR Robustness | React Aria, Radix UI, Next.js |
| **Mobile** | 10. Mobile Assistive Technology & Gestures | **A / AA** | Native Mobile Inclusion | iOS VoiceOver, Android TalkBack |
| **Quality** | 11. Automated Testing & CI/CD | **A / AA** | Regression Prevention | axe-core, Playwright, Storybook |
| **Performance** | 12. Performance & Motion Accessibility | **AA** | Cognitive & Visual Comfort | WCAG 2.3.3, prefers-reduced-motion |
| **Cognitive** | 13. Cognitive Accessibility | **A / AA / AAA** | Inclusive UX for all | WCAG 3.3.x, COGA, Silver |

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

### 9. Modern Framework Accessibility

- **Why it matters:** SPAs and SSR frameworks break native browser behaviours — page transitions don't reload, focus is not restored, and dynamic content is not announced without explicit management.
- **Technical Acceptance Criteria:**

#### React / Next.js
  - **Hydration:** Ensure server-rendered ARIA attributes match client-rendered output — hydration mismatches can silently remove roles and states.
  - **Route transitions (Next.js App Router):** On navigation, move focus to the `<h1>` or a `tabindex="-1"` landmark. Use `aria-live="polite"` region to announce the new page title.
  - **Suspense / loading states:** Wrap loading boundaries with a visible loading indicator + `aria-busy="true"` on the container. Provide a `role="status"` message (e.g. "Loading results…").
  - **Server Components:** Server components render no event handlers — interactive controls (buttons, links) must be Client Components. Do not place `onClick` on Server Component elements.
  - **Virtualized lists (react-window, TanStack Virtual):** Expose total item count via `aria-setsize` and current position via `aria-posinset`. Announce loaded chunks via a polite live region.
  - **Portals:** Elements rendered in a portal (e.g. modals, tooltips) must still follow focus management rules. The portal root must be outside `aria-hidden` containers.

#### Headless UI Libraries
  - **React Aria (Adobe):** Preferred for production — implements W3C APG keyboard patterns out of the box. Provides hooks (`useButton`, `useDialog`, `useListBox`) that handle ARIA states automatically.
  - **Radix UI:** Accessible primitives with correct roles and keyboard behaviour. Customise visuals freely; do not override data-state or role attributes.
  - **Headless UI (Tailwind Labs):** Correct ARIA for Dialog, Listbox, Combobox. Always provide visible labels — the library does not generate them automatically.
  - **Rule:** Prefer headless libraries over building custom ARIA widgets from scratch. Verify keyboard behaviour with a screen reader after integrating — library defaults may not cover all edge cases.

#### SPA Navigation Announcements
  - Inject a visually-hidden `aria-live="polite"` region at root level (present from initial load).
  - On each route change, update its text content with the new page title (e.g. `"Dashboard — App Name"`).
  - Delay the announcement by ~100ms to allow DOM rendering to settle before the SR reads.
  - Libraries: `@reach/skip-nav`, `next-a11y`, or a custom `useRouteAnnouncer` hook.

- **How to Test:** Navigate between routes with VoiceOver/NVDA active. Verify page title is announced. Tab through the new page — confirm focus is not stranded on a removed element.

---

### 10. Mobile Assistive Technology & Gestures

- **Why it matters:** Touch-first interfaces require gesture-aware design. Screen reader gestures on mobile differ fundamentally from keyboard navigation on desktop.

#### iOS VoiceOver
  - **Rotor navigation:** Users activate the rotor (two-finger twist) to switch navigation mode (Headings, Links, Form Controls, Landmarks, etc.). Ensure heading hierarchy and landmark structure are correct — rotor usability depends entirely on semantic markup.
  - **Swipe order:** VoiceOver reads elements in DOM order, not visual order. CSS `order`, `flex-direction: row-reverse`, and `position: absolute` can create a mismatch between visual and swipe order. Always verify swipe sequence with VoiceOver active.
  - **Semantic grouping:** Use `accessibilityElements` equivalent in web: group related elements with `role="group"` + `aria-label` so VoiceOver announces them as a unit instead of reading each child separately.
  - **iOS Accessibility Tree nuances:** `display: contents` can drop elements from the iOS AT tree. `visibility: hidden` hides from VoiceOver; `opacity: 0` does not. Test both.
  - **Custom actions:** For complex gestures (drag-to-reorder, swipe-to-delete), provide alternative accessible actions via `aria-roledescription` + keyboard/button equivalents.

#### Android TalkBack
  - **Gesture set:** Swipe right/left → next/previous element. Double-tap → activate. Two-finger swipe → scroll. Swipe up then right → activate first item in linear navigation.
  - **Explore by touch:** TalkBack reads elements on finger hover. Ensure touch targets are large enough (48×48 dp) and that decorative elements are hidden (`importantForAccessibility="no"` equivalent: `aria-hidden="true"`).
  - **Linear navigation vs. reading order:** TalkBack uses DOM order for linear navigation. Same swipe-order rule as VoiceOver applies.
  - **Grouping:** Use `role="group"` + `aria-label` to group related controls (e.g. a card with image + title + button) into a single focusable unit — reduces swipe count.

#### Mobile-First Semantic Grouping
  - Design cards as a single focusable unit with a descriptive label rather than 3–5 separate focusable elements (image, title, button, price, tag).
  - Pattern: wrapper element with `role="article"` or `role="listitem"`, a single `aria-label` describing the card's purpose, and inner interactive elements only focusable when the user explicitly enters the group.

- **How to Test:** Enable VoiceOver (iOS: Settings → Accessibility → VoiceOver). Swipe through the entire screen. Enable TalkBack (Android: Settings → Accessibility → TalkBack). Verify swipe order matches visual intent.

---

### 11. Automated Testing & CI/CD

- **Why it matters:** Manual audits find ~30% of issues. Automated testing catches the remaining systematic errors continuously, not just at release time.
- **Technical Acceptance Criteria:**

#### Tool Stack
  - **axe-core:** Rule engine used by axe DevTools, Playwright, Jest-axe, Cypress-axe. Run as the baseline for all automated checks.
  - **jest-axe / @testing-library + axe:** Unit-level accessibility assertions on component render. Add to every component test file.
  - **Cypress-axe:** Integration-level checks on rendered pages. Run `cy.checkA11y()` after key user interactions.
  - **Playwright + axe:** E2E accessibility snapshots across full user flows (login → dashboard → form submit). Export results as JSON for diffing.
  - **Storybook a11y addon (`@storybook/addon-a11y`):** Runs axe on every Story in the browser. Add to CI so Stories fail on axe violations.
  - **eslint-plugin-jsx-a11y:** Static analysis for common ARIA mistakes in JSX at development time. Required in `.eslintrc`.

#### CI/CD Pipeline
  ```
  PR opened → eslint-plugin-jsx-a11y (lint) → jest-axe (unit) → Storybook a11y (component) → Playwright axe (E2E) → PR blocked if violations
  ```
  - Set axe `runOnly` to `wcag2a`, `wcag2aa`, `wcag21aa`, `best-practice`.
  - Configure `disableRules` only with documented justification + issue tracker link.
  - Export axe results to a JSON artefact per build — diff against the previous build to detect regressions.

#### Accessibility Budget
  - Define a maximum allowed violation count per severity level (critical, serious, moderate, minor).
  - **Critical / Serious:** Zero tolerance — blocks merge.
  - **Moderate / Minor:** Tracked and resolved within agreed sprint. Never increase week-over-week.
  - Document the budget in `a11y.config.json` at the repo root.

#### Regression Testing
  - Playwright accessibility snapshots: capture the full axe results tree and commit as a snapshot. CI fails if new violations appear.
  - Screen reader smoke test (manual, quarterly): NVDA + Chrome and VoiceOver + Safari — run the top 5 user flows.

- **How to Test:** Run `npx axe-cli <url>` for a quick CLI audit. Check CI pipeline logs for axe violations on each PR.

---

### 12. Performance & Motion Accessibility

- **Why it matters:** Performance degradation creates accessibility barriers — lost focus on re-render, inaccessible skeleton states, and CLS disorienting keyboard users. Motion can trigger vestibular disorders.
- **Technical Acceptance Criteria:**

#### Motion & Animation
  - **`prefers-reduced-motion`:** All non-essential animations must be disabled or simplified when this media query is active.
    ```css
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: 0.01ms !important;
        transition-duration: 0.01ms !important;
      }
    }
    ```
  - **`prefers-reduced-transparency`:** Reduce or eliminate blur/glassmorphism effects.
  - **Parallax:** Always off when `prefers-reduced-motion: reduce`. Parallax is a known vestibular disorder trigger.
  - **Carousels / auto-advancing content:** Must have pause control (WCAG 2.2.2). Default: paused or speed ≤ 5 seconds between transitions.
  - **Animation interruption:** Any animation that persists > 3 seconds must be stoppable, pausable, or hideable.
  - **WCAG 2.3.3 (AAA):** Animation from interactions can be disabled. Treat as a strong recommendation.

#### Skeleton Loading
  - Skeleton screens must have `aria-busy="true"` on their container and `aria-label="Loading…"`.
  - Do not use `aria-hidden="true"` on skeleton containers — screen readers will skip and receive no feedback.
  - When content loads, remove `aria-busy`, restore focus if needed, and optionally announce completion via `role="status"`.

#### CLS & Focus Loss
  - **Cumulative Layout Shift (CLS):** Layout shifts can move the focused element off-screen or cause focus to be lost. Reserve space for dynamic content (images, ads, embeds) to prevent CLS.
  - **Focus loss rule:** If the focused element is removed from the DOM (e.g. a drawer closes), focus must be explicitly moved — never allowed to fall back to `<body>`.
  - **Lazy loading:** Images loaded below the fold must have explicit `width` and `height` attributes to prevent CLS.

#### Cognitive Load & Performance
  - **Time limits (WCAG 2.2.1):** Any time-limited action must be extendable, adjustable, or turn-off-able.
  - **Interruptions (WCAG 2.2.4):** Notifications and alerts must be postponable (except emergencies).
  - **Re-authentication (WCAG 2.2.5):** On session expiry, preserve all user data entered.

- **How to Test:** Enable `prefers-reduced-motion` in OS settings (macOS: Accessibility → Display → Reduce Motion). Simulate CLS with Lighthouse. Audit `aria-busy` states during loading with screen reader active.

---

### 13. Cognitive Accessibility

- **Why it matters:** 1 in 6 people has a cognitive or learning disability. WCAG 2.x underserves this group — WCAG 3 / Silver and the COGA (Cognitive Accessibility Guidance) task force address this gap directly.
- **Technical Acceptance Criteria:**

#### Reading & Language
  - **Plain language:** Target a reading level appropriate for a 9-year-old (WCAG 3.1.5 AAA, COGA recommendation). Avoid jargon, acronyms, and passive voice.
  - **Reading complexity:** Use short sentences (< 20 words), short paragraphs (< 4 sentences), and descriptive link text.
  - **Text spacing (WCAG 1.4.12):** No loss of content when line-height ≥ 1.5×, letter-spacing ≥ 0.12em, word spacing ≥ 0.16em.
  - **`lang` attribute:** Required on `<html>` and on any inline text in a different language — screen readers use it to switch speech synthesiser voice.
  - **Dyslexia support:** Avoid justified text (creates uneven spacing). Prefer sans-serif fonts. Allow user font-size scaling without breaking layout (WCAG 1.4.4).

#### Cognitive Overload & Distraction
  - **Reduce visual noise:** Limit the number of actions available at once. Use progressive disclosure for complex forms.
  - **Consistent navigation (WCAG 3.2.3):** Navigation components must appear in the same order on every page.
  - **Consistent identification (WCAG 3.2.4):** Components with the same function must have the same label/name across pages.
  - **Distraction reduction:** Moving, blinking, or scrolling content that starts automatically must be stoppable (WCAG 2.2.2).
  - **Session timeouts:** Warn users at least 20 seconds before an authenticated session expires (WCAG 2.2.1).

#### Timing & Interruptions
  - **No time limits** on tasks unless absolutely necessary (security exceptions apply).
  - **`aria-live="assertive"`** use must be minimal — it interrupts the user's current cognitive task.
  - **Modals and popups** must not open without a user action — unexpected focus changes are highly disruptive for cognitive disabilities.
  - **Error prevention (WCAG 3.3.4):** For legal or financial submissions, provide review + confirm step, or allow reversal.

#### WCAG 3 / Silver Direction
  - WCAG 3 moves from pass/fail binary to a **scoring model** — components earn points across multiple conformance levels.
  - **COGA patterns** (precursor content for WCAG 3): chunking information, supporting memory, reducing cognitive barriers, providing reminders and feedback.
  - **Design implication:** Start building a cognitive accessibility layer now — plain language reviews, user testing with neurodiverse participants, and distraction audits.

- **How to Test:** Hemingway Editor for reading level. Manual review of navigation consistency across pages. User testing with participants who have cognitive disabilities or dyslexia.

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
| **[jest-axe](https://github.com/nickcolley/jest-axe)** | JS testing library | Unit-level axe assertions in Jest |
| **[cypress-axe](https://github.com/component-driven/cypress-axe)** | Cypress plugin | Integration axe checks |
| **[Playwright + axe-core](https://playwright.dev/docs/accessibility-testing)** | E2E testing | Full-flow accessibility snapshots |
| **[eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y)** | ESLint plugin | Static ARIA analysis in JSX |
| **[Storybook a11y addon](https://storybook.js.org/addons/@storybook/addon-a11y)** | Storybook | Per-Story axe checks in CI |
| **[Hemingway Editor](https://hemingwayapp.com/)** | Web tool | Reading level and plain language audit |

---

## Navigation
- [RESOURCES.md](RESOURCES.md) - Reference Library.
- [HANDOFF.md](HANDOFF.md) - Engineering Specifications.

