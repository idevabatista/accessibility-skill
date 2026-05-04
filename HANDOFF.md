# Accessibility Handoff Guide
**From Intent to Implementation: Bridging the Design-Dev Gap**

This guide establishes the standard for delivering accessible designs to the engineering team, ensuring that accessibility is "baked in" rather than "bolted on."

## 1. "Ready for Dev" Checklist
Before moving a component or page to the development stage, the designer must validate:

- [ ] **Contrast Ratios:** All text and UI elements meet WCAG AA standards (checked via Contrast Analyser).
- [ ] **Tab Order:** Navigation flow is logically documented (1, 2, 3...).
- [ ] **Alt Text:** Descriptive or decorative (`alt=""`) status is specified for every image.
- [ ] **Heading Levels:** H1, H2, and H3 hierarchy is clearly marked.
- [ ] **Interactive Elements:** Buttons vs. Links are correctly identified.
- [ ] **Error States:** Feedback messages are designed and linked to their respective fields.

## 2. Technical Specification for Figma
To facilitate implementation, designers should use annotations or a dedicated "Accessibility Layer" in Figma.

### Aria-labels & Names
*   **IconButton:** Specify the label (e.g., `aria-label="Close menu"` for an 'X' icon).
*   **Group Labels:** Specify if a section needs a label (e.g., `aria-labelledby`).

### Dynamic States
Document how the component changes visually and semantically:
*   **Expanded/Collapsed:** State changes for Accordions and Menus (`aria-expanded="true/false"`).
*   **Selected:** State for Tabs or Radio buttons (`aria-selected="true"`).
*   **Loading:** Use of `aria-busy` or `aria-live` for data fetching.

### Focus Management
*   **Focus Ring:** Design a high-contrast focus indicator for all interactive elements.
*   **Focus Trapping:** Explicitly mark Modals and Overlays where the focus must be contained.
*   **Return Target:** Annotate where the focus should return after an overlay is closed.

## 3. Governance Methodology (Design Ops)

1.  **Component Library:** Audit every component in the Design System against the [SKILL.md](file:///c:/Users/29921398822/Documents/Skills/SKILL.md) criteria.
2.  **QA Cycle:** Conduct periodic audits using Axe DevTools and manual screen reader validation (NVDA/VoiceOver).
3.  **Continuous Training:** Host "Accessibility Brown Bag" sessions to keep the team updated on WCAG 2.2 changes.

---

## Navigation
*   [SKILL.md](file:///c:/Users/29921398822/Documents/Skills/SKILL.md) - Back to Taxonomy.
*   [RESOURCES.md](file:///c:/Users/29921398822/Documents/Skills/RESOURCES.md) - Knowledge Base & Tools.
