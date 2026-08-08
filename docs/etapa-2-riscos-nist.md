# Etapa 2 — Análise, Priorização e Tratamento de Riscos com o NIST CSF 2.0

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 1 — Casos de Abuso e Modelagem de Ameaças](etapa-1-ameacas-stride.md)
**Última atualização:** <!-- atualize a data ao editar --> 05/08/2026

> Esta etapa **não substitui** a Etapa 1: ela transforma as ameaças `T##` e os casos de abuso
> `CA##` já identificados em riscos avaliáveis, priorizáveis e tratáveis. O sistema, os ativos,
> os usuários e os componentes são os mesmos.

---

## Sumário

8. [Critérios de avaliação de risco](#8-critérios-de-avaliação-de-risco)
9. [Registro de riscos](#9-registro-de-riscos)
10. [Justificativas das avaliações](#10-justificativas-das-avaliações)
11. [Priorização](#11-priorização)
12. [Estratégias de tratamento](#12-estratégias-de-tratamento)
13. [Funções do NIST CSF 2.0 e mapeamento](#13-funções-do-nist-csf-20-e-mapeamento)
14. [Plano de tratamento](#14-plano-de-tratamento)
15. [Ordem inicial de implementação](#15-ordem-inicial-de-implementação)
16. [Estimativa de risco residual](#16-estimativa-de-risco-residual)
17. [Considerações finais da Etapa 2](#17-considerações-finais-da-etapa-2)

### Vocabulário adotado

O enunciado avalia explicitamente a **diferenciação entre ameaça, vulnerabilidade, ataque e
risco**. Usamos estas definições em todo o documento:

| Termo | Definição usada | Exemplo no SaborExpress |
|---|---|---|
| **Ameaça** | Fonte ou evento potencial capaz de causar dano | Falsificação da identidade de um cliente (T01) |
| **Vulnerabilidade** | Fraqueza que permite a ameaça se concretizar | Login sem segundo fator e sem limite de tentativas |
| **Ataque** | A exploração concreta da vulnerabilidade por um agente | *Credential stuffing* com uma lista de senhas vazadas |
| **Risco** | Combinação da probabilidade do evento com o impacto de suas consequências | Contas de clientes tomadas, gerando pedidos fraudulentos e vazamento de endereços (R01) |

---

## 8. Critérios de avaliação de risco

<!-- RESPONSÁVEL: Fernando -->

### 8.1 Critérios de probabilidade

A avaliação da probabilidade de ocorrência de ameaças no sistema SaborExpress baseia-se em uma escala de 1 (Baixa) a 4 (Alta). Com base na literatura científica de segurança (NIST SP 800-30 e ISO/IEC 27005), as justificativas para cada nível são delineadas a seguir, considerando o perfil dos atacantes, as barreiras de proteção existentes e as facilidades técnicas de exploração:

| Valor | Classificação | Critério |
|---|---|---|
| 1 | Baixa | Reservada para eventos que demandam condições operacionais incomuns, controle de canais físicos, ou habilidades de alto nível técnico (como quebra teórica de algoritmos de criptografia em trânsito ou ataques direcionados de dia zero - "zero-day"). No contexto de uma aplicação de delivery de comida, o incentivo econômico para que atacantes com essa sofisticação executem tais ações é estatisticamente insignificante, tornando o evento raro. |
| 2 | Média-baixa | Aplica-se a ameaças viáveis no contexto web, porém limitadas por vulnerabilidades específicas ou que dependem de condições circunstanciais e intervenção de rede (ex.: alteração do valor de pedidos se houver uma falha muito pontual de consistência de sessão). Exige um nível moderado de capacidade técnica do atacante ou o descobrimento de caminhos de exploração não triviais. |
| 3 | Média-alta | Caracteriza ameaças cuja exploração se baseia em comportamentos comuns dos usuários (como reuso de senhas em plataformas digitais) combinados com a ausência de proteções modernas. O ataque de credential stuffing (R01) e a raspagem automática de APIs públicas sem autenticação são exemplos cotidianos em aplicações web. O uso de scripts prontos e amplamente distribuídos permite a exploração por agentes de baixa qualificação técnica. |
| 4 | Alta | Aplica-se a eventos que podem ocorrer com facilidade extrema devido à completa ausência de controles defensivos mínimos em pontos públicos de interação do SaborExpress (como a manipulação de parâmetros de URLs sem validação de autorização). Qualquer usuário com conhecimento básico de requisições HTTP pode reproduzir o ataque, gerando ocorrências de alta frequência durante a operação regular do sistema. |

<!-- TODO(Fernando): acrescentar 1 parágrafo explicando quais características do SaborExpress o
     grupo considera ao atribuir probabilidade — por exemplo: existência de incentivo financeiro,
     necessidade ou não de conhecimento técnico, se o ataque é automatizável, se o agente já é um
     usuário legítimo com acesso, e se a vulnerabilidade é comum em sistemas desse tipo. -->

### 8.2 Critérios de impacto

A mensuração do impacto utiliza uma escala de 1 (Baixo) a 4 (Muito alto). Esta gradação reflete os danos potenciais sob a ótica da privacidade de dados (LGPD), perdas financeiras operacionais, integridade transacional e desgaste de reputação mercadológica da plataforma SaborExpress:

| Valor | Classificação | Critério |
|---|---|---|
| 1 | Baixo | Eventos que resultam em pequenos transtornos operacionais à equipe interna ou inconsistências de exibição de dados não críticos para os usuários. A remediação é imediata por meio de procedimentos padrão de rotina, sem qualquer reflexo financeiro, regulatório ou de publicidade negativa. |
| 2 | Moderado | Ocorrências que provocam interrupções localizadas e de curta duração em funcionalidades secundárias da plataforma. Embora causem insatisfação momentânea a um grupo reduzido de usuários, os dados sensíveis permanecem íntegros, e a capacidade de restauração e conciliação do serviço ocorre em poucas horas, sem multas legais associadas. |
| 3 | Alto | Eventos que comprometem a integridade financeira das transações (adulteração de preços - R02) ou violam dados de privacidade localizados. Provoca prejuízos diretos ao faturamento da empresa ou aos lucros de restaurantes parceiros, além de reclamações de segurança que podem gerar notificações administrativas leves e demandar esforços de correção de média complexidade de engenharia. |
| 4 | Muito alto | Desastres de segurança cibernética que causam danos de ampla escala, como a exfiltração em massa de dados cadastrais e de geolocalização de clientes (R03). Sob os termos da Lei Geral de Proteção de Dados (LGPD), este tipo de incidente resulta em sanções severas (multas pesadas, publicização da infração), além de ações judiciais coletivas, quebra irremediável da reputação do SaborExpress frente ao mercado e grande risco à segurança física dos usuários devido à exposição de endereços residenciais. |

<!-- TODO(Fernando): acrescentar 1 parágrafo indicando quais dimensões de impacto o grupo pesa:
     prejuízo aos usuários, exposição de dados, perdas financeiras, interrupção do serviço,
     consequências jurídicas (LGPD), dano à reputação, dificuldade de recuperação e número de
     pessoas afetadas. Registrar que danos físicos a pessoas (caso do endereço residencial)
     recebem automaticamente impacto 4. -->

### 8.3 Cálculo e classificação

**Pontuação = Probabilidade × Impacto**

| Pontuação | Nível do risco |
|---|---|
| 1 a 3 | Baixo |
| 4 a 7 | Médio |
| 8 a 11 | Alto |
| 12 a 16 | Crítico |

A pontuação serve para comparar riscos, mas **não substitui a análise de contexto**: dois riscos
com a mesma pontuação podem receber prioridades diferentes quando suas consequências,
dependências ou possibilidades de recuperação forem distintas (ver seção 11).

---

## 9. Registro de riscos

<!-- RESPONSÁVEIS: Fernando (consolidação e tabela mestre); Gabriel (riscos de S/T/R);
     Luis Fillipe (riscos de I/D/E) -->
<!-- TODO: cada ameaça relevante da Etapa 1 deve originar pelo menos um risco. Quando uma ameaça
     puder causar consequências diferentes, criar mais de um risco para ela.
     R01 e R02 abaixo são exemplos de formato e profundidade — revise-os e complete a tabela. -->

| ID | Origem STRIDE | Evento de risco | Vulnerabilidade ou condição | Prob. | Imp. | Pont. | Nível |
|---|---|---|---|---|---|---|---|
| R01 | T01 — Spoofing | Um atacante assume a conta de clientes e realiza pedidos e alterações em nome deles | Credenciais reaproveitadas de outros vazamentos; ausência de MFA e de limite de tentativas de login | 3 | 4 | 12 | Crítico |
| R02 | T07 — Tampering | O valor de pedidos é alterado antes do pagamento, gerando cobrança menor que a devida | O servidor confia no total calculado pelo aplicativo em vez de recalculá-lo | 2 | 3 | 6 | Médio |
| R03 | T18 — Information Disclosure | Dados pessoais de clientes (endereço, telefone) são extraídos em massa pela API | Falha de autorização por objeto (IDOR) e ausência de limite de requisições | 3 | 4 | 12 | Crítico |
| R04 | — | — | — | — | — | — | — |

<!-- TODO(Gabriel): riscos originados de T01–T17 (Spoofing, Tampering, Repudiation).
     TODO(Luis): riscos originados de T18–T33 (Information Disclosure, DoS, Elevation of Privilege).
     TODO(Fernando): consolidar tudo nesta tabela única, conferir os cálculos e garantir que a
     numeração R## seja contínua e sem repetições. -->

---

## 10. Justificativas das avaliações

<!-- RESPONSÁVEIS: quem escreveu cada risco justifica o seu. Fernando revisa a coerência. -->
<!-- TODO: para CADA risco, responder aos cinco pontos. Não basta apresentar números.
     R01 abaixo é o modelo de profundidade esperado. -->

### R01 — Tomada de contas de clientes

- **Por que a probabilidade é 3 (média-alta):** o ataque é totalmente automatizável e não exige
  capacidade técnica avançada — listas de credenciais vazadas são públicas e ferramentas de
  *credential stuffing* são amplamente distribuídas. A reutilização de senhas entre serviços é
  comportamento comum entre os clientes, e o sistema não impõe MFA nem bloqueio após tentativas
  sucessivas. Não é 4 porque cada conta individual ainda depende de o cliente ter reutilizado uma
  senha já vazada.
- **Por que o impacto é 4 (muito alto):** a conta comprometida dá acesso simultâneo ao endereço
  residencial, ao telefone, ao histórico de pedidos e ao meio de pagamento salvo. Combina prejuízo
  financeiro, violação de privacidade sob a LGPD e risco físico à pessoa. Um único vazamento de
  lista pode afetar milhares de contas ao mesmo tempo.
- **Quem ou o que é afetado:** clientes (A01, A02, A03), a área financeira da plataforma e o
  atendimento, que absorve o custo das contestações.
- **Consequências possíveis:** pedidos fraudulentos, contestação de cartão (*chargeback*),
  exposição de endereço residencial, perda de confiança e notificação à ANPD em caso de incidente
  relevante.
- **Por que o nível Crítico é adequado:** o evento é plausível no dia a dia, atinge muitos
  usuários simultaneamente e sua recuperação é lenta — dados pessoais expostos não podem ser
  "desvazados".

### R02 — <!-- TODO -->
<!-- TODO: repetir a estrutura de cinco pontos para todos os riscos registrados. -->

---

## 11. Priorização

<!-- RESPONSÁVEL: Fernando -->
<!-- TODO: ordenar os riscos e JUSTIFICAR por que um deve ser tratado antes do outro.
     A ordem não pode ser apenas a pontuação decrescente — o enunciado pede que se considere
     também gravidade das consequências, número de usuários afetados, importância do ativo,
     possibilidade de recuperação, dependências entre riscos e urgência. -->

| Ordem | Risco | Pontuação | Nível | Motivo de estar nesta posição |
|---|---|---|---|---|
| 1º | R03 | 12 | Crítico | Mesma pontuação de R01, mas colocado à frente porque o vazamento em massa é irreversível: dados exfiltrados não podem ser recuperados, enquanto uma conta tomada pode ser bloqueada e as transações estornadas |
| 2º | R01 | 12 | Crítico | <!-- TODO --> |
| 3º | — | — | — | <!-- TODO --> |

<!-- TODO: acrescentar 1 ou 2 parágrafos explicando o raciocínio geral da priorização e
     apontando as dependências entre riscos (por exemplo: tratar a falha de autorização da API
     reduz simultaneamente vários riscos de vazamento e de escalonamento de privilégio). -->

---

## 12. Estratégias de tratamento

<!-- RESPONSÁVEL: Deivid -->

| Estratégia | Descrição |
|---|---|
| **Evitar** | Eliminar a atividade ou condição que dá origem ao risco |
| **Reduzir** | Implementar medidas para diminuir sua probabilidade ou seu impacto |
| **Compartilhar** | Atribuir parte da operação ou das consequências a um terceiro |
| **Aceitar** | Reconhecer e manter conscientemente o risco, com justificativa e acompanhamento |

<!-- TODO(Deivid): escolher e JUSTIFICAR uma estratégia principal por risco. Registrar a escolha
     na tabela da seção 14.
     Atenção: se algum risco for ACEITO, é obrigatório indicar (a) o motivo da decisão,
     (b) quem aprova, (c) sob quais condições o risco é aceito e (d) quando a decisão será
     revisada. Aceitar não é ignorar. -->

### 12.1 Riscos aceitos e suas condições

| Risco | Motivo da aceitação | Quem aprova | Condições | Revisão |
|---|---|---|---|---|
| — | <!-- TODO --> | | | |

---

## 13. Funções do NIST CSF 2.0 e mapeamento

<!-- RESPONSÁVEL: Murillo -->

### 13.1 As seis funções

| Função | Finalidade | Como se aplica ao SaborExpress |
|---|---|---|
| **Govern(Governar)** | Responsável por estabelecer as diretrizes normativas de segurança e privacidade do SaborExpress, definindo os padrões de conformidade com a LGPD e políticas internas. <br>&nbsp; - *Resultado Esperado:* Que a segurança da informação seja um requisito obrigatório e formal de governança desde o desenvolvimento até o ciclo de entrega de software (DevSecOps). <br>&nbsp; - *Controle Proposto:* Instituição de uma política corporativa que condicione o lançamento de novos recursos à passagem por testes automáticos de segurança e que torne o uso de autenticação robusta obrigatório para toda a equipe administrativa e clientes em novos dispositivos. | <!-- TODO(Murillo) --> |
| **Identify(Identificar)** | Mapeia o escopo tecnológico, identificando vulnerabilidades latentes, ativos críticos e dependências do ecossistema. <br>&nbsp; - *Resultado Esperado:* Visibilidade integral sobre quais APIs expõem dados sensíveis de clientes e onde estão armazenados os ativos importantes da plataforma. <br>&nbsp; - *Controle Proposto:* Manutenção automatizada de uma documentação viva de endpoints de API (utilizando Swagger/OpenAPI) e execução mensal de varreduras de conformidade de código e ativos expostos. | <!-- TODO(Murillo) --> |
| **Protect(Proteger)** | Aplica salvaguardas tecnológicas para blindar o sistema contra possíveis explorações maliciosas. <br>&nbsp; - *Resultado Esperado:* Isolamento e autorização adequada para o acesso a recursos confidenciais das contas dos clientes e checkout de pedidos. <br>&nbsp; - *Controle Proposto:* Implementação de middlewares de autorização por propriedade de recurso (IDOR Mitigation) e restrição de taxas de requisição por IP e token (*rate limiting*). | <!-- TODO(Murillo) --> |
| **Detect(Detectar)** | Garante a capacidade de reconhecer anomalias e tentativas de exploração em tempo oportuno. <br>&nbsp; - *Resultado Esperado:* Visualização de tráfego anômalo e de tentativas automatizadas de quebra de credenciais antes que ocorra a exfiltração de dados. <br>&nbsp; - *Controle Proposto:* Criação de alertas automáticos em logs centralizados no caso de disparo de erros HTTP 403 (Unauthorized) sucessivos por um único endereço IP. | <!-- TODO(Murillo) --> |
| **Respond(Responder)** | Estabelece o fluxo de contenção imediata de incidentes operacionais detectados. <br>&nbsp; - *Resultado Esperado:* Isolamento do vetor de ataque ativo e interrupção do dano para evitar sua propagação lateral. <br>&nbsp; - *Controle Proposto:* Bloqueio temporário automatizado de credenciais e IPs sinalizados como atacantes ativos na camada de aplicação. | <!-- TODO(Murillo) --> |
| **Recover(Recuperar)** | Garante a resiliência operacional para restabelecer os serviços do SaborExpress e as comunicações regulatórias devidas. <br>&nbsp; - *Resultado Esperado:* Restauração rápida da normalidade transacional pós-incidente e conformidade com as obrigações legais de aviso à ANPD. <br>&nbsp; - *Controle Proposto:* Mecanismo robusto de backup distribuído e rotina pré-configurada para envio automatizado de e-mails para reset forçado de senhas afetadas. | <!-- TODO(Murillo) --> |

> **Distinção exigida pelo enunciado — função ≠ resultado esperado ≠ controle:**
> *Protect* é uma **função**; "proteger o acesso às contas de cliente" é um **resultado
> esperado**; "exigir MFA por TOTP em todo login a partir de dispositivo novo" é um **controle**.
> O documento deve manter essa distinção em todas as seções.

### 13.2 Mapeamento dos riscos para as funções

<!-- TODO(Murillo): analisar cada relação. NÃO marcar todas as funções automaticamente — o
     enunciado adverte contra isso. Marque apenas quando houver um resultado de segurança
     concreto a ser alcançado naquela função, e explique as escolhas menos óbvias na coluna final. -->

| Risco | Govern | Identify | Protect | Detect | Respond | Recover | Observação |
|---|:---:|:---:|:---:|:---:|:---:|:---:|---|
| R01 | X | | X | X | X | X | *Govern* entra porque é preciso uma política que torne o MFA obrigatório; *Identify* não foi marcado porque o ativo e a vulnerabilidade já estão plenamente mapeados e não há trabalho adicional de descoberta |
| R02 | | | X | X | | | <!-- TODO --> |
| R03 | | | | | | | <!-- TODO --> |

---

## 14. Plano de tratamento

<!-- RESPONSÁVEIS: Deivid (consolidação, responsáveis e evidências); Gabriel e Luis Fillipe
     (controles dos riscos que cada um registrou) -->
<!-- TODO: os controles devem ser ESPECÍFICOS E OBSERVÁVEIS. O enunciado rejeita explicitamente
     propostas genéricas como "aumentar a segurança", "usar criptografia", "melhorar a
     autenticação", "utilizar o NIST" ou "monitorar o sistema". Sempre que uma dessas ideias for
     usada, é obrigatório dizer ONDE se aplica, QUAL problema reduz, COMO funciona, QUEM é
     responsável e COMO será verificada.
     A linha R01 é o modelo de especificidade esperado. -->

| Risco | Estratégia | Controles propostos | Funções NIST | Responsáveis | Evidências e verificação |
|---|---|---|---|---|---|
| R01 — Tomada de contas de clientes | Reduzir | (1) MFA por TOTP ou código SMS obrigatório em todo login vindo de dispositivo/IP não reconhecido; (2) limite de 5 tentativas de login por conta e por IP em 15 minutos, com bloqueio temporário progressivo; (3) verificação da senha escolhida contra base de senhas vazadas no cadastro e na troca; (4) notificação por e-mail e push a cada novo dispositivo autenticado; (5) reautenticação obrigatória para trocar e-mail, telefone ou meio de pagamento | Govern, Protect, Detect, Respond, Recover | Time de Desenvolvimento (backend e mobile) e Segurança da Informação | Teste de autenticação em ambiente de homologação cobrindo dispositivo novo; relatório de tentativas bloqueadas pelo *rate limit*; simulação de conta comprometida (exercício de mesa) com registro do tempo até o bloqueio; amostragem de logs comprovando o envio das notificações |
| R02 | <!-- TODO --> | | | | |
| R03 | <!-- TODO --> | | | | |

---

## 15. Ordem inicial de implementação

<!-- RESPONSÁVEL: Deivid -->
<!-- TODO: definir e justificar a ordem. Considerar: riscos críticos e altos primeiro;
     dependências técnicas; controles que reduzem VÁRIOS riscos de uma vez (esses costumam vir
     antes mesmo com pontuação menor); custo e complexidade; recursos disponíveis; necessidade de
     uma política ou decisão prévia; urgência. -->

| Ordem | Controle | Riscos que trata | Justificativa |
|---|---|---|---|
| 1 | Autorização por objeto (verificação de propriedade) em todos os endpoints da API | R03 e demais riscos de vazamento e escalonamento | É a correção que reduz o maior número de riscos simultaneamente e não depende de nenhuma outra decisão prévia |
| 2 | <!-- TODO --> | | |
| 3 | <!-- TODO --> | | |

---

## 16. Estimativa de risco residual

<!-- RESPONSÁVEL: Felipe -->
<!-- TODO: preencher após a seção 14. Atenção: é uma ESTIMATIVA. O grupo não pode afirmar que o
     risco foi reduzido só porque um controle foi proposto — a redução só se confirma após
     implementação, testes e obtenção de evidências. Registrar isso explicitamente. -->

| Risco | Nível inicial | Nível residual esperado | Condição para aceitar o residual |
|---|---|---|---|
| R01 | Crítico (12) | Médio (6) | MFA ativo para 100% dos logins de dispositivo novo, *rate limit* verificado em teste e taxa de tomada de conta monitorada mensalmente abaixo do limiar definido pela governança |
| R02 | <!-- TODO --> | | |
| R03 | <!-- TODO --> | | |

> **Limitação declarada:** os níveis residuais acima são estimativas baseadas na expectativa de
> eficácia dos controles propostos. Nenhum controle foi implementado ou testado no âmbito deste
> trabalho, e a redução efetiva só poderá ser confirmada mediante evidências.

---

## 17. Considerações finais da Etapa 2

<!-- RESPONSÁVEL: Felipe -->
<!-- TODO: escrever ao final, cobrindo explicitamente os oito pontos abaixo. -->

### 17.1 Riscos considerados mais importantes
<!-- TODO -->

### 17.2 Razões que determinaram a priorização
<!-- TODO -->

### 17.3 Estratégias de tratamento predominantes
<!-- TODO: qual estratégia predominou e por quê. -->

### 17.4 Funções do NIST mais relevantes para o sistema
<!-- TODO -->

### 17.5 Controles considerados essenciais
<!-- TODO -->

### 17.6 Principais dificuldades encontradas
<!-- TODO -->

### 17.7 Limitações da avaliação
<!-- TODO: por exemplo — estimativas sem dados históricos de incidentes; sistema não
     implementado, o que impede testes; ausência de custo real dos controles. -->

### 17.8 Pontos a detalhar nas próximas etapas
<!-- TODO -->

---

**Volta para:** [Etapa 1 — Casos de Abuso e Modelagem de Ameaças](etapa-1-ameacas-stride.md)
