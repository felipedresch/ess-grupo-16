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

| Pasta | Arquivo | Conteúdo | Onde é citado |
|---|---|---|---|
| `etapa-1/` | `diagrama-contexto` | Perfis de usuário, apps, API, banco e serviços externos | Etapa 1, seção 4.1 |
| `etapa-1/` | `diagrama-fluxo-dados` | Caminho do pedido, com as fronteiras de confiança F1 a F5 | Etapa 1, seção 4.2 |
| `etapa-1/` | `casos-de-abuso` | Casos de abuso e sua relação com os atores | Etapa 1, seção 6.2 (feito em Mermaid, dentro do documento) |
| `etapa-3/` | `arquitetura-segura` | Arquitetura com autenticação, autorização, logs e posição dos controles | Etapa 3, seção 3 |
| `etapa-7/` | `pipeline-devsecops` | Os oito momentos do pipeline, condições de continuidade e de bloqueio | Etapa 7, seção A.4 |

> Status e responsável de cada diagrama ficam em [docs/backlog.md](../docs/backlog.md), que é a
> única fonte de verdade sobre andamento das tarefas.

## Como fazer

**Opção 1 — draw.io (usada na Etapa 1).** Use [app.diagrams.net](https://app.diagrams.net), salve
o `.drawio` na pasta da etapa e exporte em *File → Export as → PNG* com zoom 200%, na mesma pasta.
Commite **os dois arquivos**.

**Opção 2 — Mermaid (usada na Etapa 7).** Escreva o diagrama em um arquivo `.mmd`, que é o
arquivo-fonte, e gere a imagem com o mermaid-cli:

```bash
npx -y @mermaid-js/mermaid-cli -i diagrama.mmd -o diagrama.png -w 1600 -b white
```

O mesmo código pode ir dentro de um bloco ` ```mermaid ` no documento, que o GitHub renderiza
nativamente. Vale colocar o bloco dentro de um `<details>` para não duplicar a imagem na tela.

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
