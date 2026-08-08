# Etapa 7 — DevSecOps e Vídeo Final

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 6 — Detecção de Intrusões](etapa-6-deteccao-de-intrusoes.md)
**Última atualização:** <!-- atualize a data ao editar --> 07/08/2026

<!-- RESPONSÁVEL: Felipe (pipeline e roteiro); Todos (gravação) -->

> Roteiro descritivo. O enunciado é explícito: **não é necessário implementar um pipeline real**.

---

# Parte A — Pipeline DevSecOps proposto

## A.1 Ideia geral

<!-- TODO(Felipe): 1 ou 2 parágrafos explicando o que é DevSecOps com as próprias palavras.
     Ideia central a desenvolver: segurança deixa de ser uma etapa no fim (uma revisão antes de
     entregar) e passa a acontecer em todo momento do ciclo, com verificação automática e com
     critérios objetivos que barram a entrega quando algo está errado.
     Um bom fecho: este trabalho inteiro é, na prática, um percurso pelo pipeline — a Etapa 1
     foi o planejamento, a 3 o projeto, a 4 a implementação, a 5 a verificação e a 6 a operação. -->

## A.2 O pipeline

<!-- TODO(Felipe): revisar e completar. A tabela abaixo já amarra cada momento do pipeline às
     etapas do trabalho — confira se os nomes dos artefatos batem com o que de fato foi produzido
     e ajuste as condições de continuidade. -->

| Momento | Atividade de segurança | Evidência produzida | Condição para continuar |
|---|---|---|---|
| **Planejamento** | Modelagem de ameaças com STRIDE e análise de riscos | Tabelas de ameaças (`T##`) e de riscos (`R##`) — Etapas 1 e 2 | Riscos prioritários identificados e classificados |
| **Requisitos e projeto** | Derivação de requisitos de segurança e decisões de arquitetura | Requisitos `RS##`, mapeamento CWE/OWASP e diagrama da arquitetura segura — Etapa 3 | Todo risco crítico tem ao menos um requisito associado |
| **Implementação** | Práticas de código seguro, com testes escritos antes | Pseudocódigo e testes `TS##` — Etapa 4 | Revisão de código feita por outra pessoa, sem pendência aberta |
| **Testes automatizados** | Execução dos testes de segurança a cada alteração | Relatório da suíte de testes | Todos os testes de segurança aprovados |
| **Análise de código e dependências** | Análise estática (SAST) e verificação de dependências vulneráveis e de segredos no repositório | Relatório do SAST e do scanner de dependências | Nenhuma vulnerabilidade crítica ou alta em aberto; nenhum segredo detectado |
| **Teste dinâmico** | Varredura com ZAP contra o ambiente de homologação | Relatório de alertas — Etapa 5 | Achados de severidade alta analisados e tratados ou justificados |
| **Implantação** | Publicação com configuração revisada e segredos vindos do cofre | Registro da implantação e da versão publicada | Aprovação registrada e possibilidade de reverter a versão |
| **Monitoramento e resposta** | Coleta de logs, regras de detecção e tratamento de alertas | Alertas, registros e histórico de incidentes — Etapa 6 | Incidentes abertos tratados; aprendizado devolvido ao início do ciclo |

## A.3 Condições que interrompem o pipeline

O enunciado pede **pelo menos três**. Estas são as adotadas pelo grupo:

<!-- TODO(Felipe): revisar, ajustar e justificar cada uma em 1 ou 2 frases — por que essa
     condição em especial merece parar a entrega. -->

1. **Teste de segurança reprovado.** <!-- TODO: justificar -->
2. **Vulnerabilidade crítica não analisada** no relatório de dependências ou do ZAP. <!-- TODO -->
3. **Segredo encontrado no repositório** (chave de API, token, senha). <!-- TODO: amarrar com o ativo A13 -->
4. **Falha no controle de acesso** detectada pelos testes de autorização. <!-- TODO -->

## A.4 Diagrama do pipeline (opcional)

<!-- TODO(Felipe): opcional. Se quiser, um diagrama simples em Mermaid resolve e já fica
     versionado no próprio Markdown, sem precisar de arquivo-fonte separado. Algo como:
     graph LR
       A[Planejamento<br/>STRIDE] --> B[Projeto<br/>requisitos] --> C[Código<br/>testes] -->
       D[SAST + deps] --> E[ZAP] --> F[Implantação] --> G[Monitoramento] --> A -->

---

# Parte B — Roteiro do vídeo final

**Duração:** 5 a 8 minutos. **Todos os integrantes participam.**

## B.1 Divisão por integrante

<!-- TODO(grupo): confirmar quem fala o quê e fechar os tempos. A proposta abaixo dá cerca de
     1 minuto para cada um, o que fecha em ~7 minutos com a abertura e o encerramento. -->

| Bloco | Tempo | Quem fala | Conteúdo |
|---|---|---|---|
| Abertura | 0:00–0:45 | Felipe | O sistema escolhido, por que um app de delivery, e o que o grupo produziu ao longo da disciplina |
| Ameaças e casos de abuso | 0:45–2:00 | Gabriel e Murillo | As ameaças STRIDE mais relevantes e um caso de abuso contado como história, do começo ao fim |
| Riscos prioritários | 2:00–3:00 | Fernando | Como as ameaças viraram riscos, o critério de pontuação e quais ficaram no topo |
| Arquitetura e decisões | 3:00–4:00 | Deivid | O diagrama da arquitetura segura e as decisões tomadas |
| Código seguro e verificação | 4:00–5:15 | Luis Fillipe e Murillo | As práticas de código com os testes escritos antes, e os achados do ZAP |
| Detecção e pipeline | 5:15–6:15 | Felipe | As regras de detecção e o pipeline DevSecOps |
| O que aprendemos | 6:15–7:00 | Todos, uma frase cada | Aprendizado individual e as maiores dificuldades |

## B.2 Roteiro detalhado

<!-- TODO(Felipe): escrever o texto de cada bloco depois que as Etapas 3 a 6 estiverem fechadas.
     Só faz sentido roteirizar quando houver o conteúdo definitivo para apresentar. -->

### Abertura — Felipe
<!-- TODO -->

### Ameaças e casos de abuso — Gabriel e Murillo
<!-- TODO -->

### Riscos prioritários — Fernando
<!-- TODO -->

### Arquitetura e decisões — Deivid
<!-- TODO -->

### Código seguro e verificação — Luis Fillipe e Murillo
<!-- TODO -->

### Detecção e pipeline — Felipe
<!-- TODO -->

### Encerramento — Todos
<!-- TODO -->

## B.3 Orientações de gravação

- **Não leia as tabelas.** O enunciado diz explicitamente que não é necessário mostrar todas as
  tabelas — ele quer as **decisões** e a **evolução** do trabalho. Mostre a tela do repositório e
  fale por cima.
- Cada um grava o seu bloco separadamente e alguém junta, ou o grupo grava uma chamada com
  compartilhamento de tela. A segunda opção é mais rápida e costuma ficar mais natural.
- Confira o áudio antes de gravar tudo — áudio ruim estraga um vídeo bom.
- O vídeo pode ser hospedado fora do repositório (YouTube não listado, por exemplo), mas **este
  roteiro precisa estar versionado aqui**, o que é exigência explícita do enunciado.

## B.4 Onde o vídeo foi publicado

| Item | Valor |
|---|---|
| Link | <!-- TODO --> |
| Duração | <!-- TODO --> |
| Data da gravação | <!-- TODO --> |

---

## Checklist final da disciplina

Conferir antes de considerar a entrega concluída:

- [ ] **Etapa 1** — ameaças STRIDE e casos de abuso
- [ ] **Etapa 2** — análise, priorização e tratamento dos riscos
- [ ] **Etapa 3** — três requisitos, três vulnerabilidades, um diagrama e três decisões
- [ ] **Etapa 4** — duas práticas de código seguro com testes
- [ ] **Etapa 5** — uma verificação com até três achados analisados
- [ ] **Etapa 6** — roteiro com três regras de detecção
- [ ] **Etapa 7** — pipeline, roteiro e vídeo final
- [ ] Commits próprios de **todos** os integrantes, atribuídos às contas corretas do GitHub
- [ ] Arquivos, diagramas e evidências versionados no repositório
- [ ] Nenhum `<!-- TODO -->` restante nos documentos
- [ ] Endereço do repositório entregue na plataforma da disciplina
