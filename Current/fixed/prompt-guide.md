# Prompt-guia de transposição
## Bataille One-Pager Web App · para nova janela de chat

---

## Contexto do projeto

Estamos construindo um **web app local de one-pager acadêmico** para o anteprojeto de mestrado em Design Prospectivo (UTFPR). O documento central conecta Bataille/Dépense, Metadesign, Soma Design, Design Prospectivo e teoria crítica.

O app tem **separação estrita de camadas**:
- `data.js` — todo o conteúdo como objetos JS estruturados
- `style.css` — todos os estilos visuais
- `index.html` — templates declarativos via **petite-vue** (sem build, CDN)

**Stack:** HTML + CSS + vanilla JS + petite-vue 0.4.1 + Chart.js 4.4.1

---

## Arquivos do projeto (cole cada um)

### 1. `data.js`
```
[COLE O CONTEÚDO COMPLETO DO data.js AQUI]
```

### 2. `style.css`
```
[COLE O CONTEÚDO COMPLETO DO style.css AQUI]
```

### 3. `index.html`
```
[COLE O CONTEÚDO COMPLETO DO index.html AQUI]
```

---

## Instrução de execução local

```bash
# Opção A — Python (qualquer sistema)
cd pasta-do-projeto
python3 -m http.server 8080
# abrir: http://localhost:8080

# Opção B — Node (se preferir)
npx serve .
```

> Não abre direto como file:// porque os módulos JS precisam de servidor HTTP.

---

## Arquitetura de dados — como adicionar um novo bloco

Cada bloco do documento é um **objeto no `DOC`** dentro de `data.js`.

Para adicionar um bloco novo:

**1. Em `data.js`, adicione ao objeto `DOC`:**
```js
const DOC = {
  // ... blocos existentes ...

  meuNovoBloco: {
    label: "categoria / subcategoria",
    title: "Título do bloco",
    body: "Texto principal...",
    body2: "Parágrafo adicional (opcional)",
    items: [                     // se for lista
      { name: "Item", body: "Descrição" }
    ],
  },
};
```

**2. Em `index.html`, adicione o template no local desejado:**
```html
<!-- BLOCO X — NOME -->
<div class="full">               <!-- ou grid, grid-2 -->
  <div class="card teal">        <!-- cor: accent | gold | teal | purple | dark -->
    <div class="card-label">{{ meuNovoBloco.label }}</div>
    <h2>{{ meuNovoBloco.title }}</h2>
    <p>{{ meuNovoBloco.body }}</p>
    <div v-for="item in meuNovoBloco.items" style="margin-top:8px">
      <p><strong>{{ item.name }}:</strong> {{ item.body }}</p>
    </div>
  </div>
</div>
```

> Só `data.js` e `index.html` mudam. `style.css` não precisa ser tocado.

---

## Classes de layout disponíveis

| Classe       | Comportamento                        |
|--------------|--------------------------------------|
| `.full`      | largura total                        |
| `.grid-2`    | 2 colunas iguais                     |
| `.grid`      | 3 colunas iguais                     |

## Variantes de card (cor da borda esquerda)

| Classe  | Cor       | Uso sugerido                     |
|---------|-----------|----------------------------------|
| `accent`| vermelho  | problemas, tensões, colapso      |
| `gold`  | dourado   | síntese, prospectivo, esperança  |
| `teal`  | verde-azul| conexões, design, metodologia    |
| `purple`| roxo      | teoria, pensadores               |
| `dark`  | escuro    | destaque máximo, tese central    |
| (none)  | neutro    | conexões, mapas, rodapés         |

## Elementos inline

```html
<!-- Tag colorida -->
<span class="tag teal">texto</span>   <!-- teal | red | gold | purple -->

<!-- Citação com fonte -->
<div class="quote-block">
  <p>Texto da citação...</p>
  <div class="source">— Autor, Obra, Ano</div>
</div>

<!-- Fluxo de setas -->
<div class="arrow-flow">
  <span class="arrow-step hot">passo 1</span>
  <span class="arrow-sep">→</span>
  <span class="arrow-step hot">passo 2</span>
</div>

<!-- Conector de relação -->
<div class="connector">
  <div class="connector-dot" style="background:#1a6e5a"></div>
  <p><strong>A → B:</strong> descrição</p>
</div>
```

---

## Petite-vue — referência rápida

```html
<!-- interpolação -->
{{ meta.title }}

<!-- atributo dinâmico -->
:class="'card ' + t.color"
:style="'background:' + c.color"

<!-- loop -->
v-for="item in array"
v-for="(item, i) in array"

<!-- condicional -->
v-show="vizVisible"      <!-- mantém no DOM, só esconde -->
v-if="condicao"          <!-- remove do DOM -->

<!-- evento -->
@click="toggleViz()"

<!-- template sem elemento wrapper -->
<template v-for="...">
  ...
</template>
```

---

## Pedidos frequentes para o assistente

**Adicionar um novo bloco de conteúdo:**
> "Adicione um bloco chamado `[nome]` com os campos `[label, title, body]` ao `data.js` e o template correspondente no `index.html`, como card `[cor]`, em posição `[full | grid-2 | grid]` após o bloco `[referência]`."

**Adicionar um novo pensador ao grid de thinkers:**
> "Adicione ao array `thinkers` em `data.js` um objeto `{ id, color, label, title, body }` para [nome do pensador]."

**Mudar a ordem dos blocos:**
> "Mova o bloco `[nome]` no `index.html` para antes/depois do bloco `[referência]`. Não altere `data.js`."

**Adicionar um novo gráfico:**
> "Adicione um novo canvas `id='[nome]-chart'` dentro do bloco `viz-section` e implemente o `Chart.js` correspondente no método `_buildChart()` do `createApp`. Os dados devem vir de `DOC.[campo]`."

**Nova versão do documento:**
> "Incremente `meta.version` para `[vX.Y]` em `data.js` e adicione `[novo campo]` ao objeto `[bloco]`."

---

## Convenções de versionamento

- `v1.x` — estrutura dos blocos estável, adições de conteúdo
- `v2.x` — reorganização de blocos ou novos layouts
- `v3.x` — mudança de stack ou arquitetura

Altere `DOC.meta.version` e `DOC.meta.date` em `data.js` a cada iteração significativa.

---

## Estado atual (referência)

**Blocos existentes (em ordem):**
0. Síntese integradora
1. Equívoco fundador + Ficção da escassez (2 col)
2. Economia invisível (dark, full)
3. Lógica do retorno / colapso (full)
4. Pensadores: Fisher, Fukuyama, Rizomas (3 col)
5. Complexidade + Design connections (2 col)
6. Design Prospectivo (full)
7. Estratégias metodológicas (3 col)
8. Mapa de conexões (full)
9. Tensão produtiva (full)
10. Viz section (toggle, oculto por padrão)

**Dependências CDN:**
- `https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js`
- `https://cdn.jsdelivr.net/npm/petite-vue@0.4.1/dist/petite-vue.iife.js`
