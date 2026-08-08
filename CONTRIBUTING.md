# Como contribuir — Grupo 16

Leia este arquivo antes do seu primeiro commit. **A avaliação do trabalho é individual e
baseada nos commits de cada integrante**, então seguir estas regras não é burocracia: é o que
garante a sua nota.

---

## 1. Configuração inicial (fazer uma única vez)

```bash
git clone https://github.com/<ORG-OU-USUARIO>/<REPO>.git
cd <REPO>
```

Confira se seu Git está identificado com a **mesma conta do GitHub** em que você foi adicionado
ao repositório:

```bash
git config user.name "Seu Nome Completo"
git config user.email "email-da-sua-conta-github@exemplo.com"
```

Para verificar que deu certo, faça um commit qualquer e veja no GitHub se ele aparece com sua
foto e seu nome de usuário. Se aparecer só o e-mail sem link para o perfil, o e-mail está errado
— corrija antes de continuar. Se você usa o e-mail privado do GitHub, ele tem o formato
`12345678+usuario@users.noreply.github.com` e pode ser copiado em *Settings → Emails*.

---

## 2. Fluxo de trabalho

Trabalhamos com **uma branch por tarefa** para evitar conflitos entre os seis integrantes.

```bash
git checkout main
git pull                                  # sempre antes de começar algo novo
git checkout -b etapa1/stride-spoofing    # crie sua branch
# ... edite os arquivos ...
git add docs/etapa-1-ameacas-stride.md
git commit -m "Adiciona ameacas de falsificacao de identidade"
git push -u origin etapa1/stride-spoofing
```

Depois abra um **Pull Request** no GitHub, peça revisão de outro integrante e faça o merge.

### Padrão de nomes de branch

| Padrão | Exemplo |
|---|---|
| `etapa1/<assunto>` | `etapa1/casos-de-abuso` |
| `etapa2/<assunto>` | `etapa2/registro-de-riscos` |
| `etapa3/<assunto>` | `etapa3/requisitos-de-seguranca` |
| `etapa4/<assunto>` | `etapa4/controle-de-autorizacao` |
| `etapa5/<assunto>` | `etapa5/verificacao-zap` |
| `etapa6/<assunto>` | `etapa6/regras-de-deteccao` |
| `etapa7/<assunto>` | `etapa7/pipeline-devsecops` |
| `docs/<assunto>` | `docs/corrige-tabela-de-ativos` |
| `diagramas/<assunto>` | `diagramas/arquitetura-segura` |

### Regra de ouro para evitar conflitos

**Só edite a seção do documento que está atribuída a você** em
[docs/backlog.md](docs/backlog.md). Se precisar mexer na seção de outra pessoa, avise no grupo
antes. Cada seção tem um comentário HTML marcando o responsável, por exemplo:

```markdown
<!-- RESPONSÁVEL: Gabriel — não editar sem avisar -->
```

---

## 3. Mensagens de commit

O enunciado avalia explicitamente a qualidade das mensagens. Escreva no **imperativo**,
descrevendo **o que foi feito no conteúdo do trabalho** — não a mecânica do arquivo.

### ✅ Boas mensagens

```
Adiciona ameacas de falsificacao de identidade
Descreve ativos e dados sensiveis do sistema
Inclui caso de abuso de alteracao de pagamento
Atualiza diagrama de fluxo de dados
Justifica probabilidade e impacto dos riscos R04 a R07
Mapeia riscos criticos para as funcoes Protect e Detect do NIST
Corrige incoerencia entre a ameaca T09 e o caso de abuso CA05
```

### ❌ Mensagens que não serão aceitas como participação

```
alterações
ajustes
trabalho
update
commit
.
```

### O que NÃO fazer

- **Não** divida uma alteração pequena em muitos commits para inflar o histórico. O enunciado diz
  explicitamente que isso não conta como participação efetiva.
- **Não** faça commits vazios ou que só mexem em espaços em branco.
- **Não** peça para outra pessoa commitar o seu texto. O commit precisa ser seu.
- **Não** deixe tudo para o último dia — o histórico de evolução do trabalho é critério de
  avaliação.

---

## 4. Convenções de escrita dos documentos

- **Português**, com acentuação correta, nos documentos. Nas *mensagens de commit* a acentuação
  é opcional (evita problemas de codificação em alguns terminais).
- **Identificadores estáveis.** Ameaças são `T01`, `T02`, …; casos de abuso `CA01`, `CA02`, …;
  riscos `R01`, `R02`, …; requisitos de segurança `RS01`, …; testes `TS01`, …; achados do ZAP
  `A01`, …. Uma vez publicado, **não renumere** — outras etapas apontam para esses IDs.
  Lacunas na numeração são aceitáveis e preferíveis a renumerar: se uma categoria não usou todos
  os números reservados para ela, deixe o buraco e registre que ele é intencional.
- **Rastreabilidade.** Cada etapa se apoia na anterior: todo caso de abuso cita as ameaças STRIDE
  relacionadas, todo risco cita a ameaça `T##` de origem, todo requisito cita o risco `R##`, e
  toda prática de código cita o requisito `RS##`. Essa cadeia é critério de avaliação em todas as
  etapas.
- **Sem links externos como única fonte.** Diagramas feitos no draw.io, Excalidraw, Figma etc.
  devem ter **imagem e arquivo-fonte lado a lado** na pasta da etapa (ex.:
  `diagramas/etapa-1/`). Ver [diagramas/README.md](diagramas/README.md).
- **Tabelas Markdown** para as listagens de ameaças, riscos e controles — o enunciado sugere esse
  formato e ele é o mais legível na avaliação.

---

## 5. Revisão

Antes de aprovar o PR de um colega, verifique:

- [ ] O texto responde ao que o enunciado pede naquela seção?
- [ ] As ameaças são **concretas e específicas do SaborExpress**, e não definições genéricas do
      STRIDE?
- [ ] Os IDs citados existem e apontam para o item certo?
- [ ] Não há contradição com o que já está escrito em outras seções?
- [ ] As tabelas renderizam corretamente no GitHub?

---

## 6. Comandos úteis

```bash
git log --oneline --graph --all          # ver o histórico
git log --author="Seu Nome" --oneline    # conferir seus próprios commits
git pull --rebase origin main            # atualizar sua branch com a main
git diff main                            # ver o que você mudou em relação à main
```
