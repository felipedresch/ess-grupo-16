# Diagramas

O enunciado exige que os **arquivos-fonte** dos diagramas estejam versionados aqui, e não apenas
links para ferramentas externas.

## Convenção

| O quê | Onde | Formato |
|---|---|---|
| Arquivo editável | `diagramas/fonte/` | `.drawio` (recomendado), `.excalidraw` ou `.mmd` |
| Imagem exportada | `imagens/` | `.png`, largura mínima 1200px |

Os nomes do fonte e da imagem devem coincidir:

```
diagramas/fonte/diagrama-contexto.drawio
imagens/diagrama-contexto.png
```

## Diagramas previstos

| Arquivo | Conteúdo | Responsável |
|---|---|---|
| `diagrama-contexto` | Perfis de usuário, apps, API, banco de dados e serviços externos | Deivid |
| `fluxo-de-dados` | Caminho completo do pedido, com as fronteiras de confiança F1–F5 | Deivid |
| `casos-de-abuso` | Casos de abuso e sua relação com os atores (opcional) | Murillo |

## Como fazer

**Opção 1 — draw.io (recomendada).** Use [app.diagrams.net](https://app.diagrams.net),
salve o `.drawio` em `diagramas/fonte/` e exporte em *File → Export as → PNG* com zoom 200%
para `imagens/`. Commite **os dois arquivos**.

**Opção 2 — Mermaid.** Escreva o diagrama direto no Markdown, em bloco ` ```mermaid `. O GitHub
renderiza nativamente e o próprio Markdown já é o arquivo-fonte. Mais simples de versionar, mas
com menos controle sobre o layout.

## Legibilidade

Os diagramas são critério de avaliação. Antes de commitar, confira:

- [ ] O texto é legível sem ampliar a imagem no GitHub.
- [ ] Há legenda explicando as formas e as cores usadas.
- [ ] As fronteiras de confiança estão visíveis (linhas tracejadas) no DFD.
- [ ] O diagrama é citado e explicado no texto do documento — imagem solta não conta.
