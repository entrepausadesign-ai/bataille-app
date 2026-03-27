# PRD — Bataille One-Pager Web App
## Versão 1.0 · Design Prospectivo UTFPR

---

## Problema

O processo de preparação do anteprojeto de mestrado gera múltiplas versões de um mesmo documento conceitual. Sem separação entre conteúdo e apresentação, cada iteração exige reescrever HTML misturado com texto — lento, frágil, difícil de delegar a um assistente de IA.

---

## Solução

Web app local de **uma única página** que renderiza um one-pager acadêmico a partir de um arquivo de dados estruturado. Conteúdo e layout são editáveis independentemente.

---

## Usuário

Pesquisador/designer trabalhando sozinho, em laptop, sem pipeline de deploy. Usa o assistente de IA como co-editor. Precisa de exportação PDF ocasional para compartilhar com orientadores.

---

## Arquivos

| Arquivo         | Responsabilidade                        | Quem edita          |
|-----------------|-----------------------------------------|---------------------|
| `data.js`       | Todo o conteúdo como objetos JS         | IA + pesquisador    |
| `style.css`     | Todos os estilos visuais                | IA (raramente)      |
| `index.html`    | Templates declarativos (petite-vue)     | IA (adição blocos)  |
| `prompt-guide.md` | Guia de transposição para novo chat   | referência          |
| `PRD.md`        | Este documento                          | referência          |

---

## Features — v1 (implementadas)

- [x] Renderização de todos os blocos a partir de `data.js`
- [x] Layout responsivo multi-coluna (1, 2, 3 colunas)
- [x] Toolbar sticky com exportação via `window.print()`
- [x] Badge de versão/data
- [x] Visualização de dados (Chart.js, toggle)
- [x] Síntese integradora como bloco 0
- [x] Petite-vue: templates no HTML, sem JS string rendering
- [x] Separação total conteúdo / layout / estilo

---

## Features — backlog (próximas versões)

### v1.x — conteúdo
- [ ] Bloco de **referências bibliográficas** estruturadas (array `references[]`)
- [ ] Bloco de **glossário** de conceitos-chave
- [ ] Bloco de **perguntas abertas** / lacunas do anteprojeto
- [ ] Campo `meta.orientadores` visível no header

### v2.x — interatividade
- [ ] **Modo foco** por bloco (clique → expande em overlay)
- [ ] **Filtro por pensador** (clique na tag → destaca conexões)
- [ ] **Toggle de seções** (ocultar/mostrar blocos individuais)
- [ ] Segundo gráfico: **linha do tempo de leituras formativas**

### v2.x — exportação
- [ ] Exportação via `html2canvas + jsPDF` (layout preservado, sem diálogo do browser)
- [ ] Modo `?print=true` na URL que remove toolbar e aplica `@page` CSS

### v3.x — arquitetura
- [ ] `data/` como pasta com múltiplos arquivos por versão (`v1.js`, `v2.js`)
- [ ] Seletor de versão na toolbar (dropdown)
- [ ] `diff` visual entre versões (destaque do que mudou)

---

## Regras de design

1. **Só `data.js` muda para editar conteúdo.** Se precisar tocar no `index.html` para mudar texto, algo está errado.
2. **Novos blocos = novo objeto em `data.js` + novo template em `index.html`.** `style.css` não muda.
3. **Sem build, sem npm, sem node_modules.** Servidor HTTP simples (`python3 -m http.server`) é suficiente.
4. **CDN apenas para Chart.js e petite-vue.** Sem outras dependências.
5. **Tipografia:** EB Garamond (corpo) + DM Mono (labels, código, badges). Nunca trocar.
6. **Paleta:** 5 variantes de card (accent/gold/teal/purple/dark) + 4 tag colors (red/gold/teal/purple). Sem criar novas cores.

---

## Como criar uma nova versão do documento

```
1. Abrir data.js
2. Alterar meta.version e meta.date
3. Editar os campos dos blocos existentes, OU
4. Adicionar novo objeto ao DOC (ver prompt-guide.md)
5. Se novo bloco: adicionar template em index.html
6. Salvar e recarregar o browser
```

---

## Como usar o assistente de IA para iterar

Ver `prompt-guide.md` para sintaxe dos pedidos.

**Princípio:** sempre fornecer os três arquivos ao assistente no início da sessão. Pedir mudanças por bloco, não por linha. Pedir sempre a versão completa do arquivo modificado para substituição direta.

---

## Referência de cores (CSS vars)

```css
--accent: #b5390e   /* vermelho — problema, tensão   */
--gold:   #9a7020   /* dourado  — síntese, horizonte  */
--teal:   #1a6e5a   /* teal     — método, conexão     */
--purple: #4a3880   /* roxo     — teoria, pensadores  */
--ink:    #1a1814   /* dark     — destaque máximo     */
```
