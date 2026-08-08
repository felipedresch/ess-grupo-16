# Diagramas

O enunciado exige que os **arquivos-fonte** dos diagramas estejam versionados aqui, e não apenas
links para ferramentas externas.

## Convenção

Cada etapa tem sua pasta, e o **fonte fica ao lado da imagem**, com o mesmo nome:

```
diagramas/etapa-1/diagrama-contexto.drawio
diagramas/etapa-1/diagrama-contexto.png
```

Nos documentos, referencie a imagem com caminho relativo a partir de `docs/`:

```markdown
![Diagrama de contexto](../diagramas/etapa-1/diagrama-contexto.png)
```

## Diagramas do trabalho

| Pasta | Arquivo | Conteúdo | Responsável | Status |
|---|---|---|---|---|
| `etapa-1/` | `diagrama-contexto` | Perfis de usuário, apps, API, banco e serviços externos | Deivid | ✅ pronto |
| `etapa-1/` | `diagrama-fluxo-dados` | Caminho do pedido, com as fronteiras de confiança F1–F5 | Deivid | 🟡 ajustes pendentes |
| `etapa-1/` | `casos-de-abuso` | Casos de abuso e sua relação com os atores (opcional) | Murillo | 🔴 |
| `etapa-3/` | `arquitetura-segura` | Arquitetura com autenticação, autorização, logs e posição dos controles | Deivid | 🔴 |

## Como fazer

**Opção 1 — draw.io (usada na Etapa 1).** Use [app.diagrams.net](https://app.diagrams.net), salve
o `.drawio` na pasta da etapa e exporte em *File → Export as → PNG* com zoom 200%, na mesma pasta.
Commite **os dois arquivos**.

**Opção 2 — Mermaid.** Escreva o diagrama direto no Markdown, em bloco ` ```mermaid `. O GitHub
renderiza nativamente e o próprio Markdown já é o fonte, o que dispensa arquivo separado.

## Legibilidade

Os diagramas são critério de avaliação. Antes de commitar, confira:

- [ ] Largura mínima de **1200px** na imagem exportada.
- [ ] **Proporção equilibrada.** Um diagrama muito largo e baixo (ex.: 2000×500) é reduzido pelo
      GitHub à largura da coluna e o texto fica ilegível. Prefira algo próximo de 4:3.
- [ ] O texto é legível sem ampliar a imagem no GitHub.
- [ ] Há legenda explicando as formas e as cores.
- [ ] O diagrama é citado e explicado no texto do documento — imagem solta não conta.

### Fronteiras de confiança

No diagrama de fluxo de dados, use sempre a mesma convenção: **cada caixa tracejada é uma zona de
confiança**, contendo todos os componentes que compartilham o mesmo nível de confiança. Os
identificadores `F##` ficam sobre as **setas que cruzam** de uma zona para outra — é a travessia
que é a fronteira, não a caixa.

Zonas do SaborExpress:

| Zona | Contém |
|---|---|
| Dispositivo do usuário | Cliente, app do entregador, painel do restaurante |
| Plataforma SaborExpress | API backend, banco de dados, storage |
| Serviços externos | Gateway de pagamento, mapas, notificações, antifraude |
| Backoffice | Administrador e suporte |
