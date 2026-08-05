# Backlog do Grupo 16 — divisão de tarefas e acompanhamento

**Prazo final:** 14/08/2026, 23:59
**Regra:** cada pessoa faz commits **na própria conta** das tarefas atribuídas a ela. A avaliação
é individual e o professor olha o histórico. Ver [CONTRIBUTING.md](../CONTRIBUTING.md).

## Como atualizar este arquivo

Ao começar ou terminar uma tarefa, mude o status na tabela e faça o commit junto com o seu
trabalho. Não é preciso commit separado só para isso.


| Status | Significado                            |
| ------ | -------------------------------------- |
| 🔴     | Não iniciado                           |
| 🟡     | Em andamento                           |
| 🔵     | Pronto, aguardando revisão (PR aberto) |
| 🟢     | Concluído e mesclado na `main`         |
| ⚫      | Bloqueado (depende de outra tarefa)    |


---



## Cronograma


| Data        | Marco                                                                    |
| ----------- | ------------------------------------------------------------------------ |
| 05/08 (qua) | Repositório estruturado, README e documentos-base publicados             |
| 08/08 (sáb) | Ativos, diagramas e tabelas STRIDE completos                             |
| 10/08 (seg) | Casos de abuso prontos → **Etapa 1 fechada**                             |
| 12/08 (qua) | Registro de riscos, justificativas, priorização e mapeamento NIST        |
| 13/08 (qui) | Plano de tratamento, ordem de implementação, risco residual e conclusões |
| 14/08 (sex) | Revisão final, vídeo e entrega                                           |


---



## Quadro de tarefas



### Preparação


| #   | Tarefa                                                                   | Responsável | Prazo | Status |
| --- | ------------------------------------------------------------------------ | ----------- | ----- | ------ |
| P1  | Criar estrutura do repositório, README, CONTRIBUTING e documentos-base   | Felipe      | 05/08 | 🟢     |
| P2  | Informar o usuário do GitHub (Fernando ainda não enviou)                 | Fernando    | 06/08 | 🔴     |
| P3  | Cada integrante confere `git config user.email` e faz um commit de teste | Todos       | 06/08 | 🔴     |
| P4  | Ler o enunciado completo e este backlog                                  | Todos       | 06/08 | 🔴     |


---



### Etapa 1 — Casos de abuso e modelagem STRIDE

Arquivo: `[docs/modelagem-de-ameacas.md](modelagem-de-ameacas.md)`


| #     | Tarefa                                                                                 | Seção | Responsável  | Prazo | Status                     |
| ----- | -------------------------------------------------------------------------------------- | ----- | ------------ | ----- | -------------------------- |
| E1.1  | Identificação do sistema e justificativa da escolha                                    | 1     | Felipe       | 05/08 | 🟢                         |
| E1.2  | Descrição do sistema: problema, usuários, funcionalidades, dados e recursos a proteger | 2     | Felipe       | 05/08 | 🟢                         |
| E1.3  | Matriz de perfis e permissões                                                          | 3.1   | Deivid       | 08/08 | 🔴                         |
| E1.4  | Tabela de ativos com criticidade — completar até A14                                   | 3.2   | Deivid       | 08/08 | 🔴                         |
| E1.5  | Pontos de interação / superfície de ataque — completar até P09                         | 3.3   | Deivid       | 08/08 | 🔴                         |
| E1.6  | **Diagrama de contexto** (.drawio + .png)                                              | 4.1   | Deivid       | 09/08 | 🔴                         |
| E1.7  | **Diagrama de fluxo de dados** com fronteiras de confiança (.drawio + .png)            | 4.2   | Deivid       | 09/08 | 🔴                         |
| E1.8  | STRIDE — Spoofing (4 a 6 ameaças)                                                      | 5.1   | Gabriel      | 08/08 | 🔴                         |
| E1.9  | STRIDE — Tampering (4 a 6 ameaças)                                                     | 5.2   | Gabriel      | 08/08 | 🔴                         |
| E1.10 | STRIDE — Repudiation (3 a 5 ameaças)                                                   | 5.3   | Gabriel      | 08/08 | 🔴                         |
| E1.11 | STRIDE — Information Disclosure (4 a 6 ameaças)                                        | 5.4   | Luis Fillipe | 08/08 | 🔴                         |
| E1.12 | STRIDE — Denial of Service (3 a 5 ameaças)                                             | 5.5   | Luis Fillipe | 08/08 | 🔴                         |
| E1.13 | STRIDE — Elevation of Privilege (3 a 5 ameaças)                                        | 5.6   | Luis Fillipe | 08/08 | 🔴                         |
| E1.14 | Tabela de consolidação das ameaças                                                     | 5.7   | Luis Fillipe | 09/08 | ⚫ depende de E1.8–E1.13    |
| E1.15 | Casos de abuso CA02 a CA08 (CA01 já serve de modelo)                                   | 6     | Murillo      | 10/08 | ⚫ depende de E1.8–E1.13    |
| E1.16 | Tabela de rastreabilidade caso de abuso ↔ ameaça                                       | 6.1   | Murillo      | 10/08 | ⚫ depende de E1.15         |
| E1.17 | Diagrama de casos de abuso (opcional, mas soma pontos)                                 | 6     | Murillo      | 10/08 | 🔴                         |
| E1.18 | Considerações finais da Etapa 1 (4 subseções)                                          | 7     | Luis Fillipe | 10/08 | ⚫ depende de E1.14 e E1.16 |
| E1.19 | Revisão de coerência de toda a Etapa 1                                                 | —     | Felipe       | 10/08 | ⚫                          |


---



### Etapa 2 — Análise, priorização e tratamento de riscos

Arquivo: `[docs/analise-de-riscos.md](analise-de-riscos.md)`


| #     | Tarefa                                                                     | Seção | Responsável  | Prazo | Status                   |
| ----- | -------------------------------------------------------------------------- | ----- | ------------ | ----- | ------------------------ |
| E2.1  | Justificar os critérios de probabilidade no contexto do sistema            | 8.1   | Fernando     | 11/08 | 🔴                       |
| E2.2  | Justificar os critérios de impacto e as dimensões consideradas             | 8.2   | Fernando     | 11/08 | 🔴                       |
| E2.3  | Converter as ameaças T01–T17 (S/T/R) em riscos, com prob., impacto e nível | 9     | Gabriel      | 12/08 | ⚫ depende de E1.10       |
| E2.4  | Converter as ameaças T18–T33 (I/D/E) em riscos, com prob., impacto e nível | 9     | Luis Fillipe | 12/08 | ⚫ depende de E1.13       |
| E2.5  | Consolidar a tabela mestre de riscos, conferir cálculos e numeração        | 9     | Fernando     | 12/08 | ⚫ depende de E2.3 e E2.4 |
| E2.6  | Justificativas dos riscos de S/T/R (5 pontos cada)                         | 10    | Gabriel      | 12/08 | ⚫                        |
| E2.7  | Justificativas dos riscos de I/D/E (5 pontos cada)                         | 10    | Luis Fillipe | 12/08 | ⚫                        |
| E2.8  | Priorização com justificativa da ordem (não só pontuação)                  | 11    | Fernando     | 12/08 | ⚫ depende de E2.5        |
| E2.9  | Escolha e justificativa da estratégia de tratamento por risco              | 12    | Deivid       | 13/08 | ⚫                        |
| E2.10 | Tabela de riscos aceitos com motivo, aprovador, condições e revisão        | 12.1  | Deivid       | 13/08 | ⚫                        |
| E2.11 | Descrever as 6 funções do NIST CSF aplicadas ao SaborExpress               | 13.1  | Murillo      | 12/08 | 🔴                       |
| E2.12 | Mapear cada risco para as funções do NIST, com observações                 | 13.2  | Murillo      | 12/08 | ⚫ depende de E2.5        |
| E2.13 | Controles concretos para os riscos de S/T/R                                | 14    | Gabriel      | 13/08 | ⚫                        |
| E2.14 | Controles concretos para os riscos de I/D/E                                | 14    | Luis Fillipe | 13/08 | ⚫                        |
| E2.15 | Consolidar o plano de tratamento: responsáveis e evidências de verificação | 14    | Deivid       | 13/08 | ⚫                        |
| E2.16 | Ordem inicial de implementação, com justificativa                          | 15    | Deivid       | 13/08 | ⚫                        |
| E2.17 | Estimativa de risco residual e condições de aceitação                      | 16    | Felipe       | 13/08 | ⚫                        |
| E2.18 | Considerações finais da Etapa 2 (8 subseções)                              | 17    | Felipe       | 14/08 | ⚫                        |


---



### Fechamento


| #   | Tarefa                                                                                     | Responsável | Prazo | Status |
| --- | ------------------------------------------------------------------------------------------ | ----------- | ----- | ------ |
| F1  | Revisão final de coerência entre as duas etapas (todo `T##`, `CA##` e `R##` existe e bate) | Felipe      | 14/08 | 🔴     |
| F2  | Conferir que todos os diagramas têm `.png` **e** arquivo-fonte versionados                 | Deivid      | 14/08 | 🔴     |
| F3  | Remover todos os `<!-- TODO -->` remanescentes                                             | Todos       | 14/08 | 🔴     |
| F4  | Conferir que cada integrante tem commits próprios e relevantes                             | Todos       | 14/08 | 🔴     |
| F5  | Vídeo final sumarizando o trabalho                                                         | A definir   | 14/08 | 🔴     |
| F6  | Enviar o endereço do repositório na plataforma da disciplina                               | Felipe      | 14/08 | 🔴     |


---



## Resumo por pessoa


| Integrante       | Etapa 1                                                                 | Etapa 2                                                                | Carga                                                         |
| ---------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------- |
| **Felipe**       | Seções 1, 2 e revisão                                                   | Risco residual (16) e considerações finais (17)                        | Estrutura do repositório, revisões de PR, riscos e fechamento |
| **Deivid**       | Seções 3 e 4 + os dois diagramas                                        | Estratégias (12), plano consolidado (14) e ordem de implementação (15) | Ativos, diagramas e plano                                     |
| **Gabriel**      | STRIDE S, T, R (seções 5.1–5.3)                                         | Riscos, justificativas e controles de S/T/R                            | Metade do STRIDE, ponta a ponta                               |
| **Luis Fillipe** | STRIDE I, D, E (5.4–5.6), consolidação (5.7) e considerações finais (7) | Riscos, justificativas e controles de I/D/E                            | Metade do STRIDE, ponta a ponta                               |
| **Murillo**      | Casos de abuso (seção 6) e rastreabilidade                              | Funções do NIST (13.1) e mapeamento (13.2)                             | Casos de abuso e NIST                                         |
| **Fernando**     | — (apoio na revisão)                                                    | Critérios (8), consolidação do registro (9) e priorização (11)         | Métrica e priorização de risco + apoio na revisão             |


> A divisão foi feita em **fatias verticais**: quem escreve uma ameaça na Etapa 1 é quem a
> converte em risco e propõe o controle na Etapa 2. Isso garante coerência entre as etapas
> (critério de avaliação explícito) e que todo mundo tenha commits nas duas.

---



## Perguntas em aberto para o grupo

- [ ] Quem grava o vídeo final? Uma pessoa ou cada um grava a própria parte?
- [ ] Ferramenta de diagrama: draw.io é a sugestão (gera `.drawio` versionável e exporta `.png`).
  ```
  Alternativa: Mermaid direto no Markdown, que dispensa arquivo-fonte separado.
  ```

