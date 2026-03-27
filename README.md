# bataille-app

**A Parte Maldita do Design — Bataille, Dépense e a Economia do Excesso**

One-pager conceitual interativo como ferramenta de preparação para o anteprojeto de mestrado em **Design Prospectivo** na UTFPR (Curitiba/Brasil), com orientação de Fred Van Amstel, co-orientação de Caio Vassão e Kristina Höök.

---

## Sumário

- [O que é este projeto](#o-que-é-este-projeto)
- [Demo ao vivo](#demo-ao-vivo)
- [Como rodar localmente](#como-rodar-localmente)
- [Estrutura de arquivos](#estrutura-de-arquivos)
- [Arquitetura](#arquitetura)
- [Como editar o conteúdo](#como-editar-o-conteúdo)
- [Sistema de badges](#sistema-de-badges)
- [Histórico de versões](#histórico-de-versões)
- [Backlog](#backlog)
- [Handoff para nova sessão de IA](#handoff-para-nova-sessão-de-ia)

---

## O que é este projeto

Um **framework de análise conceitual reutilizável** — não apenas um documento estático. O one-pager funciona como um instrumento de pensamento: mapeia conexões entre autores, tensiona campos teóricos e sinaliza lacunas na argumentação.

Foi construído com separação estrita entre **conteúdo**, **apresentação** e **estrutura** — o que permite iterar rapidamente sobre o texto sem tocar no código, e atualizar o design sem perder dados.

### Conceito central analisado

**Dépense (Bataille)** — a economia do excesso, o gasto improdutivo, e o que o sistema precisa e nega ao mesmo tempo. A análise conecta isso a Metadesign, Soma Design, Teoria Crítica, Filosofia da Libertação, Design Prospectivo e mais de 20 campos teóricos.

---

## Demo ao vivo

```
https://entrepausadesign-ai.github.io/bataille-app/
```

---

## Como rodar localmente

**Pré-requisito:** Python 3 (já instalado no macOS por padrão)

```bash
# 1. Clone o repositório
git clone https://github.com/entrepausadesign-ai/bataille-app.git
cd bataille-app

# 2. Inicie o servidor local
python3 -m http.server 8080

# 3. Abra no browser
# http://localhost:8080
```

> **Por que servidor local?** Os arquivos `data.js` e `style.css` são carregados via `fetch` implícito pelo browser. Abrir `index.html` diretamente com dois cliques (`file://`) bloqueia esse carregamento por segurança. O servidor Python resolve isso em um comando.

---

## Estrutura de arquivos

```
bataille-app/
├── index.html          # Templates e lógica de renderização (view layer)
├── data.js             # Todo o conteúdo como objetos JS (data layer)
├── style.css           # Todos os estilos visuais (style layer)
├── README.md           # Este documento
├── PRD.md              # Product Requirements Document — decisões técnicas
└── prompt-guide.md     # Guia de transposição para nova sessão de IA
```

**Regra de ouro:** só `data.js` muda para editar conteúdo. `index.html` muda para adicionar novos tipos de bloco. `style.css` muda para ajustes visuais globais. As três camadas nunca se misturam.

---

## Arquitetura

### Stack

| Tecnologia | Versão | Papel |
|---|---|---|
| HTML semântico | — | Estrutura e acessibilidade |
| CSS puro | — | Toda a apresentação visual |
| JavaScript vanilla | ES6+ | Renderização e interatividade |
| Chart.js | 4.4.1 | Visualização de dados (CDN) |

Sem build step. Sem npm. Sem framework. Zero dependências locais.

### Separação de camadas

```
data.js          →    index.html         →    style.css
(o quê)               (como renderizar)        (como parece)

DOC.thinkers     →    renderThinkerGrid()  →   .card.lilac
DOC.badges       →    renderBadges()       →   .badge-pill
DOC.clusters     →    b11()                →   .cluster-item
```

### Fluxo de renderização

```
1. Browser carrega index.html
2. data.js é executado — cria o objeto global DOC
3. renderAll() é chamado:
   - renderHeader() → preenche <header>
   - renderFooter() → preenche <footer>
   - renderLegend() → preenche legenda de badges
   - b0() a b12()  → constroem os blocos e injetam em #blocks
4. Usuário interage → toggleViz(), âncoras de navegação
```

### Blocos de renderização (index.html)

| Função | Seção | Dados |
|---|---|---|
| `b0()` | Síntese integradora | `DOC.synthesis` |
| `b1()` | Equívoco fundador + Ficção da escassez | `DOC.coreProblem`, `DOC.scarcityMyth` |
| `b2()` | Economia invisível | `DOC.invisibleEconomy` |
| `b3()` | Lógica do colapso | `DOC.collapseLogic` |
| `renderThinkerGrid()` | Referências fundantes + Lentes de análise | `DOC.thinkers`, `DOC.thinkersExpanded` |
| `b5()` | Complexidade + Design connections | `DOC.complexity`, `DOC.designConnections` |
| `b6()` | Design Prospectivo | `DOC.prospectiveDesign` |
| `b7()` | Estratégias metodológicas | `DOC.strategies` |
| `b8()` | Mapa de conexões | `DOC.connections` |
| `b9()` | Tensão produtiva | `DOC.productiveTension` |
| `b10()` | Síntese Latour | `DOC.latourSynthesis` |
| `b11()` | Tensão / redução (clusters) | `DOC.tensionReduction` |
| `b12()` | Auditoria de conexões | `DOC.connectionAudit` |

---

## Como editar o conteúdo

### Editar um texto existente

Abra `data.js`, encontre o campo e edite diretamente. Exemplo:

```js
DOC.coreProblem.body = "Novo texto aqui...";
```

Salve e recarregue o browser. Sem compilação.

### Adicionar um novo pensador ao grid de lentes

```js
DOC.thinkersExpanded.push({
  id: "novo-autor",
  color: "lilac",              // lilac | teal | purple | coral | gray
  label: "Nome do Autor",
  title: "Conceito central",
  body: "Descrição da contribuição teórica...",
  connection: "→ Conexão com sua epistemologia de vida pessoal",
  badges: [
    { type: "campo",         text: "filosofia"    },
    { type: "tradicao",      text: "decolonial"   },
    { type: "epistemologia", text: "materialista" },
    { type: "escala",        text: "macro"        },
  ]
});
```

### Adicionar um item urgente (flag visual)

```js
DOC.thinkersExpanded[i].urgent     = true;
DOC.thinkersExpanded[i].urgentNote = "Texto do alerta que aparece no card";
```

### Adicionar um novo bloco inteiro

1. Crie o objeto em `data.js`:

```js
DOC.meuNovoBloco = {
  label: "categoria / subcategoria",
  title: "Título do bloco",
  body:  "Conteúdo...",
  badges: [ /* opcional */ ]
};
```

2. Crie o renderizador em `index.html`:

```js
function bNovo(d) {
  return full(card('teal',
    lbl(d.label) + renderBadges(d.badges) + h2(d.title) + p(d.body)
  ));
}
```

3. Chame dentro de `renderAll()`:

```js
sec('sec-novo', bNovo(DOC.meuNovoBloco)),
```

### Atualizar a versão

```js
// no final de data.js, após suas adições:
DOC.meta.version = "v1.8";
```

---

## Sistema de badges

Cinco dimensões de classificação, cada uma com cor própria:

| Tipo | Cor | Pergunta respondida | Exemplos |
|---|---|---|---|
| `campo` | Azul | De qual disciplina vem? | filosofia, design, neurociências, STS |
| `tradicao` | Roxo | De qual escola/tradição? | teoria crítica, feminismo, esquizoanálise |
| `metodo` | Verde | Como produz conhecimento? | ANT, somático, cartográfico, clínico |
| `epistemologia` | Coral | Qual sua posição no mundo? | materialista, encorporado, vitalista |
| `escala` | Âmbar | Em que escala opera? | micro, meso, macro, trans-escalar |

### Expansões de taxonomia realizadas

Ao longo das versões, os seguintes termos foram adicionados à taxonomia porque os padrões existentes não eram suficientemente precisos:

- `teoria cultural` — Fisher (Cultural Studies ≠ filosofia nem sociologia)
- `economia política` — Varoufakis (mais preciso que "economia")
- `arquitetura` — Vassão (filiação dupla design+arquitetura)
- `HCI` — Höök (Human-Computer Interaction)
- `somaestética` — Shusterman (entre pragmatismo e filosofia do corpo)
- `neurociências` — Damasio, Varela, Thompson
- `cibernética` — Wiener, Von Foerster
- `fenomenologia do corpo` — Merleau-Ponty, Reich
- `analéctica` — método específico de Dussel
- `esquizoanálise` — Deleuze/Guattari (distinta da psicanálise)
- `vitalista` — Reich, Toro (corpo como princípio ontológico)
- `cartográfico` — método Deleuze/Guattari
- `trans-escalar` — autores que recusam operar numa escala só
- `liberal-revisado` — Fukuyama pós-autocrítica
- `pragmatismo latino` — Van Amstel / contexto UTFPR
- `molecular` — escala sub-micro de Deleuze/Guattari

---

## Histórico de versões

### v1.7-p3 — março 2026
**Navegação por âncoras + dropdown na toolbar**
- Botão "ir para ▾" na toolbar com 9 destinos diretos
- Menu CSS puro (`:focus-within`), acessível por teclado e leitor de tela
- IDs semânticos injetados em todas as seções principais do `renderAll()`
- Zero JavaScript adicional

### v1.6 — março 2026
**Versão submetida pelo usuário como base para auditoria e UX review**
- Arquivos verificados: integridade ✓, cross-references ✓, separação view/data ✓
- Issue menor identificado: duas declarações de versão em `data.js` (residual)

### v1.5 — março 2026
**Refatoração — depuração e eficiência de código**
- `renderBadges()` e `BADGE_TYPES` movidos de `data.js` para `index.html` (separação view/data restaurada)
- `b4()` e `b4b()` unificadas em `renderThinkerGrid(arr, label, showConnection)` — DRY
- `<style>` inline removido do `<head>` — CSS consolidado no `style.css`
- `sLabel`, `.cluster-label`, `.cluster-item` viram classes CSS em vez de inline styles
- 6 atribuições `DOC.meta.version` redundantes removidas
- 15 campos `connection_audit` de itens individuais removidos (redundantes com `DOC.connectionAudit`)
- Total: −40 linhas, 10 issues arquiteturais resolvidos

### v1.4.4 — março 2026
**Haraway — flag de urgência**
- Sistema de urgência implementado: `urgent: true` + `urgentNote` em qualquer entrada
- Banner `⚑ urgente` renderizado antes do título — garante visibilidade antes do conteúdo
- Ativado em: card da Haraway (lentes de análise) e item Chthuluceno (b11)
- Sistema genérico — qualquer entrada futura com `urgent: true` ativa automaticamente

### v1.4.3 — março 2026
**Dussel expandido + auditoria de connections**
- Card de Dussel reescrito com `exterioridade` como conceito central explícito
- Analéctica nomeada e distinguida da dialética hegeliana
- `connection` amarra Dussel à zona norte, Missão Integral e Teologia da Libertação
- Auditoria completa dos 14 `connection` fields: 5 fortes ✓, 7 lacunas ⚠
- `DOC.connectionAudit` como objeto consultável renderizado em `b12()`

### v1.4.2 — março 2026
**Enrique Dussel + Filosofia da Libertação**
- Adicionado ao `thinkersExpanded` como décimo pensador
- Badges: `analéctica` (novo tipo de método), `decolonial`, `teoria crítica`
- `connection` conecta explicitamente à trajetória pessoal do pesquisador

### v1.4.1 — março 2026
**Varela + Maturana**
- Item de Autopoiese expandido: agora nomeia explicitamente Maturana + Varela e distingue contribuições
- Varela adicionado como entrada separada com foco em **enação** (*The Embodied Mind*, 1991)
- Badges próprios: `neurociências`, `fenomenologia do corpo`, `encorporado`

### v1.4 — março 2026
**Somaestética + bloco de tensão/redução com clusters**
- `body2` adicionado ao bloco de complexidade com Shusterman/Somaestética
- Novo bloco `DOC.tensionReduction` com 7 clusters e 13+ conceitos
- Clusters: autopoiese/autorregulação, corpo, rizoma, sentido/esperança, psicanálise, mundos emergentes, neurociências/IA
- Renderizador `b11()` com estrutura de clusters aninhados
- Novas taxonomias: `somaestética`, `neurociências`, `cibernética`, `fenomenologia do corpo`, `esquizoanálise`, `vitalista`, `cartográfico`

### v1.3 — março 2026
**Sistema de badges epistemológicos**
- `BADGE_TYPES` com 5 dimensões e paleta de cores própria
- `renderBadges()` global aplicado a thinkers e blocos conceituais
- 168 atribuições de badges distribuídas por autores e blocos
- Legenda visual da taxonomia na toolbar
- 8 expansões de taxonomia documentadas e sinalizadas

---

## Backlog

### v1.7 — UX interativo (em progresso)
- [ ] **P1** — Accordion nos thinkers expandidos (details/summary nativo)
- [ ] **P2** — Scroll horizontal com snap nos clusters do b11
- [ ] **P4** — Filtro por badge: clicar filtra o documento inteiro

### Conteúdo
- [ ] Nomear `saberes situados` (Haraway) como credencial epistemológica do anteprojeto
- [ ] Seção "por que Design Prospectivo / por que agora" — lacuna identificada na auditoria
- [ ] Seção de referências bibliográficas estruturadas (`DOC.references[]`)
- [ ] Glossário de conceitos-chave

### Técnico
- [ ] Consolidar as duas declarações de versão em `data.js` em uma só
- [ ] Exportação real via `html2canvas + jsPDF` (mantém layout, sem diálogo do browser)
- [ ] Modo `?print=true` na URL que remove toolbar
- [ ] `data/` como pasta com múltiplos arquivos por versão para comparação

---

## Handoff para nova sessão de IA

Para continuar o desenvolvimento em uma nova janela de chat, envie os três arquivos principais e use o seguinte prompt de abertura:

```
Estou continuando o desenvolvimento do bataille-app, um one-pager conceitual
para anteprojeto de mestrado em Design Prospectivo.

Stack: HTML + CSS + JS vanilla + Chart.js 4.4.1. Zero build step.
Versão atual: v1.7-p3.

Regras arquiteturais:
- Só data.js muda para editar conteúdo
- index.html muda para novos tipos de bloco
- style.css muda para estilos globais
- renderBadges() e BADGE_TYPES vivem no index.html (view layer)
- Cada bloco tem ID de seção para navegação por âncora

A próxima tarefa é: [DESCREVA AQUI]

Segue o conteúdo dos três arquivos:
[COLE data.js]
[COLE index.html]
[COLE style.css]
```

Veja também `prompt-guide.md` para referência completa de sintaxe e padrões.

---

## Créditos

**Pesquisador / autor do conteúdo:** Thiago — designer formado pela ESDI/UERJ (2017), candidato ao mestrado em Design Prospectivo na UTFPR.

**Orientação esperada:** Fred Van Amstel (UTFPR) · Caio Vassão · Kristina Höök

**Desenvolvimento assistido por IA:** Claude (Anthropic) — Sonnet 4.6

---

*"O que você está disposto a gastar sem justificar para que a dissertação alimente alguém além de você?"*
