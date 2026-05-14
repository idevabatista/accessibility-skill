# Accessibility Skills Taxonomy (V6.0 — Crawled Edition)
**Governance Framework for Design Ops & Product**

> Atualizado em 2026-05-14 com dados extraídos de: WebAIM, BBC Mobile Accessibility Guidelines, W3C APG e Deque axe.
> Este framework mapeia competências técnicas para **WCAG 2.1/2.2** e **W3C ARIA Patterns**.

---

## 1. Strategic Compliance Mapping

| Category | Core Skill | WCAG Level | Product Impact | Technical Ref. |
| :--- | :--- | :--- | :--- | :--- |
| **Structure** | 1. Hierarchy, Semantics & Landmarks | **A / AA** | Navegação estrutural & SEO | WCAG 1.3.1, 2.4.1, 2.4.6 |
| **Structure** | 2. Lists and Data Tables | **A** | Integridade de dados | WCAG 1.3.1 |
| **Design** | 3. Contrast and Discernibility | **AA / AAA** | Legibilidade universal | WCAG 1.4.3, 1.4.6, 1.4.11 |
| **Interaction** | 4. Focus & Target Size | **A / AA** | Precisão de teclado & toque | WCAG 2.1.1, 2.4.7, 2.5.8 |
| **Forms** | 5. Input & Error Management | **A / AA** | Conversão & UX | WCAG 3.3.1, 3.3.2, 3.3.3, 1.3.5 |
| **Content** | 6. Alternative Text | **A** | Inclusão visual | WCAG 1.1.1 |
| **Content** | 7. Accessible Media (Video/Audio) | **A / AA** | Inclusão sensorial | WCAG 1.2.2, 1.2.5 |
| **Dynamic** | 8. ARIA & Rich Components | **A / AA** | Robustez da interface | W3C ARIA APG |

---

## 2. Detailed Competencies

### 1. Hierarchy, Semantics & Landmarks

- **O "Porquê":** Define a "Accessibility Tree". Permite que usuários compreendam a estrutura e saltem entre seções. Screen readers anunciam landmarks e permitem navegação por atalhos.
- **Critérios Técnicos de Aceitação:**
  - **Landmarks obrigatórios:** `<main>`, `<nav>`, `<header>`, `<aside>`, `<footer>`. Usar `<form role="search">` para buscas.
  - **Múltiplas regiões do mesmo tipo** devem ter `aria-label` ou `aria-labelledby` para diferenciação (ex: dois `<nav aria-label="Menu principal">`).
  - **Região genérica:** Usar `role="region"` + `aria-label` para seções significativas sem landmark nativo.
  - **Headings:** Hierarquia lógica H1 → H2 → H3. Nunca pular níveis por razões estéticas. Cada página/tela com `<title>` único e descritivo.
  - **Skip Links:** Link "Ir para o conteúdo" visível no topo da página (BBC & WebAIM requirement).
  - **Ordem de leitura:** Código-fonte deve refletir ordem lógica visual (esquerda→direita, topo→baixo).
  - **Regra ARIA #1 (WebAIM):** Sempre preferir elemento HTML nativo. ARIA somente quando HTML não é suficiente.
  - **Regra ARIA #2:** Não sobrescrever semântica nativa sem necessidade (ex: `<ul role="navigation">` destrói os benefícios de lista).
- **Como Testar:** Auditoria de landmarks com screen reader (NVDA/JAWS/VoiceOver). Verificar outline de headings. Navegar apenas com `Tab` e `Shift+Tab`.

---

### 2. Lists and Data Tables

- **O "Porquê":** Informa o número de itens e relações entre dados complexos. Screen readers anunciam "Lista, X itens".
- **Critérios Técnicos de Aceitação:**
  - **Listas:** Usar `<ul>`/`<ol>` para grupos de 2+ itens. Nunca usar CSS `list-style:none` em listas funcionais sem role.
  - **Tabelas:** Usar `<th scope="col|row">` e `<caption>`. Nunca usar tabelas para layout.
  - **Agrupamento:** `<optgroup>` em selects — atenção: suporte inconsistente em screen readers (WebAIM). Não depender para contexto vital.
  - **Tabelas complexas:** Usar `aria-labelledby` para concatenar múltiplos cabeçalhos.
- **Como Testar:** Verificar anúncios de screen reader ("Lista, 5 itens"). Navegar dentro de tabelas com setas do teclado.

---

### 3. Contrast and Discernibility

- **O "Porquê":** Garante legibilidade em condições adversas, para usuários com baixa visão, daltonismo, e em ambientes de alta luminosidade.
- **Critérios Técnicos de Aceitação (BBC + WCAG):**

| Tipo de conteúdo | Ratio mínimo (WCAG AA) | Ratio ideal (WCAG AAA) |
| :--- | :--- | :--- |
| Texto normal (< 18pt / < 14pt bold) | **4.5:1** | 7:1 |
| Texto grande (≥ 18pt / ≥ 14pt bold) | **3:1** | 4.5:1 |
| Elementos de UI (bordas de input, ícones funcionais) | **3:1** | — |
| Links inline diferenciados apenas por cor | **3:1** vs. texto ao redor | — |

  - **Gradientes e imagens:** Aplicar overlay semitransparente ou text-shadow. O ponto de medição é o pixel de menor contraste na área do texto (BBC guideline).
  - **Cor não pode ser o único diferenciador** — sempre combinar com ícone, sublinhado ou forma.
  - **Imagens de texto:** Evitar. Se necessário, aplicar 4.5:1.
  - **Ferramenta de QA:** WebAIM Contrast Checker, TPG Colour Contrast Analyser, axe DevTools.
- **Como Testar:** Colour Contrast Analyser (ferramenta). Inspecionar cor via Dev Tools. Testar em simulação de daltonismo.

---

### 4. Focus & Target Size (Interaction)

- **O "Porquê":** Crítico para usuários de teclado e mobile. Sem indicador de foco visível, usuários de teclado ficam "perdidos" na interface.
- **Critérios Técnicos de Aceitação:**

#### Focus Indicator
  - **Proibido:** `outline: 0` ou `outline: none` em elementos focáveis sem substituto equivalente (WebAIM).
  - **Padrão mínimo:** O indicador deve ter contraste de pelo menos **3:1** contra o fundo adjacente (WCAG 2.4.11 — AA em WCAG 2.2).
  - **Recomendado:** Adicionar background-color ou border visíveis além do outline nativo.
  - **Regra ARIA #4:** Elementos focáveis por `Tab` nunca devem ter `aria-hidden="true"`.

#### Target Size (BBC + WCAG 2.5.8)

| Plataforma | Tamanho mínimo | Regra |
| :--- | :--- | :--- |
| **Web (WCAG 2.5.8 AA)** | **24x24 CSS px** com 24px de espaço livre ao redor | Critério mínimo |
| **Web (best practice)** | **44x44 CSS px** | Recomendado por WebAIM & iOS HIG |
| **iOS** | **44x44 pt** com pelo menos 1px entre targets | Apple HIG requirement |
| **Android** | **48x48 dp** com pelo menos 8dp de espaço entre controles | Material Design |
| **BBC Mobile (físico)** | **7x7mm** mínimo | Menor dedo médio humano |
| **BBC Mobile (recomendado)** | **9-10mm** | Para acessibilidade total |

  - **CSS de referência:** `button { box-sizing: border-box; min-width: 44px; min-height: 44px; }`
  - **Agrupamento:** Links adjacentes ao mesmo destino devem ser combinados em um único alvo de toque (BBC guideline).
  - **tabindex="0":** Torna elemento focável via teclado (usar apenas em widgets customizados).
  - **tabindex="-1":** Elemento focável apenas programaticamente (útil para mensagens de erro e diálogos).
- **Como Testar:** Navegação exclusiva por `Tab`. Auditoria de touch targets no Figma (medir em px/pt/dp). Validar focus ring visível.

---

### 5. Input & Error Management

- **O "Porquê":** Reduz fricção no preenchimento e prevê abandono de tarefa. Usuários com deficiências cognitivas dependem de mensagens claras e correção guiada.
- **Critérios Técnicos de Aceitação:**

#### Labeling
  - `<label for="id">` obrigatório e programaticamente associado a cada `<input>`.
  - Clicar no label deve ativar/focar o controle (teste de associação rápida — WebAIM).
  - Grupos de checkboxes/radio buttons: usar `<fieldset>` + `<legend>` (legend deve ser breve).
  - **Nunca aninhar fieldsets** — causa comportamento estranho em screen readers.

#### Required & Invalid
  - Usar atributo `required` (HTML nativo) OU `aria-required="true"` — screen readers anunciam "obrigatório".
  - **Atenção:** Asterisco (*) como único indicador de campo obrigatório é insuficiente.
  - Após validação com erros: aplicar `aria-invalid="true"` no campo.
  - Usar `aria-describedby` para associar o campo à mensagem de erro inline.
  - Usar `autocomplete` em campos de propósito conhecido (WCAG 1.3.5).

#### Error Recovery (3 abordagens — WebAIM)
1. **Error alert, then focus:** Modal customizado acessível → anunciar erro → focar no campo com problema.
2. **Errors on top:** Mensagem acima do formulário com `focus()`. Listar todos os erros com links para os campos.
3. **Inline errors:** Mensagens no contexto do campo via `aria-describedby`. Focar no primeiro campo inválido.
4. **Combinação Errors on top + Inline:** Abordagem mais robusta (WebAIM best practice).

#### BBC Error Protocol
  - Ao submeter formulário: usar `aria-live` region com lista de campos com erro acima do formulário.
  - Mover foco para a mensagem de erro ao submeter.
  - `aria-invalid="true"` + `aria-describedby` + cues visuais inline em cada campo inválido.

#### Tipos de Input
  - Usar `<input type="tel">`, `type="email"`, `type="number"`, `type="date">` — dispara teclado correto em mobile.
  - **Evitar** `<select>` múltiplo — suporte de teclado inconsistente. Prefira grupo de checkboxes.
  - **Reset buttons devem ser evitados** — fáceis de acionar por engano (WebAIM).

- **Como Testar:** Submeter formulário com dados incorretos. Verificar se erros são anunciados. Testar apenas com teclado.

---

### 6. Alternative Text

- **O "Porquê":** Entrega o valor da imagem a usuários de screen reader. Impacta SEO.
- **Critérios Técnicos de Aceitação (WebAIM Alt Text Framework):**

| Tipo de imagem | Atributo `alt` | Critério |
| :--- | :--- | :--- |
| Imagem informativa | Descritivo e sucinto | Ex: `alt="Astronauta Ellen Ochoa"` |
| Imagem decorativa | `alt=""` (null) | Nunca omitir o atributo |
| Imagem linkada (único conteúdo do link) | Descrever a função/destino | Ex: `alt="Ver perfil Ellen Ochoa"` |
| Imagem redundante (conteúdo já no texto) | `alt=""` (null) | Evitar redundância |
| Imagem de botão (`<input type="image">`) | Descrever a ação | Ex: `alt="Enviar busca"` |
| Imagem complexa (gráfico, mapa) | Alt resumido + link para descrição detalhada | — |
| Logo linkado à homepage | Nome da empresa | Ex: `alt="Acme Company"` |
| Imagem CSS/background | Não usar para conteúdo informativo | — |

  - Não incluir "imagem de..." ou "gráfico de..." (screen readers já anunciam "gráfico").
  - `longdesc` é **depreciado** — não usar.
- **Como Testar:** Desativar imagens e verificar se contexto da página permanece claro.

---

### 7. Accessible Media (Video & Audio)

- **O "Porquê":** Inclusão para usuários surdos, cegos e com deficiências auditivas/visuais.
- **Critérios Técnicos de Aceitação:**

| Requisito | Nível WCAG | Aplicação |
| :--- | :--- | :--- |
| **Legendas sincronizadas** | **A (WCAG 1.2.2)** | Todo áudio relevante em vídeo pré-gravado |
| **Transcrições em texto** | **A (WCAG 1.2.1)** | Podcasts, áudios standalone |
| **Audiodescrição** | **AA (WCAG 1.2.5)** | Ações visuais não narradas |
| **Controles de player acessíveis** | **A (WCAG 4.1.2)** | Todos os controles via teclado + aria-label |
| **Sem autoplay com áudio** | **AA (WCAG 1.4.2)** | Autoplay apenas sem som, com controle de parar |

  - **Formato de legenda:** Fornecer arquivos SRT ou VTT.
  - **Autoplay (BBC rule):** Vídeo pode autoplay sem som com controle visível de parar/pausar.
- **Como Testar:** Assistir sem som → validar legendas. Testar todos os controles do player com teclado.

---

### 8. ARIA & Rich Components (W3C APG)

- **O "Porquê":** Comunica estados e comportamentos para UI complexa (SPAs). ARIA expande o vocabulário semântico do HTML para casos não cobertos nativamente.
- **5 Regras Fundamentais de Uso do ARIA (WebAIM):**
  1. **Regra #1:** Se existe elemento HTML nativo equivalente, use-o.
  2. **Regra #2:** Não altere semântica nativa.
  3. **Regra #3:** Todos os controles ARIA interativos devem ser operáveis por teclado.
  4. **Regra #4:** Controles interativos não podem ser escondidos (`aria-hidden="true"` em elementos focáveis).
  5. **Regra #5:** Todo elemento interativo deve ter um nome acessível descritivo.

- **Critérios por Componente:**

#### Button
  - **ARIA:** `role="button"`. Toggles: `aria-pressed="true/false"`. Com popup: `aria-haspopup`.
  - **Disclosure/Accordion:** `aria-expanded="true|false"`.
  - **Teclado:** `Enter` ou `Space` ativa.
  - **Handoff:** `aria-label` obrigatório para botões icon-only.

#### Modal Dialog (W3C APG)
  - **ARIA:** `role="dialog"`, `aria-modal="true"`, `aria-labelledby`.
  - **Teclado:** `Tab` faz loop dentro do dialog. `Shift+Tab` navega reversamente. `Escape` fecha.
  - **Focus placement:** Primeiro elemento focável (simples) / `tabindex="-1"` em elemento estático (complexo).
  - Ao fechar: foco **retorna ao elemento trigger**.

#### Tabs
  - **ARIA:** `role="tablist"`, `role="tab"`, `aria-selected="true/false"`, `role="tabpanel"`.
  - **Teclado:** `←` `→` navega e ativa tabs. `Tab` entra/sai do grupo.

#### Accordion
  - **ARIA:** Header como `<button aria-expanded="true|false" aria-controls="panel-id">`.
  - **Teclado:** `Space/Enter` → toggle. `↑` `↓` → navega entre headers.

#### Slider
  - **Teclado:** `←` `→` incrementa/decrementa. `Home/End` → min/max. `PageUp/Down` → salto maior.

#### Live Regions (Conteúdo Dinâmico)
  - `aria-live="polite"`: Anunciar após pausa. Uso: status, notificações.
  - `aria-live="assertive"`: Anunciar imediatamente. Uso: **apenas para erros críticos**.
  - `role="alert"`: Equivalente a `aria-live="assertive"`.
  - **Regra BBC:** `aria-live` deve ser definido no carregamento da página.

#### Rotulagem ARIA (Hierarquia de Prioridade)
  - `aria-labelledby` > `aria-label` > `<label>` > `alt` > texto do elemento.
  - `aria-label` sobrescreve labels visuais — garantir que o nome acessível inclua o texto visível (WCAG 2.5.3).
  - `aria-describedby` → lido após o label — usar para instruções e mensagens de erro inline.

- **Como Testar:** Validação com screen reader (NVDA/JAWS/VoiceOver). axe DevTools. WAVE.

---

## 3. QA Tools Reference

| Ferramenta | Tipo | Uso Principal |
| :--- | :--- | :--- |
| **[WAVE (WebAIM)](https://wave.webaim.org/)** | Browser extension / API | Detecção automática: erros, alertas, estrutura |
| **[axe DevTools (Deque)](https://www.deque.com/axe/)** | Browser extension / CI/CD | Testes automatizados, integração com pipelines |
| **[WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker)** | Web tool | Verificação de ratio de contraste |
| **TPG Colour Contrast Analyser** | Desktop app | Análise de contraste em qualquer tela (eyedropper) |
| **NVDA / JAWS** | Screen reader (Windows) | Teste real de anúncios |
| **VoiceOver** | Screen reader (iOS/macOS) | Teste real mobile/desktop Apple |
| **Android Accessibility Scanner** | Mobile app | Auditoria de targets e contraste em Android |

---

## Navigation
- [RESOURCES.md](RESOURCES.md) - Reference Library.
- [HANDOFF.md](HANDOFF.md) - Engineering Specifications.
