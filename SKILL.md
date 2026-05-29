# Accessibility Skills Taxonomy (V7.0 - Multi-Context Edition)
**Governance Framework for Design Ops & Engineering**

> Updated 2026-05-29. Sources: WebAIM, BBC Mobile Accessibility Guidelines, W3C APG, Deque axe, React Aria, Radix UI, WCAG 3 / Silver direction.
> This framework maps technical and design competencies to **WCAG 2.1/2.2**, **W3C ARIA Patterns**, and **modern frontend accessibility**.

---

## 💡 Core Audit Principles (Context-Awareness)

Before performing any audit, identify the context of the artifact provided:

1.  🎨 **UX/UI Design (Figma / Mockups / Screens)**
    *   **Goal:** Document critical visual and interaction design accessibility gaps.
    *   **Rule:** **NEVER** report missing HTML elements, tags, attributes, or ARIA properties (e.g., `lang`, `role`, `aria-*`, `<main>`).
    *   **Focus on:** Color contrast ratios, touch target sizes, visual focus indicator designs, non-color state indicators, heading hierarchy concepts, alternative text specs, and annotation of intended keyboard sequences.

2.  💻 **Front-end Development (HTML / CSS / JS / Live URLs)**
    *   **Goal:** Audit technical semantic implementation and screen reader compatibility.
    *   **Focus on:** HTML5 landmarks, correct tab order, skip links, semantic headings (`<h1>`-`<h6>`), lists/tables semantics, form label association, ARIA roles, dynamic states (`aria-expanded`, `aria-selected`), framework routing focus, and keyboard focus trapping.

---

## 1. Strategic Compliance Mapping

| Category | Core Skill | WCAG Level | Figma Focus (Design) | HTML Focus (Code) |
| :--- | :--- | :--- | :--- | :--- |
| **Structure** | 1. Hierarchy, Semantics & Landmarks | **A / AA** | Typographic Hierarchy, Reading Order | Semantic Landmarks, Heading tags |
| **Structure** | 2. Lists and Data Tables | **A** | Visual groupings, Clear grid layouts | `<ul>`/`<ol>` tags, semantic `<th>` headers |
| **Design** | 3. Contrast and Discernibility | **AA / AAA** | Color contrast (4.5:1/3:1), Non-color cues | CSS color properties, High-Contrast support |
| **Interaction** | 4. Focus & Target Size | **A / AA** | Touch target size (44x44px), Focus states | Keyboard tab order, focus ring, focus trap |
| **Forms** | 5. Input & Error Management | **A / AA** | Visible labels, error layout placement | `<label>` binding, `aria-describedby` |
| **Content** | 6. Alternative Text | **A** | Annotating decorative vs informative | Correct `alt="..."` or `alt=""` attributes |
| **Content** | 7. Accessible Media (Video/Audio) | **A / AA** | Captions visual design, audio-description cues | WebVTT integration, keyboard player controls |
| **Dynamic** | 8. ARIA & Rich Components | **A / AA** | Carousel buttons labeling, modal interaction | W3C Keyboard patterns, dynamic ARIA states |
| **Framework** | 9. Modern Framework Accessibility | **A / AA** | Skeleton loader screens, transition alerts | SPA & SSR Robustness, hydration, Radix/Aria |
| **Mobile** | 10. Native Gestures & Mobile AT | **A / AA** | Touch target margins, gesture alternatives | VoiceOver swipe order, Android TalkBack |
| **Quality** | 11. Automated Testing & CI/CD | **A / AA** | Handoff visual specs, Figma plugins | axe-core integration, Jest-axe, regression |
| **Performance** | 12. Performance & Motion | **AA** | Motion bypass options, auto-advance play/pause | `prefers-reduced-motion`, focus preservation |
| **Cognitive** | 13. Cognitive Accessibility | **A / AA / AAA** | Layout simplicity, plain language | Lang tag, session extend warnings, COGA |

---

## 2. Detailed Competencies

### 1. Hierarchy, Semantics & Landmarks

- **Why it matters:** Establishes the core structure of the application. Allows assistive technologies to understand the hierarchy and navigate seamlessly. Screen readers announce landmarks and allow shortcut navigation.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Typographic Hierarchy:** Design a logical visual hierarchy where title sizes and weights clearly indicate their importance.
    *   **Reading Flow:** Ensure visual content flows naturally (typically left-to-right, top-to-bottom) so that reading order is highly intuitive.
    *   **Regional Segregation:** Clearly delineate visual page boundaries (main content, navigation sidebar, footer) so they translate to logical layout containers.
  - 💻 **HTML / Code:**
    *   **Landmarks:** Mandatory use of semantic landmark tags (`<main>`, `<nav>`, `<header>`, `<aside>`, `<footer>`). Use `<form role="search">` for search regions.
    *   **Multiple regions of the same type** must have `aria-label` or `aria-labelledby` to differentiate them (e.g. `<nav aria-label="Main navigation">`).
    *   **Headings:** Use logical heading order (H1 → H2 → H3). Never skip levels. Every page must have a unique `<title>`.
    *   **Skip Links:** "Skip to main content" link visible at the top of the page.
    *   **ARIA Rules:** Always prefer native HTML elements. Use ARIA only when HTML is insufficient. Do not override native semantics.
- **How to Test:**
  - *Figma:* Check visual typography scaling and flow diagrams.
  - *HTML:* Audit landmarks with a screen reader. Check heading outline. Navigate using only `Tab` and `Shift+Tab`.

---

### 2. Lists and Data Tables

- **Why it matters:** Communicates the number of items and relationships between complex data. Screen readers announce "List, X items".
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Visual Grouping:** Group repeating elements (e.g., card decks, vertical lists) using clear spacing and visual alignment.
    *   **Grid Structure:** Design tables with clearly defined boundaries, visually distinguished header rows, and legible data alignment.
  - 💻 **HTML / Code:**
    *   **Lists:** Wrap repeated elements in `<ul>` or `<ol>` tags with matching `<li>` children. Never use CSS `list-style: none` on functional lists without a role.
    *   **Tables:** Use `<th scope="col|row">` and `<caption>`. Never use tables for layout.
    *   **Complex tables:** Use `aria-labelledby` to concatenate multiple headers.
- **How to Test:**
  - *Figma:* Verify alignment, groupings, and visual clarity of grids.
  - *HTML:* Check screen reader announcements ("List, 5 items"). Navigate table cells with arrow keys.

---

### 3. Contrast and Discernibility

- **Why it matters:** Ensures readability under adverse conditions, for users with low vision, colour blindness, and in high-luminance environments.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Contrast Ratios:** Maintain the following minimum ratios:
        *   Normal text (< 18pt / < 14pt bold): **4.5:1** (AA) / **7:1** (AAA).
        *   Large text (≥ 18pt / ≥ 14pt bold): **3:1** (AA) / **4.5:1** (AAA).
        *   UI components (input borders, active boundaries, functional icons): **3:1**.
    *   **Gradients and images:** Apply a semi-transparent overlay or text-shadow. The measurement point is the lowest-contrast pixel in the text area.
    *   **Colour is not the only differentiator:** Always combine colors with shapes, lines, text, or icons (e.g., active links must have underlines, error states must have helper text or warning icons).
    *   **Text images:** Avoid text embedded directly in flat images.
  - 💻 **HTML / Code:**
    *   **CSS Contrast:** Ensure CSS color values maintain the approved contrast ratios in all states (hover, active, focus).
    *   **High-Contrast support:** Support user system-level high-contrast or dark mode preferences.
- **How to Test:**
  - *Figma:* Run Colour Contrast Analyser or plugins (e.g., Stark). Test with colour-blindness simulation.
  - *HTML:* Inspect calculated styles in DevTools. Test with system-level high contrast mode enabled.

---

### 4. Focus & Target Size

- **Why it matters:** Critical for keyboard and mobile users. Without a visible focus indicator, keyboard users are "lost" in the interface.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Touch Targets:** Minimum visual size of **44x44px** (or 24x24px with sufficient safety spacing around the target to prevent overlapping clicks).
    *   **Visual Focus State:** Explicitly design the visible focus state (e.g., `:focus` outline/ring) for all interactive components. It must have at least **3:1** contrast against both the component and the adjacent background.
    *   **Focus Flow Sequence:** Document the planned keyboard navigation flow across the screen layout.
  - 💻 **HTML / Code:**
    *   **Focus Ring Preservation:** Never use `outline: 0` or `outline: none` on focusable elements without an equivalent visible substitute.
    *   **Focus Trapping:** Implement robust focus trapping inside active dialogs/modals (focus must not leak to background elements).
    *   **Interactive Targets:** Ensure DOM element sizes match target specifications. Correctly manage focusable elements with `tabindex` and `aria-hidden` rules.
- **How to Test:**
  - *Figma:* Measure target coordinate sizes and verify focus ring designs.
  - *HTML:* Navigate exclusively with `Tab`. Validate focus outline visibility.

---

### 5. Input & Error Management

- **Why it matters:** Reduces friction and prevents task abandonment. Users with cognitive disabilities depend on clear messages and guided correction.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Visible Labels:** Form controls must have a visible, persistent text label. Do not hide important instructions or labels inside placeholder text.
    *   **Error Indicators:** Visual error messages must appear close to their corresponding input, with descriptive helper text and support icons.
    *   **Form Field States:** Design visual states for Hover, Focus, Disabled, Active, and Error.
    *   **Grouping:** Place groups of checkboxes/radio buttons under a clear section title.
  - 💻 **HTML / Code:**
    *   **Label Binding:** Explicitly associate `<label>` tags to `<input>` fields via `for` and `id` matching. Use `<fieldset>` + `<legend>` for checkboxes/radios.
    *   **Required & Invalid:** Set the `required` HTML attribute or `aria-required="true"`. Apply `aria-invalid="true"` and use `aria-describedby` to associate the field with its inline error message.
    *   **Error Recovery Focus:** Move focus to error summaries on form submit or announce them via live regions. Use `autocomplete` attributes.
- **How to Test:**
  - *Figma:* Verify visual mockups for all input states and clear error layouts.
  - *HTML:* Submit invalid form data. Check whether errors are announced. Test with keyboard only.

---

### 6. Alternative Text

- **Why it matters:** Delivers image value to screen reader users. Impacts SEO. Critical when images fail to load.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Image Classification:** Document in annotations whether an image is "Informative" or "Decorative".
    *   **Alt Spec:** Write proposed descriptive text in the design annotations for all informative visuals (describing function/destination for links or actions).
  - 💻 **HTML / Code:**
    *   **Informative Images:** Apply a functional, concise `alt="..."` description.
    *   **Decorative Images:** Set empty alt tags `alt=""` so assistive tools ignore them.
- **How to Test:**
  - *Figma:* Read design annotations for alternative text descriptions.
  - *HTML:* Disable images on the web browser and verify if the page remains perfectly understandable.

---

### 7. Accessible Media (Video & Audio)

- **Why it matters:** Inclusion for deaf, blind, and hearing/visually impaired users.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Player Controls:** Visual designs must include high-contrast and keyboard-tabbable controls (Play, Pause, Progress, Mute, Closed Captions).
    *   **Transcript Accessibility:** Design a visible entry point (link or button) to download or read text transcripts of video or audio.
  - 💻 **HTML / Code:**
    *   **Captions/Tracks:** Implement `<track>` elements with SRT/WebVTT captions files for video and audio markup.
    *   **Autoplay:** Video may autoplay without sound if a visible pause/stop control is present.
    *   **Keyboard Controls:** All custom player controls must be operable by keyboard and labeled with correct accessible names.
- **How to Test:**
  - *Figma:* Review media player component screens and layout links.
  - *HTML:* Watch content without sound and check keyboard interactions on video players.

---

### 8. ARIA & Rich Components (W3C APG)

- **Why it matters:** Communicates states and behaviours for complex UIs (SPAs). ARIA extends HTML's semantic vocabulary for cases not natively covered.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Interaction Context:** Document exactly how rich elements behave (e.g., auto-sliding carousels must have play/pause visual buttons).
    *   **Icon-Only Button Labels:** Explicitly annotate visual icons that act as controls (such as "next" and "previous" chevron arrows on a carousel) with their intended descriptive names (e.g., "Next Slide", "Previous Slide").
  - 💻 **HTML / Code:**
    *   **Button & Toggles:** Implement correct `role="button"` and toggles using `aria-pressed="true/false"`.
    *   **Modal Dialogs:** Apply `role="dialog"`, `aria-modal="true"`. Restrict keyboard focus to the active modal (Focus Loop). Return focus to the trigger on close.
    *   **Tabs & Accordions:** Implements correct markup (`role="tablist"`, `role="tab"`, `role="tabpanel"`, `aria-selected`, `aria-expanded`). Follow W3C keyboard arrow key patterns.
    *   **Live Regions:** Utilize `aria-live="polite"` or `role="alert"` (equivalent to `assertive`) to announce dynamic content updates.
- **How to Test:**
  - *Figma:* Review carousel and interactive layout control designs.
  - *HTML:* Validate state transitions with a screen reader and navigate solely using keyboard arrow and tab keys.

---

### 9. Modern Framework Accessibility

- **Why it matters:** SPAs and SSR frameworks break native browser behaviours — page transitions don't reload, focus is not restored, and dynamic content is not announced without explicit management.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Skeleton Loading States:** Design visible skeleton loader layouts with high-contrast indicator shapes.
    *   **State indicators:** Graphically plan the visual indicators for dynamic rendering blocks and pagination.
  - 💻 **HTML / Code:**
    *   **Hydration:** Ensure server-rendered ARIA attributes match client-rendered output to prevent role stripping.
    *   **Route transitions:** On navigation, move focus to the `<h1>` or a `tabindex="-1"` landmark and announce transitions via `aria-live`.
    *   **Skeleton Busy States:** Wrap loading skeletons with `aria-busy="true"` and `role="status"` announcements.
    *   **Headless Libraries:** Utilize accessible primitives (e.g. Radix UI, React Aria) that implement W3C APG keyboard patterns.
- **How to Test:**
  - *Figma:* Verify state mockups for loading overlays and skeleton components.
  - *HTML:* Navigate routes with VoiceOver/NVDA active. Confirm focus is not stranded on a removed element.

---

### 10. Native Gestures & Mobile AT

- **Why it matters:** Touch-first interfaces require gesture-aware design. Screen reader gestures on mobile differ fundamentally from keyboard navigation on desktop.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Touch spacing:** Minimum margin sizes (8dp on Android, 1px on iOS) between active controls to prevent miss-touches.
    *   **Gesture alternatives:** Design simple tapping button alternatives for all complex gestures (e.g., if swiping a row deletes it, design an alternative options menu accessible by a single tap).
  - 💻 **HTML / Code:**
    *   **Swipe order:** Ensure visual swipe sequence matches DOM source order. Prevent mobile-AT mismatches.
    *   **Semantic grouping:** Group related text and badges (e.g., in lists or cards) under a single `role="group"` + `aria-label` container.
- **How to Test:**
  - *Figma:* Measure margins and review layout files for gesture alternatives.
  - *HTML:* Enable VoiceOver on iOS or TalkBack on Android and swipe through the full interface.

---

### 11. Automated Testing & CI/CD

- **Why it matters:** Manual audits find ~30% of issues. Automated testing catches the remaining systematic errors continuously, not just at release time.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Quality Handoff:** Apply Figma accessibility plug-ins to review contrasts and target sizes before handoff.
  - 💻 **HTML / Code:**
    *   **Pipeline integration:** Implement `eslint-plugin-jsx-a11y` in build steps and `jest-axe` in unit tests.
    *   **E2E Validation:** Integrate `cypress-axe` or Playwright accessibility snapshots into core user flows. Set zero-tolerance for critical/serious accessibility budgets.
- **How to Test:**
  - *Figma:* Review design handoff annotation layers.
  - *HTML:* Verify pipeline execution logs and ESLint output.

---

### 12. Performance & Motion

- **Why it matters:** Performance degradation creates accessibility barriers. Motion can trigger vestibular disorders.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Motion Control:** Design visual options or play/pause buttons to disable heavy vestibular-triggering animations (e.g., auto-advancing carousels, parallax effects, or fast zoom transitions).
  - 💻 **HTML / Code:**
    *   **Reduced Motion CSS:** Implements `prefers-reduced-motion` media queries in CSS to shut down transitions when requested.
    *   **CLS prevention:** Reserve layout space for images and async dynamic elements to preventCumulative Layout Shift. Focus preservation on element deletion.
- **How to Test:**
  - *Figma:* Check carousel controls and motion specs.
  - *HTML:* Enable `prefers-reduced-motion` in OS settings and check CSS response.

---

### 13. Cognitive Accessibility

- **Why it matters:** 1 in 6 people has a cognitive or learning disability. WCAG 3 / Silver and the COGA guidelines address this gap directly by prioritizing layout simplicity.
- **Audit Criteria:**
  - 🎨 **Figma / Design:**
    *   **Consistent Layouts:** Maintain identical navigation and component sequences across all layout screens.
    *   **Visual Simplicity:** Prevent cognitive overload by limiting visual noise, dividing complex forms progressively, and leveraging clean whitespace.
    *   **Plain language:** Review microcopy to target clear reading levels. Avoid justified text block alignments.
  - 💻 **HTML / Code:**
    *   **Language tags:** Define correct `lang="..."` attributes on the root HTML element and on inline code shifts.
    *   **Session warnings:** Warn users at least 20 seconds before sessions expire, offering a simple click to extend.
- **How to Test:**
  - *Figma:* Conduct visual simplicity audits and evaluate font and alignment rules.
  - *HTML:* Check correct lang attributes in source code.

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
