# Accessibility Skills Taxonomy (V3.1 - Cumulative Final)
**Governance Framework for Design Ops & Product**

This document consolidates 100% of the core competencies mapped to **WCAG 2.1/2.2** success criteria.

## 1. Executive Summary: Compliance Mapping

| Category | Core / Specialist Skill | Strategic Impact | WCAG Level | Main Ref. |
| :--- | :--- | :--- | :--- | :--- |
| **Structure** | 1. Hierarchy and Semantics | Structural Navigation & SEO | **A / AA** | 1.3.1, 2.4.6 |
| **Structure** | 2. Lists and Data Tables | Integrity & Organization | **A** | 1.3.1 |
| **Design** | 3. Contrast and Discernibility | Universal Readability | **AA** | 1.4.3, 1.4.11 |
| **Design** | 4. Focus Management (Pro) | Efficiency & Focus Trapping | **A / AA** | 2.1.1, 2.4.7 |
| **Forms** | 5. Input & Error Management | Conversion & Friction Reduction | **A / AA** | 3.3.1, 3.3.3 |
| **Content** | 6. Alternative Text (Alt Text) | Reach & Visual Accessibility | **A** | 1.1.1 |
| **Content** | 7. Media (Video & Audio) | Sensory Inclusion | **A / AA** | 1.2.2, 1.2.5 |
| **Dynamic** | 8. ARIA & Dynamic States | Robustness in Rich Interfaces | **A / AA** | 4.1.2, 4.1.3 |

---

## 2. Detailed Competencies

### 1. HTML Semantics and Heading Hierarchy
*   **The "Why":** Defines the "Accessibility Tree". Allows screen reader users to understand the structure and "jump" between sections.
*   **Technical Criteria:**
    *   **Landmarks:** Use of `<main>`, `<nav>`, `<header>`.
    *   **Headings:** Logical order H1 > H2 > H3. Never skip levels for aesthetic reasons.
*   **How to Test:** Landmark audits and page outline verification.
> [!NOTE]
> Reference: [WCAG 1.3.1 (A)](https://www.w3.org/WAI/WCAG21/Understanding/info-and-relationships.html) | [WCAG 2.4.6 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/headings-and-labels.html)

### 2. List Structure and Data Tables
*   **The "Why":** Informs the user about the number of items and the relationship between complex data, avoiding the cognitive load of processing isolated data.
*   **Technical Criteria:**
    *   **Lists:** Use `<ul>`/`<ol>` for groups of 2+ items.
    *   **Tables:** Use `<th>` with `scope` and `<caption>`. Never use tables for layout.
*   **How to Test:** Verify if the screen reader announces "List, X items" or reads the correct header for each table cell.
> [!NOTE]
> Reference: [WCAG 1.3.1 (A)](https://www.w3.org/WAI/WCAG21/Understanding/info-and-relationships.html)

### 3. Contrast and Discernibility
*   **The "Why":** Ensures readability in adverse conditions (low vision or direct sunlight).
*   **Technical Criteria:**
    *   **Text:** 4.5:1 (normal) / 3:1 (large).
    *   **UI:** 3:1 for button borders and essential icons.
    *   **Indicators:** Never use color alone to convey status (e.g., error).
*   **How to Test:** Colour Contrast Analyser and color blindness simulation.
> [!NOTE]
> Reference: [WCAG 1.4.3 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html) | [WCAG 1.4.11 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/non-text-contrast.html)

### 4. Focus Management and Keyboard Navigation
*   **The "Why":** Fundamental for those who do not use a mouse. Focus must be visible and controlled.
*   **Technical Criteria:**
    *   **Focus Trapping:** Focus must remain "trapped" inside modals/overlays.
    *   **Focus Return:** When closing a modal, focus returns to the triggering button.
    *   **Links vs Buttons:** `<a>` for navigation, `<button>` for state actions.
*   **How to Test:** Exclusive `Tab` navigation; check for a visible focus indicator.
> [!NOTE]
> Reference: [WCAG 2.1.1 (A)](https://www.w3.org/WAI/WCAG21/Understanding/keyboard.html) | [WCAG 2.4.7 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/focus-visible.html)

### 5. Input Semantics and Error Management
*   **The "Why":** Reduces friction in form filling and prevents task abandonment.
*   **Technical Criteria:**
    *   **Links:** `<label for="ID">` connected to `<input id="ID">`.
    *   **Dynamic Messages:** Use `role="alert"` and `aria-describedby` to link the error to the field.
    *   **Suggestions:** The system must say *how* to correct the error (e.g., date format).
*   **How to Test:** Try submitting the form empty/incorrect and verify if the error is announced immediately.
> [!NOTE]
> Reference: [WCAG 3.3.1 (A)](https://www.w3.org/WAI/WCAG21/Understanding/error-identification.html) | [WCAG 3.3.3 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/error-suggestion.html)

### 6. Alternative Text (Alt Text)
*   **The "Why":** Delivers the value of the image to those who cannot see it. Improves SEO and page resilience.
*   **Technical Criteria:**
    *   **Descriptive:** Short and focused on the *function* of the image.
    *   **Decorative:** Mandatory use of `alt=""` to ignore aesthetic elements.
*   **How to Test:** Disable images and check if the context remains.
> [!NOTE]
> Reference: [WCAG 1.1.1 (A)](https://www.w3.org/WAI/WCAG21/Understanding/non-text-content.html)

### 7. Accessible Media (Video & Audio)
*   **The "Why":** Inclusion of deaf and blind users in multimedia content.
*   **Technical Criteria:**
    *   **Captions:** Synchronized for all relevant audio (Level A).
    *   **Audio Description:** Description of visual actions not narrated (Level AA).
    *   **Transcripts:** Full text for podcasts or videos.
*   **How to Test:** Watch without sound and listen only to audio to validate comprehension.
> [!NOTE]
> Reference: [WCAG 1.2.2 (A)](https://www.w3.org/WAI/WCAG21/Understanding/captions-prerecorded.html) | [WCAG 1.2.5 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/audio-description-prerecorded.html)

### 8. ARIA and Dynamic Components (Advanced)
*   **The "Why":** Communicates state changes in real-time (SPAs).
*   **Technical Criteria:**
    *   **States:** Use of `aria-expanded`, `aria-selected`, `aria-hidden`.
    *   **Live Regions:** `aria-live="polite"` for non-disruptive notifications.
*   **How to Test:** Check screen reader announcements when opening menus or loading data.
> [!NOTE]
> Reference: [WCAG 4.1.2 (A)](https://www.w3.org/WAI/WCAG21/Understanding/name-role-value.html) | [WCAG 4.1.3 (AA)](https://www.w3.org/WAI/WCAG21/Understanding/status-messages.html)

---

## Further Reading
*   [RESOURCES.md](file:///c:/Users/29921398822/Documents/Skills/RESOURCES.md) - Tools and Knowledge Base.
*   [HANDOFF.md](file:///c:/Users/29921398822/Documents/Skills/HANDOFF.md) - Design-to-Dev Delivery Guide.
