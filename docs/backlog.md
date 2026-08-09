# Backlog do Grupo 16 — divisão de tarefas e acompanhamento

**Prazo final:** 14/08/2026, 23:59 — **7 dias**
**Regra:** cada pessoa faz commits **na própria conta** das tarefas atribuídas a ela. A avaliação
é individual e o professor olha o histórico. Ver [CONTRIBUTING.md](../CONTRIBUTING.md).

> ⚠️ **O enunciado foi atualizado em 07/08.** O trabalho passou de 2 para **7 etapas**. As Etapas
> 1 e 2 não mudaram nada — nenhum retrabalho. As Etapas 3 a 7 são novas e foram deliberadamente
> reduzidas pelo professor para caber no prazo. Ele também autorizou a **forma descritiva**
> (pseudocódigo e descrição) no lugar de implementação real, o que reduz muito o esforço das
> Etapas 4 e 7.

## Como atualizar este arquivo

Ao começar ou terminar uma tarefa, mude o status na tabela e faça o commit junto com o seu
trabalho. Não é preciso commit separado só para isso.

| Status | Significado |
|---|---|
| 🔴 | Não iniciado |
| 🟡 | Em andamento |
| 🔵 | Pronto, aguardando revisão (PR aberto) |
| 🟢 | Concluído e mesclado na `main` |
| ⚫ | Bloqueado (depende de outra tarefa) |

---

## Cronograma

| Data | Marco |
|---|---|
| 07/08 (sex) | ✅ Repositório reestruturado para as 7 etapas; ✅ PR do Deivid mesclado; **Murillo já pode instalar ZAP + Juice Shop** |
| 08/08 (sáb) | ✅ STRIDE completo, 31 ameaças (Gabriel, Luis); ✅ ajustes dos diagramas (Deivid); critérios de risco (Fernando) |
| 09/08 (dom) | Casos de abuso e rastreabilidade (Murillo) → **Etapa 1 fechada** |
| 10/08 (seg) | Registro de riscos, justificativas, priorização e NIST |
| 11/08 (ter) | Plano de tratamento, ordem, residual e conclusões → **Etapa 2 fechada** |
| 12/08 (qua) | **Etapa 3** completa; **Etapa 5** entregue (roda em paralelo desde já) |
| 13/08 (qui) | **Etapa 4** e **Etapa 6**; roteiro do vídeo pronto |
| 14/08 (sex) | **Etapa 7**; gravação do vídeo; revisão final e entrega |

> **Caminho crítico: a Etapa 1.** Enquanto o STRIDE não sair, as Etapas 2, 3 e 4 ficam
> bloqueadas — cinco pessoas dependem do Gabriel e do Luis entregarem até sábado. A única frente
> que roda em paralelo sem depender de ninguém é a Etapa 5, do Murillo.

---

## Resumo por pessoa

| Integrante | Etapa 1 | Etapa 2 | Etapas 3–7 |
|---|---|---|---|
| **Felipe** | Seções 1 e 2 ✅, revisão geral | Seção 16 (risco residual) e seção 17 (conclusões) | **E6** roteiro de detecção, **E7** pipeline e roteiro do vídeo, entrega |
| **Deivid** | Seções 3 e 4 + diagramas ✅ | Seção 12 (estratégias), 14 (plano) e 15 (ordem) | **E3** diagrama da arquitetura segura |
| **Gabriel** | STRIDE S/T/R | riscos, justificativas e controles de S/T/R | **E3** vulnerabilidades CWE/OWASP, **E4** prática 1 |
| **Luis Fillipe** | STRIDE I/D/E, seções 5.7 e 7 | riscos, justificativas e controles de I/D/E | **E4** prática 2 |
| **Murillo** | casos de abuso, seção 6.1 | — | **E5** verificação com ZAP, completa |
| **Fernando** | — | Seção 8 (critérios), 9 (consolidação), 11 (priorização) e 13 (NIST) | **E3** requisitos e decisões |

A divisão é em **fatias verticais**: quem escreve uma ameaça na Etapa 1 é quem a converte em
risco na Etapa 2 e propõe o requisito ou a prática de código nas Etapas 3 e 4. Isso garante a
coerência entre etapas — critério de avaliação explícito — e que todo mundo tenha commits ao
longo de todo o trabalho.

---

## Quadro de tarefas

### Preparação

| # | Tarefa | Responsável | Prazo | Status |
|---|---|---|---|---|
| P1 | Criar estrutura do repositório e documentos-base | Felipe | 05/08 | 🟢 |
| P2 | Reestruturar o repositório para as 7 etapas do enunciado atualizado | Felipe | 07/08 | 🟢 |
| P3 | Cada integrante confere `git config user.email` e faz um commit de teste | Todos | 08/08 | 🟡 faltam Murillo e Fernando |
| P4 | Ler o enunciado atualizado e este backlog | Todos | 08/08 | 🔴 |

---

### Etapa 1 — Casos de abuso e modelagem STRIDE

Arquivo: [`docs/etapa-1-ameacas-stride.md`](etapa-1-ameacas-stride.md)

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E1.1 | Identificação do sistema e justificativa da escolha | 1 | Felipe | 05/08 | 🟢 |
| E1.2 | Descrição do sistema | 2 | Felipe | 05/08 | 🟢 |
| E1.3 | Matriz de perfis e permissões | 3.1 | Deivid | 07/08 | 🟢 |
| E1.4 | Tabela de ativos com criticidade (A01–A14) | 3.2 | Deivid | 07/08 | 🟢 |
| E1.5 | Pontos de interação (P01–P09) | 3.3 | Deivid | 07/08 | 🟢 |
| E1.6 | Diagrama de contexto | 4.1 | Deivid | 07/08 | 🟢 |
| E1.7 | Diagrama de fluxo de dados | 4.2 | Deivid | 07/08 | 🟢 |
| E1.8 | **Ajustes do PR #1:** corrigir fronteira F5, padronizar as zonas de confiança, reexportar as imagens maiores e atualizar os textos de 4.2 e 4.3 | 4 | Deivid | 09/08 | 🟢 |
| E1.9 | STRIDE — Spoofing (4 a 6 ameaças) | 5.1 | Gabriel | 08/08 | 🟢 T01–T06 |
| E1.10 | STRIDE — Tampering (4 a 6 ameaças) | 5.2 | Gabriel | 08/08 | 🟢 T07–T12 |
| E1.11 | STRIDE — Repudiation (3 a 5 ameaças) | 5.3 | Gabriel | 08/08 | 🟢 T13–T16 |
| E1.12 | STRIDE — Information Disclosure (4 a 6 ameaças) | 5.4 | Luis Fillipe | 08/08 | 🟢 T18–T23 |
| E1.13 | STRIDE — Denial of Service (3 a 5 ameaças) | 5.5 | Luis Fillipe | 08/08 | 🟢 T24–T28 |
| E1.14 | STRIDE — Elevation of Privilege (3 a 5 ameaças) | 5.6 | Luis Fillipe | 08/08 | 🟢 T29–T32 |
| E1.15 | Tabela de consolidação das ameaças: preencher a contagem por categoria e o total (os intervalos de ID já estão corretos) | 5.7 | Luis Fillipe | 09/08 | 🔴 |
| E1.16 | Casos de abuso CA02 a CA08 | 6 | Murillo | 09/08 | 🟢 |
| E1.17 | Rastreabilidade caso de abuso ↔ ameaça | 6.1 | Murillo | 09/08 | 🟢 |
| E1.18 | Diagrama de casos de abuso (opcional) | 6 | Murillo | 09/08 | 🟢 |
| E1.19 | Considerações finais da Etapa 1 | 7 | Luis Fillipe | 09/08 | ⚫ depende de E1.15 e E1.17 |
| E1.20 | Revisão de coerência da Etapa 1 | — | Felipe | 09/08 | ⚫ |

---

### Etapa 2 — Análise, priorização e tratamento de riscos

Arquivo: [`docs/etapa-2-riscos-nist.md`](etapa-2-riscos-nist.md)

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E2.1 | Justificar os critérios de probabilidade | 8.1 | Fernando | 08/08 | 🔴 |
| E2.2 | Justificar os critérios de impacto | 8.2 | Fernando | 08/08 | 🔴 |
| E2.3 | Converter as ameaças T01–T16 (S/T/R) em riscos | 9 | Gabriel | 10/08 | 🔴 desbloqueado |
| E2.4 | Converter as ameaças T18–T32 (I/D/E) em riscos | 9 | Luis Fillipe | 10/08 | 🔴 desbloqueado |
| E2.5 | Consolidar a tabela mestre, conferir cálculos e numeração | 9 | Fernando | 10/08 | ⚫ depende de E2.3 e E2.4 |
| E2.6 | Justificativas dos riscos de S/T/R (5 pontos cada) | 10 | Gabriel | 10/08 | ⚫ |
| E2.7 | Justificativas dos riscos de I/D/E (5 pontos cada) | 10 | Luis Fillipe | 10/08 | ⚫ |
| E2.8 | Priorização com justificativa da ordem | 11 | Fernando | 10/08 | ⚫ depende de E2.5 |
| E2.9 | Estratégia de tratamento por risco | 12 | Deivid | 11/08 | ⚫ |
| E2.10 | Tabela de riscos aceitos | 12.1 | Deivid | 11/08 | ⚫ |
| E2.11 | As 6 funções do NIST CSF aplicadas ao SaborExpress | 13.1 | Fernando | 10/08 | 🔴 |
| E2.12 | Mapear cada risco para as funções do NIST | 13.2 | Fernando | 10/08 | ⚫ depende de E2.5 |
| E2.13 | Controles concretos para os riscos de S/T/R | 14 | Gabriel | 11/08 | ⚫ |
| E2.14 | Controles concretos para os riscos de I/D/E | 14 | Luis Fillipe | 11/08 | ⚫ |
| E2.15 | Consolidar o plano: responsáveis e evidências | 14 | Deivid | 11/08 | ⚫ |
| E2.16 | Ordem inicial de implementação | 15 | Deivid | 11/08 | ⚫ |
| E2.17 | Estimativa de risco residual | 16 | Felipe | 11/08 | ⚫ |
| E2.18 | Considerações finais da Etapa 2 | 17 | Felipe | 11/08 | ⚫ |

---

### Etapa 3 — Arquitetura segura

Arquivo: [`docs/etapa-3-arquitetura-segura.md`](etapa-3-arquitetura-segura.md)

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E3.1 | Três requisitos de segurança derivados dos riscos prioritários | 1 | Fernando | 12/08 | ⚫ depende de E2.8 |
| E3.2 | Três mapeamentos de vulnerabilidade (CWE / OWASP) | 2 | Gabriel | 12/08 | ⚫ depende de E3.1 |
| E3.3 | Diagrama da arquitetura segura (`.drawio` + `.png` em `diagramas/etapa-3/`) | 3 | Deivid | 12/08 | ⚫ depende de E2.15 |
| E3.4 | Três decisões de arquitetura justificadas | 4 | Fernando | 12/08 | ⚫ depende de E3.1 |

---

### Etapa 4 — Código seguro e testes

Arquivo: [`docs/etapa-4-codigo-seguro.md`](etapa-4-codigo-seguro.md)

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E4.1 | Prática 1: revisar o modelo, confirmar IDs e completar | Prática 1 | Gabriel | 13/08 | ⚫ depende de E3.1 |
| E4.2 | Prática 2: escolher a prática, escrever os 2 testes **antes** e o pseudocódigo | Prática 2 | Luis Fillipe | 13/08 | ⚫ depende de E3.1 |

> Lembrete: os testes vêm antes da implementação no documento. O enunciado avalia isso
> explicitamente.

---

### Etapa 5 — Verificação de vulnerabilidades

Arquivos: [`docs/etapa-5-verificacao-vulnerabilidades.md`](etapa-5-verificacao-vulnerabilidades.md)
e `evidencias/etapa-5/`

**Esta etapa não depende de nenhuma outra — pode começar hoje.**

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E5.1 | Instalar o ZAP e subir o OWASP Juice Shop localmente | 1 | Murillo | 09/08 | 🟢 |
| E5.2 | Executar a sessão de verificação e salvar capturas em `evidencias/etapa-5/` | 2 e 3 | Murillo | 10/08 | ⚫ depende de E5.1 |
| E5.3 | Analisar três achados, com correção proposta para cada | 4 | Murillo | 12/08 | ⚫ depende de E5.2 |
| E5.4 | Registrar alertas descartados e falsos positivos | 4.2 | Murillo | 12/08 | ⚫ |
| E5.5 | Relacionar os achados com os riscos do SaborExpress e registrar limitações | 5 e 6 | Murillo | 12/08 | ⚫ depende de E2.8 |

---

### Etapa 6 — Detecção de intrusões

Arquivo: [`roteiros/etapa-6-deteccao-de-intrusoes.md`](../roteiros/etapa-6-deteccao-de-intrusoes.md)

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E6.1 | Conceito de detecção e diferença entre prevenir e detectar | 1 e 2 | Felipe | 13/08 | 🟢 |
| E6.2 | Eventos a registrar e o que não deve ir para o log | 3 e 3.1 | Felipe | 13/08 | 🟢 |
| E6.3 | Três regras de detecção | 4 | Felipe | 13/08 | ⚫ depende de E2.8 |
| E6.4 | Fluxo de resposta ao alerta e limitações | 5 e 6 | Felipe | 13/08 | ⚫ |

---

### Etapa 7 — DevSecOps e vídeo final

Arquivo: [`roteiros/etapa-7-devsecops-e-video-final.md`](../roteiros/etapa-7-devsecops-e-video-final.md)

| # | Tarefa | Seção | Responsável | Prazo | Status |
|---|---|---|---|---|---|
| E7.1 | Explicação de DevSecOps e revisão da tabela do pipeline | A.1 e A.2 | Felipe | 13/08 | 🟢 |
| E7.2 | Justificar as condições que interrompem o pipeline | A.3 | Felipe | 13/08 | 🟢 |
| E7.3 | Fechar a divisão de falas do vídeo com o grupo | B.1 | Todos | 12/08 | 🔴 |
| E7.4 | Escrever o roteiro detalhado do vídeo | B.2 | Felipe | 14/08 | ⚫ depende das Etapas 3–6 |
| E7.5 | Gravar o vídeo (5 a 8 min, todos participam) | B | Todos | 14/08 | ⚫ depende de E7.4 |
| E7.6 | Publicar o vídeo e registrar o link | B.4 | Felipe | 14/08 | ⚫ depende de E7.5 |

---

### Fechamento

| # | Tarefa | Responsável | Prazo | Status |
|---|---|---|---|---|
| F1 | Revisão de coerência entre as 7 etapas (todo `T##`, `CA##`, `R##`, `RS##` existe e bate) | Felipe | 14/08 | 🔴 |
| F2 | Conferir que todo diagrama tem imagem **e** arquivo-fonte versionados | Deivid | 14/08 | 🔴 |
| F3 | Remover todos os `<!-- TODO -->` remanescentes | Todos | 14/08 | 🔴 |
| F4 | Conferir que cada integrante tem commits próprios em mais de uma etapa | Todos | 14/08 | 🔴 |
| F5 | Percorrer o checklist final do enunciado | Felipe | 14/08 | 🔴 |
| F6 | Entregar o endereço do repositório na plataforma da disciplina | Felipe | 14/08 | 🔴 |

---

## Perguntas em aberto

- [ ] Confirmar o nome do sistema — "SaborExpress" é proposta e pode ser trocado.
- [ ] Etapa 4: alguém quer escrever código executável de verdade, ou fica tudo em pseudocódigo?
      (as duas formas são aceitas; o pseudocódigo é bem mais rápido)
- [ ] Vídeo: gravação em chamada única com tela compartilhada, ou cada um grava seu bloco e
      alguém edita?
