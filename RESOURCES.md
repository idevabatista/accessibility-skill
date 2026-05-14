# Accessibility Knowledge Base & Resources (V6.0)
**The Knowledge Ecosystem for Product Teams**

> Atualizado em 2026-05-14 com recursos extraídos de WebAIM, BBC, W3C APG, Deque axe e WCAG 2.2.

---

## 1. Technical Implementation References

### International (Gold Standard)

- **[W3C ARIA APG — Patterns](https://www.w3.org/WAI/ARIA/apg/patterns/):** ⭐ **Referência técnica primária.** Define comportamento completo de cada componente (Roles, States, Keyboard Interaction).
- **[W3C APG — Dialog (Modal)](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/):** Especificação completa do comportamento de foco, keyboard interaction e ARIA para modais.
- **[WCAG 2.2 Quick Reference](https://www.w3.org/WAI/WCAG22/quickref/):** Guia oficial de critérios de sucesso. Inclui WCAG 2.5.8 (Target Size AA) e 2.4.11 (Focus Appearance).
- **[WebAIM — Keyboard Accessibility](https://webaim.org/techniques/keyboard/):** Tabela completa de padrões de teclado por tipo de componente.
- **[WebAIM — Alternative Text](https://webaim.org/techniques/alttext/):** Framework de decisão para alt text com exemplos.
- **[WebAIM — Form Validation & Error Recovery](https://webaim.org/techniques/formvalidation/):** Três abordagens para gerenciamento de erros acessíveis.
- **[WebAIM — Accessible Forms](https://webaim.org/techniques/forms/controls):** Labels, fieldsets, required, aria-invalid, autocomplete.
- **[WebAIM — Introduction to ARIA](https://webaim.org/techniques/aria/):** 5 Regras de Uso do ARIA, Landmark Roles, Live Regions, Labels e Descriptions.
- **[WAVE (WebAIM)](https://wave.webaim.org/):** Ferramenta de avaliação automática de acessibilidade.
- **[Deque axe Platform](https://www.deque.com/axe/):** Suite completa de ferramentas de teste automatizado. Integração com browser DevTools, CI/CD pipelines e design.

### BBC Accessibility Guidelines

- **[BBC Mobile Accessibility Guidelines](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/):** Diretrizes de acessibilidade móvel da BBC. Inclui: Target Touch Size, Colour Contrast, Error Messages, Focus Management, Form Controls, Audio & Video. Licença Open Government — uso livre.
- **[BBC — Target Touch Size](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/target-touch-size/):** Requisito: mínimo 7x7mm (físico). iOS: 44x44pt. Android: 48x48dp.
- **[BBC — Colour Contrast](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/colour-contrast/):** Mínimo 4.5:1 WCAG AA para texto normal. Inclui procedimento de teste com eyedropper.
- **[BBC — Error Messages and Correction](https://www.bbc.co.uk/accessibility/forproducts/guides/mobile/error-messages-and-correction/):** Protocolo BBC: aria-live region + aria-invalid + aria-describedby + foco no primeiro campo inválido.
- **[BBC GEL — Design for Touch](http://www.bbc.co.uk/gel/guidelines/how-to-design-for-touch):** Diretrizes visuais e de interação por toque.

### Brazilian Community Resources

- **[Guia-WCAG](https://guia-wcag.com/):** Tradução e interpretação prática dos critérios WCAG em português.
- **[Movimento Web para Todos (MWPT)](https://mwpt.com.br/):** Movimento brasileiro focado em cultura de acessibilidade digital.

### Platform-Specific References

- **[Apple iOS Human Interface Guidelines — Accessibility](https://developer.apple.com/documentation/accessibility):** Requisitos de acessibilidade iOS/macOS. Target size 44x44pt.
- **[Android Material Design — Touch Target Size](https://material.io/guidelines/layout/metrics-keylines.html#metrics-keylines-touch-target-size):** Requisito 48x48dp com 8dp de espaço entre controles.
- **[appt.org — Guide for Making Apps Accessible](https://appt.org/en/):** Guia prático focado em aplicativos móveis.

---

## 2. Accessibility Engineering Glossary (Expanded)

| Termo | Definição Técnica |
| :--- | :--- |
| **Focus Trapping** | Mecanismo que impede o foco de sair de um Modal até ele ser fechado. Implementado via loop de Tab/Shift+Tab dentro do container. |
| **Live Regions** | Áreas que notificam screen readers sobre mudanças dinâmicas (`aria-live`). Valores: `off`, `polite`, `assertive`. Devem ser definidas no carregamento da página. |
| **Role** | O propósito semântico de um elemento (ex: `button`, `tab`, `dialog`, `alert`). Define como AT interpreta o elemento. |
| **States & Properties** | Atributos dinâmicos (`aria-checked`, `aria-expanded`, `aria-invalid`) e relacionais (`aria-labelledby`, `aria-describedby`). |
| **AOM** | *Accessibility Object Model*. Representação da interface para tecnologias assistivas. O "shadow DOM da acessibilidade". |
| **APCA** | *Accessible Perceptual Contrast Algorithm*. Algoritmo de contraste perceptual — futuro padrão no WCAG 3.0. |
| **Focus Indicator** | Indicador visual do elemento com foco de teclado. Deve ter contraste mínimo de 3:1 contra o fundo adjacente (WCAG 2.4.11 AA em WCAG 2.2). Nunca usar `outline: none` sem substituto. |
| **Target Size** | Área mínima clicável/tocável. WCAG 2.5.8 (AA): 24x24px com 24px livre ao redor. Best practice: 44x44px (iOS HIG, WebAIM). |
| **Accessible Name** | O nome que screen readers anunciam para identificar um elemento. Calculado por: `aria-labelledby` > `aria-label` > `<label>` > atributo `alt` > texto do elemento. |
| **Focus Management** | Prática de controlar programaticamente onde o foco vai após eventos (abertura de modal, envio de formulário, navegação). Crítico para UX de teclado. |
| **aria-modal** | Atributo que informa AT que o conteúdo fora do dialog é inerte. Usar apenas quando o código *realmente* impede interação fora e CSS obscurece o fundo. |
| **aria-invalid** | Estado que indica campo de formulário inválido. Anunciado como "inválido" pelo screen reader. Sem impacto visual — requer CSS adicional. |
| **aria-required** | Indica campo obrigatório. Equivalente funcional ao HTML `required`. Screen readers anunciam "obrigatório". |
| **aria-describedby** | Associa conteúdo de descrição secundária a um elemento (ex: mensagem de erro inline, instruções de formato). Lido após o label. |
| **tabindex="0"** | Torna elemento focável via teclado na ordem natural do documento. Usar apenas em widgets interativos customizados. |
| **tabindex="-1"** | Torna elemento focável somente por programação (JavaScript `focus()`). Útil para mensagens de erro e áreas de conteúdo a receber foco inicial. |
| **tabindex positivo** | `tabindex="1"` ou maior — **nunca usar**. Destrói a ordem natural de navegação e cria bugs de foco difíceis de manter. |
| **Jump Menu** | `<select>` com `onChange` que dispara navegação. **Anti-pattern** — navegar com setas dispara ações acidentais. Substituir por `<select>` + botão submit. |
| **Skip Link** | Link "Ir para o conteúdo" no topo da página. Permite que usuários de teclado saltem o menu de navegação. |
| **WCAG 2.5.8** | Target Size (Minimum) — novo critério AA em WCAG 2.2. Mínimo: 24x24px com 24px de espaço livre ao redor, ou 44x44px sem restrição de espaço. |
| **WCAG 2.4.11** | Focus Appearance — novo critério AA em WCAG 2.2. Focus indicator com área mínima e contraste de 3:1. |

---

## 3. QA & Testing Toolkit

### Ferramentas Automáticas
- **[WAVE](https://wave.webaim.org/):** Extensão de browser + API. Detecta erros, alertas e estrutura. Visual e intuitivo.
- **[axe DevTools (Deque)](https://www.deque.com/axe/):** Extensão Chrome/Firefox. Integração com Cypress, Jest, Storybook. Regras baseadas em WCAG 2.2.
- **[WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker):** Web tool para verificação rápida de ratio de contraste.

### Ferramentas de Desktop
- **TPG Colour Contrast Analyser:** App desktop com eyedropper para medir contraste em qualquer tela (útil para gradientes e imagens).
- **NVDA** (gratuito): Screen reader Windows. Par com Firefox para testes de referência.
- **JAWS** (licença): Screen reader Windows. Par com Chrome.

### Ferramentas Mobile
- **VoiceOver (iOS/macOS):** Screen reader nativo Apple. Essencial para testes de aplicativos Apple.
- **TalkBack (Android):** Screen reader nativo Android.
- **Android Accessibility Scanner:** Auditoria de targets e contraste em Android.

---

## 4. WCAG 2.2 Key Success Criteria Reference

| SC | Nome | Nível | Relevância para Design |
| :--- | :--- | :--- | :--- |
| 1.1.1 | Non-text Content | A | Alt text em imagens |
| 1.2.2 | Captions (Prerecorded) | A | Legendas em vídeo |
| 1.2.5 | Audio Description | AA | Audiodescrição |
| 1.3.1 | Info and Relationships | A | HTML semântico, landmarks, tabelas |
| 1.3.5 | Identify Input Purpose | AA | Autocomplete em formulários |
| 1.4.3 | Contrast (Minimum) | AA | 4.5:1 texto, 3:1 texto grande |
| 1.4.6 | Contrast (Enhanced) | AAA | 7:1 texto, 4.5:1 texto grande |
| 1.4.11 | Non-text Contrast | AA | 3:1 para bordas de UI e ícones |
| 2.1.1 | Keyboard | A | Tudo operável por teclado |
| 2.4.1 | Bypass Blocks | A | Skip links |
| 2.4.6 | Headings and Labels | AA | Headings descritivos |
| 2.4.7 | Focus Visible | AA | Focus indicator visível |
| 2.4.11 | Focus Appearance | AA *(WCAG 2.2 novo)* | Focus ring com contraste 3:1 e área mínima |
| 2.5.8 | Target Size (Minimum) | AA *(WCAG 2.2 novo)* | 24x24px com 24px de espaço ao redor |
| 3.3.1 | Error Identification | A | Identificar campos com erro |
| 3.3.2 | Labels or Instructions | A | Labels e instruções de preenchimento |
| 3.3.3 | Error Suggestion | AA | Sugerir como corrigir o erro |
| 4.1.2 | Name, Role, Value | A | ARIA correto em componentes customizados |
| 4.1.3 | Status Messages | AA | `role="status"` / `aria-live` para mensagens de status |

---

## Navigation
- [SKILL.md](SKILL.md) - Skills Framework.
- [HANDOFF.md](HANDOFF.md) - Engineering Specifications.
