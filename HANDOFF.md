# Accessibility Handoff Guide (V6.0)
**Bridge Design-to-Engineering: W3C ARIA Patterns + BBC + WebAIM**

> Atualizado em 2026-05-14 com especificações extraídas de W3C APG, WebAIM e BBC Mobile Guidelines.

**Não implementar sem consultar os padrões de teclado abaixo.**

---

## 1. Component Interaction Patterns (W3C APG — Spec Completa)

### Button

| Atributo ARIA | Valor | Quando usar |
| :--- | :--- | :--- |
| `role="button"` | — | Em `<div>` ou `<span>` customizados (prefira `<button>` nativo) |
| `aria-pressed` | `"true"` / `"false"` | Toggle buttons |
| `aria-expanded` | `"true"` / `"false"` | Accordion headers, disclosure widgets |
| `aria-haspopup` | `"dialog"` / `"menu"` / `"listbox"` | Botões que abrem popups |
| `aria-label` | texto descritivo | **Obrigatório** para botões icon-only |

**Teclado:** `Enter` ou `Space` ativa.
**Anti-pattern:** Nunca usar `<div role="button">` sem `tabindex="0"` + handlers `keydown` para Enter/Space.

---

### Modal Dialog (W3C APG — Especificação Completa)

**Estrutura HTML mínima:**
```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirmar exclusão</h2>
  <p id="dialog-desc">Esta ação não pode ser desfeita.</p>
  <!-- conteúdo -->
  <button>Cancelar</button>
  <button>Confirmar</button>
</div>
```

**Atributos ARIA obrigatórios:**

| Atributo | Regra |
| :--- | :--- |
| `role="dialog"` | **Obrigatório** no container do modal |
| `aria-modal="true"` | **Obrigatório** — apenas quando código impede interação fora E CSS obscurece o fundo |
| `aria-labelledby` | **Obrigatório** — referencia o `id` do título visível |
| `aria-label` | Alternativa quando não há título visível |
| `aria-describedby` | **Opcional** — omitir se conteúdo tem estrutura complexa (listas/tabelas) |

**Comportamento de teclado (W3C APG obrigatório):**

| Tecla | Comportamento |
| :--- | :--- |
| `Tab` | Foco no próximo elemento focável DENTRO do dialog. No último → volta ao primeiro (loop) |
| `Shift + Tab` | Foco no elemento anterior DENTRO do dialog. No primeiro → vai ao último (loop) |
| `Escape` | Fecha o modal |

**Protocolo de gerenciamento de foco:**

| Cenário | Onde colocar o foco ao abrir |
| :--- | :--- |
| Conteúdo simples | Primeiro elemento interativo |
| Conteúdo complexo (listas/tabelas) | Elemento estático com `tabindex="-1"` no início do conteúdo |
| Conteúdo longo (primeiro botão fora da viewport) | `tabindex="-1"` no título ou parágrafo inicial |
| Ação destrutiva irreversível (delete, payment) | Ação menos destrutiva (ex: "Cancelar") |
| Dialog informativo | Elemento mais frequentemente usado (ex: "OK") |

**Ao fechar:** Foco retorna ao **elemento trigger** que abriu o modal.
**Obrigatório:** Incluir botão visível (X ou "Cancelar") que fecha o modal.

---

### Tabs

```html
<div role="tablist" aria-label="Seções do produto">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">Especificações</button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2">Avaliações</button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">...</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>...</div>
```

| Atributo | Elemento | Valor |
| :--- | :--- | :--- |
| `role="tablist"` | Container das abas | — |
| `role="tab"` | Cada aba | — |
| `aria-selected` | Cada aba | `"true"` / `"false"` |
| `aria-controls` | Cada aba | ID do painel correspondente |
| `role="tabpanel"` | Cada painel | — |
| `aria-labelledby` | Cada painel | ID da aba correspondente |

**Teclado:**

| Tecla | Comportamento |
| :--- | :--- |
| `Tab` | Entra no grupo (foca na aba ativa) / sai do grupo |
| `←` / `→` | Navega e **ativa** a aba anterior/seguinte |
| `Home` | Primeira aba |
| `End` | Última aba |

> **Distinção crítica:** Este padrão é para "application tabs" que mudam conteúdo dinamicamente. Se os "tabs" são links para páginas diferentes, usar `Tab + Enter` (comportamento de link normal).

---

### Accordion

```html
<h3>
  <button aria-expanded="false" aria-controls="panel-1">
    Seção 1
  </button>
</h3>
<div id="panel-1" hidden>
  <!-- conteúdo -->
</div>
```

| Atributo | Elemento | Valor |
| :--- | :--- | :--- |
| `aria-expanded` | `<button>` header | `"true"` / `"false"` |
| `aria-controls` | `<button>` header | ID do painel |
| `hidden` ou `aria-hidden` | Painel | Quando fechado |

**Teclado:** `Space` / `Enter` → toggle. `↑` `↓` → navega entre headers.

---

### Slider

**Teclado:**

| Tecla | Comportamento |
| :--- | :--- |
| `←` `→` ou `↑` `↓` | Incrementa / decrementa valor |
| `Home` | Valor mínimo |
| `End` | Valor máximo |
| `PageUp` / `PageDown` | Salto maior (ex: 10%) |

**Double-headed slider:** `Tab` / `Shift+Tab` alterna entre os dois handles.

---

### Select / Combobox

**Teclado (select nativo):**

| Tecla | Comportamento |
| :--- | :--- |
| `↑` `↓` | Navega opções |
| `Enter` | Seleciona e fecha |
| `Escape` | Fecha sem selecionar |
| Digitação | Filtra / salta para opção |

> **Anti-pattern:** Evitar `<select>` com `onChange` que dispara navegação (jump menu). Substituir por `<select>` + botão "Ir" separado — navegar com setas não deve causar ações.

---

### Live Regions (Conteúdo Dinâmico)

| Valor `aria-live` | Quando usar | Exemplo |
| :--- | :--- | :--- |
| `"polite"` | Atualizações não críticas, após pausa | Status de upload, mensagens de chat, estoque |
| `"assertive"` | Erros críticos — interrompe leitura atual | Erro de validação de segurança, falha de pagamento |
| `"off"` | Desativa anúncios | Mudanças irrelevantes |

**Roles equivalentes:**
- `role="alert"` = `aria-live="assertive"` + `aria-atomic="true"`
- `role="log"` = Live region com histórico (chat)
- `role="status"` = `aria-live="polite"` (para status messages)

> **Regra de implementação:** `aria-live` deve ser definido no **carregamento inicial da página**. Injetar o atributo via JavaScript após carregamento não funciona de forma confiável.

---

### Media (Video/Audio)

| Requisito | Nível | Spec de Handoff |
| :--- | :--- | :--- |
| **Legendas sincronizadas** | WCAG 1.2.2 (A) | Arquivo VTT ou SRT |
| **Audiodescrição** | WCAG 1.2.5 (AA) | Especificar se necessário faixa AD separada |
| **Transcrição em texto** | WCAG 1.2.1 (A) | Para áudios e podcasts |
| **Controles acessíveis** | WCAG 4.1.2 (A) | Todos os controles via teclado + `aria-label` |
| **Sem autoplay com áudio** | WCAG 1.4.2 (AA) | Autoplay apenas sem som, com controle de parar |
| **Controle de volume** | BBC guideline | Controle independente do sistema |

---

## 2. Error Management Protocol (Handoff Spec)

### HTML Reference Pattern (BBC + WebAIM)

```html
<!-- 1. Região aria-live para anunciar erros (deve existir no load) -->
<div aria-live="polite" id="form-errors" class="sr-only"></div>

<!-- 2. Formulário -->
<form>
  <div class="field-group">
    <label for="email">Email <span aria-hidden="true">*</span></label>
    <!-- aria-required anuncia "obrigatório" -->
    <input
      id="email"
      type="email"
      aria-required="true"
      aria-invalid="true"
      aria-describedby="email-error"
    >
    <!-- Mensagem de erro inline -->
    <span id="email-error" role="alert">
      Erro: Insira um endereço de email válido (ex: nome@dominio.com)
    </span>
  </div>
</form>
```

### Protocolo de Validação (3 passos obrigatórios)
1. **Anunciar:** Erro visível + anunciado por screen reader (`aria-live` region ou `role="alert"`).
2. **Identificar:** Indicar especificamente qual campo precisa correção (`aria-invalid="true"` + mensagem inline).
3. **Permitir correção:** Mover foco para o primeiro campo inválido + manter form preenchido.

---

## 3. Design Inspection Checklist V6.0 (Design Ops)

### Visual & Contrast
- [ ] **Texto normal:** Contraste ≥ **4.5:1** (WCAG 1.4.3 AA)
- [ ] **Texto grande** (≥ 18pt ou ≥ 14pt bold): Contraste ≥ **3:1**
- [ ] **Elementos de UI** (bordas de input, ícones funcionais): Contraste ≥ **3:1** (WCAG 1.4.11)
- [ ] **Gradientes e imagens de fundo:** Texto sobre gradiente com overlay testado no ponto de menor contraste
- [ ] **Cor como único diferenciador:** Combinada com ícone, forma ou sublinhado (links, erros, estados)

### Target Size & Focus
- [ ] **Botões/links primários:** Mínimo **44x44px** (recomendado para todos)
- [ ] **Mínimo absoluto (WCAG 2.5.8):** 24x24px com 24px de espaço livre ao redor
- [ ] **iOS nativo:** 44x44pt | **Android:** 48x48dp com 8dp entre controles | **BBC Mobile:** 7x7mm mínimo físico
- [ ] **Focus ring:** Visível e com contraste ≥ 3:1 contra o fundo adjacente
- [ ] **Focus ring:** Não usar `outline: none` sem substituto equivalente visível
- [ ] **Links adjacentes ao mesmo destino:** Combinados em único target de toque

### Structure & Navigation
- [ ] **Skip link:** "Ir para o conteúdo" visível no topo da página
- [ ] **Landmarks:** `<main>`, `<nav>`, `<header>`, `<footer>` definidos
- [ ] **Múltiplos navs:** Diferenciados por `aria-label`
- [ ] **Hierarquia H1→H2→H3:** Preservada sem pular níveis
- [ ] **Título de página/tela:** Único e descritivo por página
- [ ] **Ordem de Tab:** Documentada e segue fluxo visual lógico
- [ ] **tabindex positivo (>0):** Nunca usado — destrói ordem natural

### Forms & Inputs
- [ ] **Labels:** Programaticamente associados a cada input (`<label for>` ou `aria-labelledby`)
- [ ] **Campos obrigatórios:** `required` ou `aria-required="true"` (não apenas asterisco visual)
- [ ] **Erros:** `aria-invalid="true"` + `aria-describedby` apontando para mensagem de erro
- [ ] **Mensagem de erro:** Visível + anunciada por SR + indica especificamente o que corrigir
- [ ] **Autocomplete:** Atributo definido para campos de dados pessoais (WCAG 1.3.5)
- [ ] **Tipo de input correto:** `type="email"`, `type="tel"`, `type="number"` quando aplicável

### ARIA & Rich Components
- [ ] **Alt text:** Definido para todas as imagens funcionais (null `alt=""` para decorativas)
- [ ] **Botões icon-only:** `aria-label` definido
- [ ] **Modais:** `role="dialog"`, `aria-modal="true"`, `aria-labelledby`, focus trap, retorno de foco ao fechar
- [ ] **Tabs:** `role="tablist"`, `role="tab"`, `aria-selected`, `role="tabpanel"` estruturados
- [ ] **Accordions:** `aria-expanded` atualizado no toggle
- [ ] **Live regions:** `aria-live` definido no load da página para regiões dinâmicas
- [ ] **Erros críticos:** `role="alert"` ou `aria-live="assertive"` (use com cautela)

### Media
- [ ] **Vídeos:** Legendas (VTT/SRT) especificadas
- [ ] **Áudios:** Transcrição disponível
- [ ] **Audiodescrição:** Especificada se há informação visual não narrada
- [ ] **Controles de player:** Todos acessíveis por teclado + rotulados

---

## Navigation
- [SKILL.md](SKILL.md) - Skills Framework.
- [RESOURCES.md](RESOURCES.md) - Technical Library.
