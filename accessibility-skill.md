# Taxonomia de Habilidades de Acessibilidade (V3.1 - Cumulativa Final)
**Framework de Governança para Design Ops & Produto**

Esta versão consolida 100% do conhecimento das versões 1, 2 e 3, mapeando as 8 competências fundamentais aos critérios de sucesso da **WCAG 2.1/2.2**.

## 1. Resumo Executivo: Mapeamento de Conformidade

| Categoria | Habilidade Core / Especialista | Impacto Estratégico | Nível WCAG | Ref. Principal |
| :--- | :--- | :--- | :--- | :--- |
| **Estrutura** | 1. Hierarquia e Semântica | Navegação Estrutural e SEO | **A / AA** | 1.3.1, 2.4.6 |
| **Estrutura** | 2. Listas e Tabelas de Dados | Integridade e Organização | **A** | 1.3.1 |
| **Design** | 3. Contraste e Discernibilidade | Legibilidade Universal | **AA** | 1.4.3, 1.4.11 |
| **Design** | 4. Gerenciamento de Foco (Pro) | Eficiência e Focus Trapping | **A / AA** | 2.1.1, 2.4.7 |
| **Formulários** | 5. Gestão de Inputs e Erros | Conversão e Fricção | **A / AA** | 3.3.1, 3.3.3 |
| **Conteúdo** | 6. Texto Alternativo (Alt Text) | Alcance e Acessibilidade Visual | **A** | 1.1.1 |
| **Conteúdo** | 7. Mídia (Vídeo e Áudio) | Inclusão Sensorial | **A / AA** | 1.2.2, 1.2.5 |
| **Dinâmico** | 8. ARIA e Estados Dinâmicos | Robustez em Interfaces Ricas | **A / AA** | 4.1.2, 4.1.3 |

---

## 2. Detalhamento de Competências

### 1. Semântica HTML e Hierarquia de Títulos
*   **O "Porquê":** Define a "Árvore de Acessibilidade". Permite que usuários de leitores de tela entendam a estrutura e "saltem" entre seções.
*   **Critérios Técnicos:**
    *   **Marcos (Landmarks):** Uso de `<main>`, `<nav>`, `<header>`.
    *   **Títulos:** Ordem lógica H1 > H2 > H3. Nunca pular níveis por estética.
*   **Como Testar:** Auditoria de Landmarks e Outline da página.
> [!NOTE]
> Referência: [WCAG 1.3.1 (A)](https://guia-wcag.com/#131) | [WCAG 2.4.6 (AA)](https://guia-wcag.com/#246)

### 2. Estrutura de Listas e Tabelas de Dados
*   **O "Porquê":** Informa ao usuário o número de itens e a relação entre dados complexos, evitando a carga cognitiva de processar dados isolados.
*   **Critérios Técnicos:**
    *   **Listas:** Uso de `<ul>`/`<ol>` para grupos de 2+ itens.
    *   **Tabelas:** Uso de `<th>` com `scope` e `<caption>`. Nunca usar tabelas para layout.
*   **Como Testar:** Verificação se o leitor de tela anuncia "Lista, X itens" ou lê o cabeçalho correto em cada célula da tabela.
> [!NOTE]
> Referência: [WCAG 1.3.1 (A)](https://guia-wcag.com/#131)

### 3. Contraste e Discernibilidade
*   **O "Porquê":** Garante leitura em condições adversas (baixa visão ou luz solar).
*   **Critérios Técnicos:**
    *   **Texto:** 4.5:1 (normal) / 3:1 (grande).
    *   **UI:** 3:1 para bordas de botões e ícones essenciais.
    *   **Indicadores:** Nunca usar apenas cor para transmitir status (ex: erro).
*   **Como Testar:** Colour Contrast Analyser e simulação de daltonismo.
> [!NOTE]
> Referência: [WCAG 1.4.3 (AA)](https://guia-wcag.com/#143) | [WCAG 1.4.11 (AA)](https://guia-wcag.com/#1411)

### 4. Gerenciamento de Foco e Navegação por Teclado
*   **O "Porquê":** Fundamental para quem não usa mouse. O foco deve ser visível e controlado.
*   **Critérios Técnicos:**
    *   **Focus Trapping:** O foco deve ficar "preso" dentro de modais/overlays.
    *   **Retorno de Foco:** Ao fechar um modal, o foco volta para o botão disparador.
    *   **Links vs Buttons:** `<a>` para navegação, `<button>` para ações de estado.
*   **Como Testar:** Navegação exclusiva por `Tab`; verificação do indicador de foco visível.
> [!NOTE]
> Referência: [WCAG 2.1.1 (A)](https://guia-wcag.com/#211) | [WCAG 2.4.7 (AA)](https://guia-wcag.com/#247)

### 5. Semântica de Inputs e Gestão de Erros
*   **O "Porquê":** Reduz a fricção no preenchimento e evita o abandono de tarefas.
*   **Critérios Técnicos:**
    *   **Vínculos:** `<label for="ID">` conectado ao `<input id="ID">`.
    *   **Mensagens Dinâmicas:** Uso de `role="alert"` e `aria-describedby` para vincular o erro ao campo.
    *   **Sugestões:** O sistema deve dizer *como* corrigir o erro (ex: formato de data).
*   **Como Testar:** Tente submeter o formulário vazio/errado e verifique se o erro é anunciado imediatamente.
> [!NOTE]
> Referência: [WCAG 3.3.1 (A)](https://guia-wcag.com/#331) | [WCAG 3.3.3 (AA)](https://guia-wcag.com/#333)

### 6. Texto Alternativo (Alt Text)
*   **O "Porquê":** Entrega o valor da imagem para quem não a vê. Melhora SEO e resiliência da página.
*   **Critérios Técnicos:**
    *   **Descritivo:** Curto e focado na *função* da imagem.
    *   **Decorativo:** Uso obrigatório de `alt=""` para ignorar elementos estéticos.
*   **Como Testar:** Desabilite imagens e verifique se o contexto permanece.
> [!NOTE]
> Referência: [WCAG 1.1.1 (A)](https://guia-wcag.com/#111)

### 7. Mídia Acessível (Vídeo e Áudio)
*   **O "Porquê":** Inclusão de usuários surdos e cegos em conteúdos multimídia.
*   **Critérios Técnicos:**
    *   **Legendas:** Sincronizadas para todo áudio relevante (Nível A).
    *   **Audiodescrição:** Descrição das ações visuais não narradas (Nível AA).
    *   **Transcrições:** Texto completo para podcasts ou vídeos.
*   **Como Testar:** Assista sem som e apenas com áudio para validar a compreensão.
> [!NOTE]
> Referência: [WCAG 1.2.2 (A)](https://guia-wcag.com/#122) | [WCAG 1.2.5 (AA)](https://guia-wcag.com/#125)

### 8. ARIA e Componentes Dinâmicos (Advanced)
*   **O "Porquê":** Comunica mudanças de estado em tempo real (SPAs).
*   **Critérios Técnicos:**
    *   **Estados:** Uso de `aria-expanded`, `aria-selected`, `aria-hidden`.
    *   **Live Regions:** `aria-live="polite"` para notificações não disruptivas.
*   **Como Testar:** Verificação de anúncios do leitor de tela ao abrir menus ou carregar dados.
> [!NOTE]
> Referência: [WCAG 4.1.2 (A)](https://guia-wcag.com/#412) | [WCAG 4.1.3 (AA)](https://guia-wcag.com/#413)

---

## 3. Metodologia de Governança (Design Ops)

1.  **Component Library:** Mapear cada componente do Design System aos critérios WCAG acima.
2.  **QA de Acessibilidade:** Auditorias com Axe DevTools e validação com NVDA/VoiceOver.
3.  **Handoff:** Especificar `Alt Text`, ordem de `Tab` e `Aria-labels` no design.

---
**Documento V3.1 Final** | [Guia WCAG](https://guia-wcag.com/)
