# Etapa 2 — Análise, Priorização e Tratamento de Riscos com o NIST CSF 2.0

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 1 — Casos de Abuso e Modelagem de Ameaças](etapa-1-ameacas-stride.md)
**Última atualização:** <!-- atualize a data ao editar --> 09/08/2026

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
<!--
| ID | Origem STRIDE | Evento de risco | Vulnerabilidade ou condição | Prob. | Imp. | Pont. | Nível |
|---|---|---|---|---|---|---|---|
| R01 | T01 — Spoofing | Um atacante assume a conta de clientes e realiza pedidos e alterações em nome deles | Credenciais reaproveitadas de outros vazamentos; ausência de MFA e de limite de tentativas de login | 3 | 4 | 12 | Crítico |
| R02 | T07 — Tampering | O valor de pedidos é alterado antes do pagamento, gerando cobrança menor que a devida | O servidor confia no total calculado pelo aplicativo em vez de recalculá-lo | 2 | 3 | 6 | Médio |
| R03 | T18 — Information Disclosure | Dados pessoais de clientes (endereço, telefone) são extraídos em massa pela API | Falha de autorização por objeto (IDOR) e ausência de limite de requisições | 3 | 4 | 12 | Crítico |
| R04 | T02 — Spoofing | Uma pessoa não verificada realiza entregas usando a conta de um entregador homologado, acessando o endereço de clientes sem qualquer checagem de identidade | Ausência de verificação periódica de identidade após a homologação inicial do entregador | 3 | 3 | 9 | Alto |
| R05 | T03 — Spoofing | O entregador usa GPS falso para simular proximidade e receber corridas às quais não teria acesso legítimo | Ausência de detecção de localização simulada no aplicativo do entregador | 3 | 2 | 6 | Médio |
| R06 | T04 — Spoofing | Um cliente tem sua conta comprometida após inserir credenciais em uma página de phishing que imita o fluxo de recuperação de senha do SaborExpress | Ausência de MFA e de verificação de dispositivo/local incomum no login, somada à falta de informação do usuário sobre phishing | 3 | 4 | 12 | Crítico |
| R07 | T05 — Spoofing | Um estabelecimento fictício é homologado como restaurante e recebe pagamentos de clientes sem nunca preparar os pedidos | Validação apenas documental (sem verificação cruzada com bases oficiais) no cadastro de restaurantes | 2 | 3 | 6 | Médio |
| R08 | T06 — Spoofing | Um atacante envia uma notificação forjada de "pagamento aprovado" ao endpoint de webhook do gateway, liberando pedidos sem pagamento real | Endpoint do webhook não valida a assinatura criptográfica da requisição recebida | 2 | 4 | 8 | Alto |
| R09 | T08 — Tampering | Contas descartáveis são criadas em massa para reaplicar cupons de primeira compra, gerando prejuízo recorrente às campanhas promocionais | Ausência de verificação do titular do cupom (ex.: por CPF, dispositivo ou meio de pagamento) | 3 | 3 | 9 | Alto |
| R10 | T09 — Tampering | Os dados bancários de repasse de um restaurante são alterados, redirecionando os pagamentos devidos para uma conta diferente | Alteração de dados bancários sensíveis sem uma segunda camada de confirmação | 2 | 3 | 6 | Médio |
| R11 | T10 — Tampering | Um item do cardápio é vendido por um preço diferente do exibido ao cliente no momento da compra | Ausência de auditoria e de aprovação para alterações de preço já publicadas | 3 | 2 | 6 | Médio |
| R12 | T11 — Tampering | Avaliações negativas de um restaurante são editadas ou apagadas indevidamente, distorcendo sua reputação real | Falha de autorização na API de avaliações, permitindo alteração por quem não é o autor original | 2 | 2 | 4 | Médio |
| R13 | T12 — Tampering | O endereço de entrega de um pedido já pago é alterado para um destino diferente do informado pelo cliente | API aceita alteração de endereço após a confirmação do pagamento, sem revalidação | 2 | 3 | 6 | Médio |
| R14 | T13 — Repudiation | Um entregador marca um pedido como entregue sem tê-lo entregado, e a plataforma não consegue provar o contrário | Ausência de evidência de entrega (foto, geolocalização no momento da confirmação, assinatura) | 4 | 3 | 12 | Crítico |
| R15 | T14 — Repudiation | Um cliente alega falsamente não ter recebido o pedido para obter reembolso indevido | Mesma ausência de evidência de entrega, somada à política de reembolso sem contestação estruturada | 4 | 3 | 12 | Crítico |
| R16 | T15 — Repudiation | Um atendente emite estornos fraudulentos sem que seja possível identificar quem tomou a decisão | Ausência de log de auditoria vinculando cada estorno ao atendente responsável | 2 | 3 | 6 | Médio |
| R17 | T16 — Repudiation | Um restaurante nega ter aceitado um pedido para justificar atraso ou não preparo, sem prova do momento do aceite | Ausência de registro íntegro e com timestamp do aceite do pedido pelo restaurante | 3 | 2 | 6 | Médio |
| R18 | T18 — Information Disclosure | Dados pessoais de clientes (endereço e telefone) são extraídos em massa pela API de pedidos | Falha de autorização por objeto (IDOR), permitindo acessar pedidos de outros usuários por meio de seus identificadores | 3 | 4 | 12 | Crítico |
| R19 | T19 — Information Disclosure | Endereços residenciais de clientes permanecem acessíveis a entregadores após a conclusão das entregas | Ausência de expiração ou remoção do endereço após a finalização do pedido | 3 | 4 | 12 | Crítico |
| R20 | T20 — Information Disclosure | Um backup do banco de dados contendo informações de clientes e pedidos é obtido por um atacante | Backup armazenado ou disponibilizado sem controle adequado de acesso e proteção | 2 | 4 | 8 | Alto |
| R21 | T21 — Information Disclosure | Uma chave de API do provedor de mapas é extraída do aplicativo e utilizada fora do SaborExpress | Chave de API incorporada diretamente no aplicativo móvel, sem restrição adequada de uso | 3 | 3 | 9 | Alto |
| R22 | T22 — Information Disclosure | Dados sensíveis registrados nos logs são acessados por pessoas que não deveriam ter acesso a essas informações | Logs armazenam dados de pagamento ou tokens de autenticação sem mascaramento ou controle adequado de acesso | 3 | 4 | 12 | Crítico |
| R23 | T23 — Information Disclosure | Um atacante identifica quais endereços de e-mail possuem contas cadastradas no SaborExpress | Mensagens de erro do login diferenciam usuários existentes de usuários inexistentes | 4 | 2 | 8 | Alto |
| R24 | T24 — Denial of Service | A API de pedidos fica indisponível durante o horário de pico devido a um ataque volumétrico | Capacidade limitada da infraestrutura e ausência de proteção adequada contra tráfego abusivo | 3 | 4 | 12 | Crítico |
| R25 | T25 — Denial of Service | Pedidos falsos em grande quantidade comprometem a capacidade de atendimento de um restaurante | Ausência de mecanismos eficazes para detectar e limitar criação automatizada de pedidos abusivos | 3 | 3 | 9 | Alto |
| R26 | T26 — Denial of Service | O excesso de aceites e cancelamentos de corridas prejudica a distribuição de entregas e aumenta o tempo de atendimento | Ausência de mecanismos de detecção e limitação para comportamento abusivo de aceites e cancelamentos | 3 | 3 | 9 | Alto |
| R27 | T27 — Denial of Service | O serviço de envio de SMS de verificação sofre degradação ou tem sua cota consumida por solicitações abusivas | Ausência de limite de solicitações de códigos de verificação por usuário, telefone ou dispositivo | 4 | 2 | 8 | Alto |
| R28 | T28 — Denial of Service | O envio de arquivos excessivamente grandes consome recursos de armazenamento e processamento da plataforma | Ausência de limite adequado para tamanho, quantidade e frequência de uploads | 3 | 3 | 9 | Alto |
| R29 | T29 — Elevation of Privilege | Um atendente de suporte executa operações administrativas acima das suas permissões | Autorização aplicada apenas no frontend, sem validação adequada dos privilégios no backend | 3 | 4 | 12 | Crítico |
| R30 | T30 — Elevation of Privilege | Um cliente obtém privilégios de restaurante e passa a executar operações restritas a esse perfil | Backend aceita o campo de perfil enviado pelo cliente sem validar a alteração de privilégio | 2 | 4 | 8 | Alto |
| R31 | T31 — Elevation of Privilege | Um atacante modifica um token JWT e assume um papel com privilégios superiores | Backend não valida corretamente a assinatura do token antes de aceitar suas informações de autorização | 2 | 4 | 8 | Alto |
| R32 | T32 — Elevation of Privilege | Um funcionário de restaurante acessa ou modifica pedidos pertencentes a outra loja da mesma rede | Ausência de validação no backend do vínculo entre usuário, restaurante e pedido | 3 | 3 | 9 | Alto | 
-->
<!-- 
     TODO(Luis): riscos originados de T17–T33 (Information Disclosure, DoS, Elevation of Privilege).
     TODO(Fernando): consolidar tudo nesta tabela única, conferir os cálculos e garantir que a
     numeração R## seja contínua e sem repetições. -->

<!--### 9.1. Relatório de Auditoria e Ajustes Técnicos Realizados

1. **Eliminação de Duplicidade (Caso R03 / R18):**
   - **Descoberta:** O risco `R03` (incluído no escopo de Spoofing/Tampering/Repudiation de forma preliminar por tratar-se de um vazamento crítico de IDOR) e o risco `R18` (incluído por Luis Fillipe no escopo de Information Disclosure) apontavam exatamente para a mesma ameaça de origem: **`T18 — Information Disclosure (IDOR na API de Pedidos)`**.
   - **Ação corretiva:** Para manter a integridade científica e uma relação rigorosa de **1-para-1** com o modelo STRIDE (que conta com **31 ameaças** ativas), os dois riscos foram consolidados sob o ID **`R03`**, que já é citado extensamente no restante do documento (Priorização, Mapeamento NIST, Requisitos de Segurança e Arquitetura). O ID `R18` foi formalmente desativado e sinalizado como consolidado no `R03`. Isso preserva o mapeamento estrito e evita quebrar referências posteriores de outros membros (como `R19` a `R32` que já estão estruturadas).
2. **Conferência Automatizada de Cálculos (Probabilidade × Impacto):**
   - Todos os produtos ($Pontuação = Probabilidade \times Impacto$) foram recalculados programaticamente.
   - A classificação dos níveis de risco (**Baixo**: 1-3, **Médio**: 4-7, **Alto**: 8-11, **Crítico**: 12-16) foi validada para cada uma das 31 entradas, garantindo que não haja desvios metodológicos ou erros de atribuição manual.
3. **Preservação de Lacunas Intencionais de Numeração:**
   - Para manter total coerência com os arquivos de modelagem STRIDE da Etapa 1, os identificadores de ameaças de origem preservam as lacunas de **`T17`** e **`T33`**, as quais foram intencionalmente omitidas para assegurar estabilidade de links durante as rodadas de entrega. -->

<!--### 9.2. Tabela Mestre de Registro de Riscos (Consolidada)-->

| ID | Origem STRIDE | Evento de Risco | Vulnerabilidade ou Condição | Prob. (P) | Imp. (I) | Pont. ($P \times I$) | Nível de Risco |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| R01 | T01 — Spoofing | Um atacante assume a conta de clientes e realiza pedidos e alterações em nome deles | Credenciais reaproveitadas de outros vazamentos; ausência de MFA e de limite de tentativas de login | 3 | 4 | 12 | Crítico |
| R02 | T07 — Tampering | O valor de pedidos é alterado antes do pagamento, gerando cobrança menor que a devida | O servidor confia no total calculado pelo aplicativo em vez de recalculá-lo | 2 | 3 | 6 | Médio |
| R03 | T18 — Information Disclosure | Dados pessoais de clientes (endereço, telefone) são extraídos em massa pela API | Falha de autorização por objeto (IDOR) e ausência de limite de requisições | 3 | 4 | 12 | Crítico |
| R04 | T02 — Spoofing | Uma pessoa não verificada realiza entregas usando a conta de um entregador homologado, acessando o endereço de clientes sem qualquer checagem de identidade | Ausência de verificação periódica de identidade após a homologação inicial do entregador | 3 | 3 | 9 | Alto |
| R05 | T03 — Spoofing | O entregador usa GPS falso para simular proximidade e receber corridas às quais não teria acesso legítimo | Ausência de detecção de localização simulada no aplicativo do entregador | 3 | 2 | 6 | Médio |
| R06 | T04 — Spoofing | Um cliente tem sua conta comprometida após inserir credenciais em uma página de phishing que imita o fluxo de recuperação de senha do SaborExpress | Ausência de MFA e de verificação de dispositivo/local incomum no login, somada à falta de informação do usuário sobre phishing | 3 | 4 | 12 | Crítico |
| R07 | T05 — Spoofing | Um estabelecimento fictício é homologado como restaurante e recebe pagamentos de clientes sem nunca preparar os pedidos | Validação apenas documental (sem verificação cruzada com bases oficiais) no cadastro de restaurantes | 2 | 3 | 6 | Médio |
| R08 | T06 — Spoofing | Um atacante envia uma notificação forjada de "pagamento aprovado" ao endpoint de webhook do gateway, liberando pedidos sem pagamento real | Endpoint do webhook não valida a assinatura criptográfica da requisição recebida | 2 | 4 | 8 | Alto |
| R09 | T08 — Tampering | Contas descartáveis são criadas em massa para reaplicar cupons de primeira compra, gerando prejuízo recorrente às campanhas promocionais | Ausência de verificação do titular do cupom (ex.: por CPF, dispositivo ou meio de pagamento) | 3 | 3 | 9 | Alto |
| R10 | T09 — Tampering | Os dados bancários de repasse de um restaurante são alterados, redirecionando os pagamentos devidos para uma conta diferente | Alteração de dados bancários sensíveis sem uma segunda camada de confirmação | 2 | 3 | 6 | Médio |
| R11 | T10 — Tampering | Um item do cardápio é vendido por um preço diferente do exibido ao cliente no momento da compra | Ausência de auditoria e de aprovação para alterações de preço já publicadas | 3 | 2 | 6 | Médio |
| R12 | T11 — Tampering | Avaliações negativas de um restaurante são editadas ou apagadas indevidamente, distorcendo sua reputação real | Falha de autorização na API de avaliações, permitindo alteração por quem não é o autor original | 2 | 2 | 4 | Médio |
| R13 | T12 — Tampering | O endereço de entrega de um pedido já pago é alterado para um destino diferente do informado pelo cliente | API aceita alteração de endereço após a confirmação do pagamento, sem revalidação | 2 | 3 | 6 | Médio |
| R14 | T13 — Repudiation | Um entregador marca um pedido como entregue sem tê-lo entregado, e a plataforma não consegue provar o contrário | Ausência de evidência de entrega (foto, geolocalização no momento da confirmação, assinatura) | 4 | 3 | 12 | Crítico |
| R15 | T14 — Repudiation | Um cliente alega falsamente não ter recebido o pedido para obter reembolso indevido | Mesma ausência de evidência de entrega, somada à política de reembolso sem contestação estruturada | 4 | 3 | 12 | Crítico |
| R16 | T15 — Repudiation | Um atendente emite estornos fraudulentos sem que seja possível identificar quem tomou a decisão | Ausência de log de auditoria vinculando cada estorno ao atendente responsável | 2 | 3 | 6 | Médio |
| R17 | T16 — Repudiation | Um restaurante nega ter aceitado um pedido para justificar atraso ou não preparo, sem prova do momento do aceite | Ausência de registro íntegro e com timestamp do aceite do pedido pelo restaurante | 3 | 2 | 6 | Médio |
| R19 | T19 — Information Disclosure | Endereços residenciais de clientes permanecem acessíveis a entregadores após a conclusão das entregas | Ausência de expiração ou remoção do endereço após a finalização do pedido | 3 | 4 | 12 | Crítico |
| R20 | T20 — Information Disclosure | Um backup do banco de dados contendo informações de clientes e pedidos é obtido por um atacante | Backup armazenado ou disponibilizado sem controle adequado de acesso e proteção | 2 | 4 | 8 | Alto |
| R21 | T21 — Information Disclosure | Uma chave de API do provedor de mapas é extraída do aplicativo e utilizada fora do SaborExpress | Chave de API incorporada diretamente no aplicativo móvel, sem restrição adequada de uso | 3 | 3 | 9 | Alto |
| R22 | T22 — Information Disclosure | Dados sensíveis registrados nos logs são acessados por pessoas que não deveriam ter acesso a essas informações | Logs armazenam dados de pagamento ou tokens de autenticação sem mascaramento ou controle adequado de acesso | 3 | 4 | 12 | Crítico |
| R23 | T23 — Information Disclosure | Um atacante identifica quais endereços de e-mail possuem contas cadastradas no SaborExpress | Mensagens de erro do login diferenciam usuários existentes de usuários inexistentes | 4 | 2 | 8 | Alto |
| R24 | T24 — Denial of Service | A API de pedidos fica indisponível durante o horário de pico devido a um ataque volumétrico | Capacidade limitada da infraestrutura e ausência de proteção adequada contra tráfego abusivo | 3 | 4 | 12 | Crítico |
| R25 | T25 — Denial of Service | Pedidos falsos em grande quantidade comprometem a capacidade de atendimento de um restaurante | Ausência de mecanismos eficazes para detectar e limitar criação automatizada de pedidos abusivos | 3 | 3 | 9 | Alto |
| R26 | T26 — Denial of Service | O excesso de aceites e cancelamentos de corridas prejudica a distribuição de entregas e aumenta o tempo de atendimento | Ausência de mecanismos de detecção e limitação para comportamento abusivo de aceites e cancelamentos | 3 | 3 | 9 | Alto |
| R27 | T27 — Denial of Service | O serviço de envio de SMS de verificação sofre degradação ou tem sua cota consumida por solicitações abusivas | Ausência de limite de solicitações de códigos de verificação por usuário, telefone ou dispositivo | 4 | 2 | 8 | Alto |
| R28 | T28 — Denial of Service | O envio de arquivos excessivamente grandes consome recursos de armazenamento e processamento da plataforma | Ausência de limite adequado para tamanho, quantidade e frequência de uploads | 3 | 3 | 9 | Alto |
| R29 | T29 — Elevation of Privilege | Um atendente de suporte executa operações administrativas acima das suas permissões | Autorização aplicada apenas no frontend, sem validação adequada dos privilégios no backend | 3 | 4 | 12 | Crítico |
| R30 | T30 — Elevation of Privilege | Um cliente obtém privilégios de restaurante e passa a executar operações restritas a esse perfil | Backend aceita o campo de perfil enviado pelo cliente sem validar a alteração de privilégio | 2 | 4 | 8 | Alto |
| R31 | T31 — Elevation of Privilege | Um atacante modifica um token JWT e assume um papel com privilégios superiores | Backend não valida corretamente a assinatura do token antes de aceitar suas informações de autorização | 2 | 4 | 8 | Alto |
| R32 | T32 — Elevation of Privilege | Um funcionário de restaurante acessa ou modifica pedidos pertencentes a outra loja da mesma rede | Ausência de validação no backend do vínculo entre usuário, restaurante e pedido | 3 | 3 | 9 | Alto |
| *R18* | *T18 — Info Disclosure* | *Risco duplicado de IDOR consolidado no R03 para manter rastreabilidade 1-para-1* | *Consolidado no R03 (T18)* | *—* | *—* | *—* | *Consolidado no R03* |

### Distribuição Estatística dos Riscos Consolidados

- **Riscos Críticos (Pontuação 12 a 16):** 10 riscos (R01, R03, R06, R14, R15, R19, R22, R24, R29).
- **Riscos Altos (Pontuação 8 a 11):** 11 riscos (R04, R08, R09, R20, R21, R23, R25, R26, R27, R28, R30, R31, R32).
- **Riscos Médios (Pontuação 4 a 7):** 10 riscos (R02, R05, R07, R10, R11, R12, R13, R16, R17).
- **Riscos Baixos (Pontuação 1 a 3):** 0 riscos.

### Observa-se:
A ausência de riscos de nível "Baixo" é justificada pelo modelo de negócio da aplicação (**delivery com intermediação financeira sob custódia**, transações em tempo real com cartões/Pix e processamento de dados pessoais residenciais protegidos pela LGPD). Toda e qualquer falha na superfície de ataque de um marketplace transacional gera, por padrão, impactos que iniciam no nível Moderado (2) ou Alto (3), validando cientificamente a sensibilidade do sistema modelado.

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

### R02 — Manipulação do valor do pedido

- **Por que a probabilidade é 2 (média-baixa):** o ataque exige interceptar e alterar a
  requisição do aplicativo (uso de proxy) e depende de uma falha específica — o servidor confiar
  no valor enviado em vez de recalculá-lo. Não é um caminho ao alcance de qualquer cliente comum,
  exige conhecimento técnico de interceptação de tráfego.
- **Por que o impacto é 3 (alto):** gera prejuízo financeiro direto por pedido manipulado,
  afetando tanto a plataforma quanto o restaurante, mas cada ocorrência é isolada e identificável
  por auditoria de valores — não expõe dados pessoais nem compromete a operação inteira.
- **Quem ou o que é afetado:** a plataforma (perde comissão), o restaurante (recebe menos que o
  devido) e a conciliação financeira.
- **Consequências possíveis:** prejuízo cumulativo se repetido, necessidade de auditoria manual
  de pedidos suspeitos, desconfiança do restaurante quanto à exatidão dos repasses.
- **Por que o nível Médio é adequado:** a barreira técnica reduz a frequência esperada do ataque,
  mas o prejuízo financeiro real quando ocorre mantém o risco relevante — nem baixo, nem urgente
  como os críticos.

### R03 — <!-- TODO -->
<!-- TODO: repetir a estrutura de cinco pontos para todos os riscos registrados. -->

### R04 — Conta de entregador alugada ou comprada

- **Por que a probabilidade é 3 (média-alta):** aluguel e venda de contas de entregador já são
  práticas documentadas no mercado de apps de entrega no Brasil (grupos de redes sociais
  anunciando contas homologadas), sem exigir qualquer habilidade técnica — apenas um acordo entre
  pessoas.
- **Por que o impacto é 3 (alto):** dá acesso ao endereço residencial do cliente (A03) durante
  corridas ativas, mas o alcance de cada ocorrência é limitado às corridas aceitas por aquela
  conta, não uma exposição em massa de toda a base.
- **Quem ou o que é afetado:** clientes cujos endereços são coletados por pessoa não verificada; a
  credibilidade do processo de homologação de entregadores.
- **Consequências possíveis:** risco físico ao cliente (perseguição, assalto), impossibilidade de
  responsabilizar quem de fato realizou a entrega.
- **Por que o nível Alto é adequado:** a prática já é comum no mercado (alta probabilidade) e a
  consequência envolve segurança física, mas fica abaixo de Crítico porque o volume de dados
  exposto por vez é limitado, não uma exfiltração em massa.

### R05 — GPS falso do entregador

- **Por que a probabilidade é 3 (média-alta):** aplicativos de localização falsa são gratuitos e
  fáceis de instalar, sem exigir conhecimento técnico, e há incentivo econômico direto (mais
  corridas).
- **Por que o impacto é 2 (moderado):** o dano é operacional — alocação injusta de corridas e
  possíveis atrasos — sem expor dados sensíveis nem gerar prejuízo financeiro direto à
  plataforma; é identificável e corrigível rapidamente.
- **Quem ou o que é afetado:** outros entregadores (perdem corridas justas) e clientes (atraso).
- **Consequências possíveis:** insatisfação de entregadores legítimos, necessidade de revisar o
  algoritmo de alocação de corridas.
- **Por que o nível Médio é adequado:** fácil de executar, mas o estrago é limitado e reversível,
  sem comprometer dados ou finanças.

### R06 — Comprometimento de conta via phishing

- **Por que a probabilidade é 3 (média-alta):** phishing é um vetor de baixo custo e baixo risco
  para o atacante, não depende de nenhuma vulnerabilidade técnica do SaborExpress — apenas de
  engenharia social sobre o cliente — e campanhas em massa são fáceis de disparar.
- **Por que o impacto é 4 (muito alto):** uma vez capturada a senha, o atacante tem acesso total
  à conta — endereço, histórico, pagamento salvo — idêntico ao R01, podendo afetar qualquer
  cliente-alvo, com dano irreversível de exposição de dados pessoais.
- **Quem ou o que é afetado:** qualquer cliente atingido pela campanha; a área de atendimento
  (volume de contestações).
- **Consequências possíveis:** pedidos fraudulentos, exposição de endereço residencial, dano
  reputacional à marca, possível notificação à ANPD conforme o volume afetado.
- **Por que o nível Crítico é adequado:** barreira de entrada baixíssima somada a um impacto
  máximo — mesmo raciocínio do R01, por um vetor diferente.

### R07 — Cadastro de restaurante fictício

- **Por que a probabilidade é 2 (média-baixa):** exige obter ou falsificar documentos de
  terceiros (CNPJ, alvará, contrato social) e passar pela homologação — mais atrito do que um
  ataque aberto ao público.
- **Por que o impacto é 3 (alto):** gera prejuízo financeiro direto aos clientes lesados e dano à
  confiança na plataforma, mas fica limitado ao raio de atuação daquele estabelecimento até ser
  identificado e suspenso.
- **Quem ou o que é afetado:** clientes que pedem no estabelecimento fictício; a reputação da
  marca.
- **Consequências possíveis:** reembolsos, reclamações públicas, necessidade de reforçar a
  homologação.
- **Por que o nível Médio é adequado:** a barreira de entrada reduz a probabilidade mesmo com um
  impacto relevante por ocorrência.

### R08 — Falsificação do webhook de pagamento

- **Por que a probabilidade é 2 (média-baixa):** exige conhecimento técnico específico para
  identificar o endpoint e forjar uma requisição válida sem assinatura — depende de uma falha
  pontual (ausência de validação de assinatura), não acessível ao usuário comum.
- **Por que o impacto é 4 (muito alto):** se explorado, libera pedidos sem pagamento real de
  forma repetível e automatizável, comprometendo a integridade financeira do modelo de negócio
  enquanto não for detectado.
- **Quem ou o que é afetado:** a receita da plataforma como um todo; os restaurantes, que
  produzem pedidos não pagos.
- **Consequências possíveis:** prejuízo financeiro crescente e sistemático, possível dano a
  restaurantes que produziram sem receber.
- **Por que o nível Alto é adequado:** a exigência técnica contém a probabilidade, mas o
  potencial de dano financeiro sistemático justifica ficar no patamar mais alto abaixo do
  crítico.

### R09 — Fábrica de contas para abuso de cupom

- **Por que a probabilidade é 3 (média-alta):** criar contas descartáveis é automatizável (e-mails
  temporários, scripts de cadastro) e não exige conhecimento técnico avançado — prática já
  observada com frequência em plataformas com cupons de primeira compra.
- **Por que o impacto é 3 (alto):** gera prejuízo financeiro recorrente e escalável às campanhas
  promocionais; o valor por fraude individual é pequeno, mas a escala é o problema.
- **Quem ou o que é afetado:** o orçamento de marketing/promoções da plataforma.
- **Consequências possíveis:** distorção de métricas de aquisição, prejuízo financeiro acumulado,
  necessidade de revisar a elegibilidade dos cupons.
- **Por que o nível Alto é adequado:** facilidade de execução somada ao potencial de escala
  justifica um nível alto mesmo sem afetar dados pessoais.

### R10 — Alteração de dados bancários de repasse

- **Por que a probabilidade é 2 (média-baixa):** exige acesso ao cadastro bancário do restaurante
  — via conta comprometida ou insider — uma condição mais restrita que ataques abertos ao
  público.
- **Por que o impacto é 3 (alto):** resulta em prejuízo financeiro direto ao restaurante lesado,
  mas afeta um parceiro por vez, não a base inteira.
- **Quem ou o que é afetado:** o restaurante lesado; a área financeira, que precisa investigar.
- **Consequências possíveis:** perda financeira do parceiro, dano à confiança de restaurantes na
  plataforma.
- **Por que o nível Médio é adequado:** a condição de acesso restrita equilibra o impacto
  financeiro relevante.

### R11 — Alteração de preço do cardápio por funcionário do restaurante

- **Por que a probabilidade é 3 (média-alta):** o autor já é um usuário legítimo com acesso ao
  painel — não há barreira técnica, apenas falta de controle de processo (aprovação, auditoria).
- **Por que o impacto é 2 (moderado):** o dano por ocorrência é limitado a cobranças divergentes
  em pedidos pontuais, geralmente identificável e reembolsável rapidamente.
- **Quem ou o que é afetado:** clientes que compraram durante a divergência de preço.
- **Consequências possíveis:** reclamações individuais, pequenos reembolsos, desgaste pontual de
  confiança no restaurante específico.
- **Por que o nível Médio é adequado:** alta probabilidade (acesso já legítimo) compensada por
  impacto limitado e de fácil correção.

### R12 — Manipulação de avaliações

- **Por que a probabilidade é 2 (média-baixa):** depende de uma falha específica de autorização
  na API de avaliações, não um caminho amplamente conhecido — exige descoberta técnica da falha.
- **Por que o impacto é 2 (moderado):** o dano é reputacional e indireto, sem prejuízo financeiro
  direto imediato nem exposição de dados pessoais.
- **Quem ou o que é afetado:** restaurantes cuja reputação é manipulada; clientes que decidem com
  base em avaliações falsas.
- **Consequências possíveis:** perda de clientes para um restaurante injustamente prejudicado, ou
  vantagem indevida para quem apaga críticas negativas.
- **Por que o nível Médio é adequado (na borda inferior):** tanto probabilidade quanto impacto são
  contidos, mas a integridade da reputação é um pilar de confiança do marketplace.

### R13 — Alteração de endereço de entrega após pagamento

- **Por que a probabilidade é 2 (média-baixa):** exige explorar uma falha pontual de validação
  (a API aceitar alteração após confirmação do pagamento), não um caminho aberto a qualquer
  usuário.
- **Por que o impacto é 3 (alto):** pode ser usado para redirecionar mercadoria paga (furto) ou
  causar entregas erradas, gerando prejuízo financeiro e operacional relevante.
- **Quem ou o que é afetado:** o cliente que teve o pedido desviado; o entregador, instrumentalizado
  sem saber.
- **Consequências possíveis:** furto de mercadoria, reembolso ao cliente lesado, repetição do
  golpe até ser identificado.
- **Por que o nível Médio é adequado:** a exigência de uma falha técnica específica equilibra a
  gravidade da consequência por ocorrência.

### R14 — Entregador nega ter deixado de entregar

- **Por que a probabilidade é 4 (alta):** não há nenhuma barreira — o entregador apenas marca
  "entregue" no aplicativo, sem exigência de prova alguma (foto, código, geolocalização),
  tornando o evento tão fácil quanto qualquer uso normal do app.
- **Por que o impacto é 3 (alto):** gera reembolso indevido e prejuízo financeiro recorrente à
  plataforma e ao restaurante, mas cada ocorrência é isolada e não expõe dados pessoais.
- **Quem ou o que é afetado:** a plataforma (arca com o reembolso), o restaurante (já preparou o
  pedido), o cliente legítimo (se o produto de fato não chegou e não há como provar).
- **Consequências possíveis:** prejuízo financeiro acumulado se o padrão se repetir,
  impossibilidade de responsabilizar quem está errado, desgaste com restaurantes.
- **Por que o nível Crítico é adequado:** ausência total de controle (probabilidade máxima)
  somada a um impacto financeiro recorrente justifica o nível mais alto, mesmo o dano por
  unidade não sendo catastrófico — a exposição está na repetição sem controle.

### R15 — Cliente alega falsamente não ter recebido o pedido

- **Por que a probabilidade é 4 (alta):** basta abrir um chamado alegando não recebimento —
  nenhuma barreira técnica, um padrão de fraude já documentado em plataformas de delivery reais
  ("item não recebido").
- **Por que o impacto é 3 (alto):** reembolso indevido recorrente, prejuízo financeiro tanto à
  plataforma quanto ao restaurante que já produziu o pedido.
- **Quem ou o que é afetado:** plataforma e restaurantes (absorvem o custo), o atendimento
  (sobrecarga de chamados).
- **Consequências possíveis:** prejuízo financeiro sistemático se não houver controle, incentivo a
  clientes reincidentes explorarem a falha.
- **Por que o nível Crítico é adequado:** mesmo raciocínio do R14 — facilidade extrema de execução
  multiplicada por impacto financeiro real e recorrente.

### R16 — Estorno sem rastreabilidade do responsável

- **Por que a probabilidade é 2 (média-baixa):** depende de um agente interno (atendente)
  disposto a abusar do próprio acesso — condição mais restrita do que uma fraude aberta ao
  público externo.
- **Por que o impacto é 3 (alto):** compromete a integridade financeira ao permitir estornos não
  rastreáveis, dificultando auditoria e podendo mascarar fraude interna continuada.
- **Quem ou o que é afetado:** a área financeira e de compliance; indiretamente, clientes cujas
  contas são usadas no estorno fraudulento.
- **Consequências possíveis:** dificuldade de investigar fraudes internas, prejuízo continuado não
  detectado por falta de rastreabilidade.
- **Por que o nível Médio é adequado:** a exigência de um agente interno malicioso reduz a
  probabilidade em relação às fraudes abertas ao público, mesmo com impacto financeiro relevante.

### R17 — Restaurante nega ter aceitado o pedido

- **Por que a probabilidade é 3 (média-alta):** negar o aceite é uma alegação simples e sem custo
  para o restaurante, plausível em situações comuns de atraso ou sobrecarga na cozinha.
- **Por que o impacto é 2 (moderado):** o dano é limitado ao atraso e à insatisfação do cliente
  naquele pedido específico, geralmente resolvido via reembolso pontual.
- **Quem ou o que é afetado:** o cliente que sofre o atraso; a relação comercial entre plataforma e
  restaurante.
- **Consequências possíveis:** desgaste na relação com o restaurante, dificuldade de aplicar
  penalidades justas sem prova do momento do aceite.
- **Por que o nível Médio é adequado:** fácil de alegar, mas o estrago por ocorrência é limitado e
  recuperável.

### R18 — Extração de dados pessoais pela API

- **Por que a probabilidade é 3 (média-alta):** a exploração de uma falha de autorização por objeto (IDOR) pode ser realizada diretamente pela API e não exige necessariamente conhecimento técnico avançado. Se os identificadores dos pedidos puderem ser obtidos ou enumerados, um atacante pode automatizar requisições para consultar pedidos de outros usuários. Não é 4 porque o ataque depende da existência da falha de autorização e da possibilidade de obter identificadores válidos.
- **Por que o impacto é 4 (muito alto):** a exploração permite acessar informações pessoais como endereço residencial e telefone de diversos clientes. Em escala, isso representa uma violação significativa de privacidade e pode gerar riscos físicos e consequências relacionadas à LGPD.
- **Quem ou o que é afetado:** clientes e seus dados pessoais, especialmente endereço e telefone, além da plataforma e sua área de atendimento.
- **Consequências possíveis:** vazamento em massa de dados pessoais, golpes, assédio, perseguição, reclamações de clientes, danos à reputação e possíveis consequências regulatórias.
- **Por que o nível Crítico é adequado:** a pontuação 12 resulta da combinação de probabilidade média-alta e impacto muito alto. Além disso, uma exploração automatizada pode atingir muitos clientes e os dados pessoais expostos não podem ser simplesmente recuperados após o vazamento.

### R19 — Exposição prolongada do endereço dos clientes

- **Por que a probabilidade é 3 (média-alta):** o problema ocorre sempre que um entregador continua tendo acesso ao endereço após a conclusão da entrega, caso não exista uma política de expiração ou remoção dessas informações. A exploração não exige técnicas sofisticadas, pois basta consultar os dados já disponibilizados ao entregador. Não é 4 porque o acesso depende de o indivíduo ter realizado uma entrega para aquele cliente.
- **Por que o impacto é 4 (muito alto):** endereços residenciais são dados pessoais que podem revelar diretamente o local onde uma pessoa mora. A exposição prolongada aumenta o risco de perseguição, assédio e outros usos indevidos.
- **Quem ou o que é afetado:** clientes e seus endereços residenciais, além da plataforma, que pode sofrer danos reputacionais e reclamações.
- **Consequências possíveis:** criação de bases de endereços, perseguição, assédio, invasão de privacidade e perda de confiança dos clientes na plataforma.
- **Por que o nível Crítico é adequado:** a pontuação 12 combina probabilidade média-alta com impacto muito alto. Embora o cenário dependa do acesso prévio do entregador, o potencial de dano decorrente da exposição de endereços residenciais é elevado.

### R20 — Exposição de backup do banco de dados

- **Por que a probabilidade é 2 (média-baixa):** backups normalmente ficam em ambientes separados e podem possuir controles de acesso próprios, reduzindo a facilidade de exploração. Entretanto, uma configuração inadequada de armazenamento ou permissões pode permitir que um atacante obtenha acesso ao arquivo. Não é 3 ou 4 porque o atacante precisa primeiro localizar e acessar o backup protegido.
- **Por que o impacto é 4 (muito alto):** um backup do banco pode concentrar grandes quantidades de dados de clientes, pedidos e outras informações da plataforma. A exposição de um único arquivo pode comprometer diversos ativos simultaneamente.
- **Quem ou o que é afetado:** banco de dados principal, dados pessoais dos clientes, credenciais e tokens e informações relacionadas aos pedidos.
- **Consequências possíveis:** vazamento em massa de dados, comprometimento de credenciais, fraude, violação da LGPD, prejuízos financeiros e danos reputacionais.
- **Por que o nível Alto é adequado:** a pontuação 8 resulta da combinação de probabilidade média-baixa com impacto muito alto. Apesar de a obtenção do backup depender de uma falha de controle de acesso, o impacto potencial é significativo porque um único backup pode concentrar informações de muitos usuários.

### R21 — Exposição de chave de API do provedor de mapas

- **Por que a probabilidade é 3 (média-alta):** uma chave incorporada diretamente em um aplicativo distribuído aos usuários pode ser obtida por análise do aplicativo ou observação das requisições realizadas. Não é necessário comprometer o backend para tentar reutilizar a chave. Não é 4 porque a exploração ainda depende de a chave possuir permissões ou restrições insuficientes.
- **Por que o impacto é 3 (alto):** o uso indevido da chave pode permitir consumo não autorizado dos serviços do provedor, gerar custos e atingir limites de utilização contratados pela plataforma. Dependendo das permissões associadas à chave, o impacto pode ser maior.
- **Quem ou o que é afetado:** chaves de API de serviços externos, orçamento da plataforma e o serviço de mapas utilizado pelo SaborExpress.
- **Consequências possíveis:** consumo indevido da API, aumento de custos, esgotamento de cotas, indisponibilidade do serviço de mapas e possível comprometimento de funcionalidades que dependem do provedor.
- **Por que o nível Alto é adequado:** a pontuação 9 combina probabilidade média-alta com impacto alto. A exposição de uma chave distribuída no aplicativo é uma condição plausível, e seu uso indevido pode gerar prejuízo financeiro e indisponibilidade de serviços dependentes da API.

### R22 — Exposição de dados sensíveis nos logs

- **Por que a probabilidade é 3 (média-alta):** logs são acessados frequentemente por equipes de desenvolvimento, suporte e operações. Se informações sensíveis forem registradas sem mascaramento e os controles de acesso forem inadequados, uma quantidade significativa de dados pode ficar disponível para pessoas que não precisam deles. Não é 4 porque o atacante ou usuário indevido ainda precisa obter acesso ao ambiente de logs.
- **Por que o impacto é 4 (muito alto):** logs podem conter tokens de autenticação, dados de pagamento ou outras informações sensíveis. A exposição desses dados pode permitir acesso indevido a contas e comprometer informações pessoais e financeiras.
- **Quem ou o que é afetado:** logs de auditoria, credenciais e tokens de sessão, dados de pagamento e clientes da plataforma.
- **Consequências possíveis:** comprometimento de contas, fraude financeira, exposição de dados pessoais, violação da LGPD e utilização dos dados registrados para novos ataques.
- **Por que o nível Crítico é adequado:** a pontuação 12 combina probabilidade média-alta com impacto muito alto. Além disso, logs podem acumular informações de muitos usuários durante longos períodos, aumentando a quantidade de dados potencialmente expostos.

### R23 — Enumeração de usuários pelo login

- **Por que a probabilidade é 4 (alta):** a técnica é simples e pode ser automatizada por meio de várias tentativas de login ou recuperação de senha. Se o sistema apresentar mensagens diferentes para e-mails cadastrados e não cadastrados, um atacante consegue identificar usuários sem precisar comprometer suas contas.
- **Por que o impacto é 2 (moderado):** descobrir que determinado e-mail possui uma conta não fornece, por si só, acesso à conta ou aos demais dados do usuário. Entretanto, essa informação pode ser utilizada como etapa inicial de phishing, engenharia social ou ataques direcionados.
- **Quem ou o que é afetado:** credenciais de acesso e os clientes que possuem contas cadastradas na plataforma.
- **Consequências possíveis:** criação de listas de usuários válidos, aumento da eficácia de campanhas de phishing, engenharia social e tentativas posteriores de comprometimento de contas.
- **Por que o nível Alto é adequado:** a pontuação 8 resulta da alta probabilidade de exploração combinada com impacto moderado. Embora a vulnerabilidade isoladamente não conceda acesso às contas, a facilidade de automação e seu potencial de servir como etapa para ataques posteriores justificam o nível Alto.

### R24 — Indisponibilidade da API de pedidos por ataque volumétrico

- **Por que a probabilidade é 3 (média-alta):** ataques volumétricos são uma técnica conhecida e podem ser realizados por meio de grandes volumes de requisições direcionadas à API. O risco aumenta durante os horários de pico, quando a infraestrutura já está próxima de sua capacidade. Não é 4 porque o ataque depende de recursos suficientes para gerar tráfego e de a infraestrutura não conseguir absorvê-lo ou filtrá-lo adequadamente.
- **Por que o impacto é 4 (muito alto):** a API de pedidos é um componente central do SaborExpress, sendo utilizada para operações essenciais da plataforma. Sua indisponibilidade pode impedir clientes de realizar pedidos, restaurantes de processá-los e entregadores de atualizarem as entregas.
- **Quem ou o que é afetado:** API de pedidos, clientes, restaurantes, entregadores e a operação financeira da plataforma.
- **Consequências possíveis:** interrupção das vendas durante períodos de maior faturamento, pedidos não processados, atrasos nas entregas, prejuízos aos restaurantes, perda de receita e danos à reputação do SaborExpress.
- **Por que o nível Crítico é adequado:** a pontuação 12 combina probabilidade média-alta com impacto muito alto. A indisponibilidade de um componente central durante horários de pico pode afetar simultaneamente grande quantidade de usuários e gerar prejuízo financeiro significativo.

### R25 — Pedidos falsos em massa contra um restaurante

- **Por que a probabilidade é 3 (média-alta):** a criação automatizada de pedidos pode ser realizada por meio de múltiplas contas ou requisições automatizadas caso não existam mecanismos eficazes de detecção e limitação. Não é 4 porque o ataque depende da capacidade de criar ou controlar contas e de contornar eventuais controles existentes na plataforma.
- **Por que o impacto é 3 (alto):** uma grande quantidade de pedidos falsos pode ocupar a capacidade de produção de um restaurante, consumir ingredientes e funcionários e impedir que pedidos legítimos sejam preparados dentro do prazo.
- **Quem ou o que é afetado:** restaurantes, clientes que realizam pedidos legítimos, API de pedidos e a operação de entrega.
- **Consequências possíveis:** desperdício de alimentos, sobrecarga da equipe do restaurante, atrasos, cancelamentos, perda de vendas legítimas e prejuízo financeiro ao estabelecimento.
- **Por que o nível Alto é adequado:** a pontuação 9 resulta da combinação de probabilidade média-alta com impacto alto. O ataque pode comprometer diretamente a capacidade operacional de um restaurante, embora seu impacto seja mais localizado do que uma indisponibilidade completa da plataforma.

### R26 — Abuso de aceites e cancelamentos de corridas

- **Por que a probabilidade é 3 (média-alta):** um entregador que consiga aceitar e cancelar corridas repetidamente pode realizar o comportamento abusivo sem necessidade de técnicas sofisticadas, caso não existam mecanismos de detecção e limitação. Não é 4 porque o ataque depende de uma conta de entregador e de a plataforma não identificar o padrão de comportamento.
- **Por que o impacto é 3 (alto):** o comportamento pode prejudicar a distribuição das entregas e reduzir a disponibilidade de entregadores para pedidos legítimos, aumentando o tempo de espera dos clientes.
- **Quem ou o que é afetado:** entregadores, clientes, restaurantes e o sistema de distribuição de corridas da plataforma.
- **Consequências possíveis:** aumento do tempo de entrega, pedidos cancelados, sobrecarga de outros entregadores, insatisfação dos clientes e perda de eficiência operacional.
- **Por que o nível Alto é adequado:** a pontuação 9 combina probabilidade média-alta com impacto alto. O problema pode afetar a disponibilidade do serviço de entrega, mas tende a possuir alcance mais limitado do que um ataque direto à API de pedidos.

### R27 — Abuso do envio de SMS de verificação

- **Por que a probabilidade é 4 (alta):** o envio repetido de códigos de verificação pode ser automatizado caso a plataforma não limite as solicitações por usuário, telefone ou dispositivo. A técnica não exige comprometimento de outros componentes do sistema e pode ser repetida em grande quantidade.
- **Por que o impacto é 2 (moderado):** o abuso pode consumir a cota contratada de mensagens e gerar custos para a plataforma, além de prejudicar temporariamente o recebimento de códigos por usuários legítimos. O impacto sobre os demais serviços da plataforma tende a ser limitado.
- **Quem ou o que é afetado:** serviço de notificações, orçamento da plataforma e usuários que dependem do SMS para autenticação ou recuperação de acesso.
- **Consequências possíveis:** aumento dos custos com SMS, esgotamento da cota contratada, atraso no recebimento de códigos e dificuldade para usuários legítimos concluírem processos de autenticação.
- **Por que o nível Alto é adequado:** a pontuação 8 resulta da alta probabilidade combinada com impacto moderado. A facilidade de automatização e a possibilidade de gerar custos repetidamente justificam o nível Alto.

### R28 — Sobrecarga por upload de arquivos excessivamente grandes

- **Por que a probabilidade é 3 (média-alta):** o envio de arquivos grandes pode ser automatizado caso a plataforma não estabeleça limites adequados de tamanho, quantidade e frequência. O ataque não exige necessariamente técnicas avançadas, mas depende de existir uma funcionalidade de upload acessível ao atacante.
- **Por que o impacto é 3 (alto):** arquivos excessivamente grandes podem consumir armazenamento, processamento e largura de banda, reduzindo a disponibilidade dos recursos para usuários legítimos.
- **Quem ou o que é afetado:** infraestrutura da plataforma, armazenamento de arquivos, serviços responsáveis pelo processamento dos uploads e usuários que utilizam essas funcionalidades.
- **Consequências possíveis:** consumo excessivo de armazenamento, degradação do desempenho, aumento de custos de infraestrutura, indisponibilidade parcial de funcionalidades e lentidão para outros usuários.
- **Por que o nível Alto é adequado:** a pontuação 9 combina probabilidade média-alta com impacto alto. A exploração pode ser automatizada e consumir recursos da plataforma, embora normalmente tenha alcance mais limitado do que um ataque volumétrico contra a API principal.

### R29 — Execução de operações administrativas por atendente de suporte

- **Por que a probabilidade é 3 (média-alta):** a exploração pode ocorrer diretamente por meio da API caso a autorização seja aplicada somente na interface do sistema. Um atendente com uma conta legítima pode tentar acessar endpoints administrativos diretamente, sem precisar comprometer outra conta. Não é 4 porque o atacante precisa possuir uma conta de suporte e conhecer ou descobrir os endpoints disponíveis.
- **Por que o impacto é 4 (muito alto):** a exploração pode permitir que um atendente execute operações acima das suas permissões, como emitir estornos, alterar comissões e acessar dados de usuários. Essas ações podem gerar prejuízos financeiros e comprometer informações de diversos clientes.
- **Quem ou o que é afetado:** painel administrativo, registros financeiros, dados dos usuários e a própria plataforma.
- **Consequências possíveis:** estornos fraudulentos, alteração indevida de comissões, acesso não autorizado a dados de clientes, prejuízos financeiros e perda de confiança na plataforma.
- **Por que o nível Crítico é adequado:** a pontuação 12 combina probabilidade média-alta com impacto muito alto. Uma única conta de suporte comprometida ou utilizada de forma indevida pode permitir operações administrativas com impacto sobre diversos usuários e sobre a situação financeira da plataforma.

### R30 — Obtenção indevida de privilégios de restaurante

- **Por que a probabilidade é 2 (média-baixa):** a exploração depende de o backend aceitar diretamente um campo de perfil enviado pelo cliente durante o cadastro ou alteração da conta. Caso essa falha exista, a exploração pode ser simples, mas o risco é reduzido pela necessidade de existir uma implementação vulnerável desse mecanismo. Não é 3 ou 4 porque depende de uma falha específica no processo de atribuição de privilégios.
- **Por que o impacto é 4 (muito alto):** um cliente que consiga obter privilégios de restaurante pode acessar funcionalidades destinadas a estabelecimentos, podendo alterar informações de cardápio, preços e pedidos. Dependendo das permissões concedidas, isso pode causar prejuízos financeiros e comprometer a integridade das operações.
- **Quem ou o que é afetado:** API de pedidos, restaurantes, clientes e informações relacionadas aos pedidos e cardápios.
- **Consequências possíveis:** alteração indevida de preços e cardápios, manipulação de pedidos, fraude, prejuízos aos restaurantes e comprometimento da confiança na plataforma.
- **Por que o nível Alto é adequado:** a pontuação 8 combina probabilidade média-baixa com impacto muito alto. Embora a exploração dependa de uma falha específica na atribuição de perfis, a obtenção de privilégios de restaurante pode permitir ações significativamente superiores às permitidas para um cliente.

### R31 — Falsificação de privilégios por alteração de JWT

- **Por que a probabilidade é 2 (média-baixa):** a exploração depende de uma falha na validação da assinatura do JWT pelo backend. Se essa condição existir, um atacante pode tentar modificar as informações de autorização contidas no token. Não é 3 ou 4 porque a exploração depende de uma implementação específica e de o backend aceitar tokens cuja autenticidade não tenha sido validada corretamente.
- **Por que o impacto é 4 (muito alto):** um token adulterado que seja aceito pelo backend pode permitir que um atacante assuma um papel com privilégios superiores aos seus. Isso pode possibilitar acesso a funcionalidades administrativas ou operações destinadas a outros perfis.
- **Quem ou o que é afetado:** credenciais e tokens de sessão, API de pedidos, painel administrativo e dados dos usuários.
- **Consequências possíveis:** acesso não autorizado a funcionalidades restritas, alteração de dados, fraude financeira, acesso a informações de outros usuários e comprometimento de contas com privilégios elevados.
- **Por que o nível Alto é adequado:** a pontuação 8 combina probabilidade média-baixa com impacto muito alto. A falha não é necessariamente fácil de encontrar ou explorar, mas, caso exista e seja explorada, pode permitir uma elevação significativa de privilégios.

### R32 — Acesso de funcionário a pedidos de outra loja

- **Por que a probabilidade é 3 (média-alta):** um funcionário de restaurante já possui uma conta legítima e acesso à API utilizada para gerenciar pedidos. Se o backend não verificar corretamente o vínculo entre o funcionário, o restaurante e o pedido solicitado, o usuário pode tentar acessar identificadores pertencentes a outra loja. Não é 4 porque a exploração depende da existência da falha de isolamento entre restaurantes.
- **Por que o impacto é 3 (alto):** o acesso indevido pode permitir consultar ou modificar pedidos de outros estabelecimentos. Isso compromete a separação entre restaurantes e pode causar alterações indevidas nas operações de terceiros.
- **Quem ou o que é afetado:** API de pedidos, pedidos dos restaurantes, restaurantes participantes da plataforma e clientes relacionados aos pedidos.
- **Consequências possíveis:** exposição de informações de pedidos, alteração ou cancelamento indevido de pedidos, prejuízos aos restaurantes, problemas nas entregas e perda de confiança na plataforma.
- **Por que o nível Alto é adequado:** a pontuação 9 combina probabilidade média-alta com impacto alto. A exploração pode ser realizada por um usuário que já possui acesso legítimo à plataforma, mas o impacto tende a ficar limitado aos pedidos e estabelecimentos alcançados pela falha de autorização.
---

## 11. Priorização

Esta seção apresenta a ordenação estratégica para o tratamento dos riscos identificados no ecossistema do SaborExpress. Como a modelagem matemática tradicional baseada no NIST SP 800-30 resultou em múltiplos riscos empatados com a pontuação máxima (**Criticidade 12**), foi necessária a aplicação de uma metodologia qualitativa de triagem secundária, baseada em princípios consagrados de engenharia de software seguro, na severidade jurídica sob a égide da LGPD e no potencial de danos físicos e operacionais diretos.

### 11.1 Metodologia de Priorização e Critérios de Desempate

<!-- RESPONSÁVEL: Fernando -->
<!-- TODO: ordenar os riscos e JUSTIFICAR por que um deve ser tratado antes do outro.
     A ordem não pode ser apenas a pontuação decrescente — o enunciado pede que se considere
     também gravidade das consequências, número de usuários afetados, importância do ativo,
     possibilidade de recuperação, dependências entre riscos e urgência. -->

Para desempatar e ordenar os riscos críticos (pontuação 12), foram adotados quatro critérios científicos hierárquicos:

1. **Critério A: Irreversibilidade do Dano e Impacto à Privacidade (LGPD):** Incidentes que resultam na exfiltração em massa de dados cadastrais (especialmente a correlação entre nome, telefone e endereço residencial) são irreversíveis — dados uma vez vazados não podem ser recuperados ou "desvazados". Sob a LGPD, acarretam sanções civis graves e multas pesadas, além de expor clientes a riscos reais de violência física ou perseguição devido à entrega de alimentos em suas residências.
2. **Critério B: Dependência Sistêmica e Efeito Cascata (Vetor de Entrada):** Vulnerabilidades que atuam como porta de entrada para o comprometimento de outros ativos críticos do sistema ou que franqueiam privilégios administrativos (como a quebra de autenticação, sequestro de contas e falhas no painel administrativo de suporte) devem ser mitigadas prioritariamente. A blindagem das credenciais protege indiretamente as transações e as informações corporativas.
3. **Critério C: Disponibilidade Sistêmica e Integridade do Modelo de Negócio:** Interrupções completas das operações de backend por ataques volumétricos (DDoS) ou fraudes transacionais sistemáticas na validação de pagamentos afetam o faturamento da empresa no horário de pico, comprometendo a subsistência financeira dos restaurantes parceiros.
4. **Critério D: Perdas Financeiras e Confiabilidade da Entrega (*Last-Mile*):** Riscos associados a fraudes pontuais de repúdio ou desvios de conduta no ato de entrega. Embora críticos na contabilidade final, tratam-se de perdas transacionais que ocorrem de forma isolada (pedido a pedido) e que admitem processos administrativos secundários para contenção e ressarcimento.


### 11.2 Tabela de Priorização dos Riscos (Top 15)

A tabela abaixo estabelece a ordem prioritária de tratamento de riscos, cruzando as pontuações quantitativas com os critérios de triagem qualitativa detalhados anteriormente:

| Ordem | ID | Origem STRIDE | Evento de Risco | Pont. | Nível | Foco do Tratamento / Justificativa Estratégica |
| :---: | :---: | :--- | :--- | :---: | :--- | :--- |
| **1º** | **R03** | T18 — Info Disclosure | Vazamento em massa de dados de clientes (endereço, telefone) via IDOR na API de pedidos | 12 | Crítico | **Privacidade e LGPD:** Vazamento em larga escala, irreversível e com alto potencial de responsabilização jurídica imediata. |
| **2º** | **R19** | T19 — Info Disclosure | Endereços residenciais de clientes permanecem acessíveis a entregadores após a conclusão das entregas | 12 | Crítico | **Segurança Física:** Risco contínuo e silencioso de assédio ou perseguição física contra clientes utilizando dados expostos. |
| **3º** | **R22** | T22 — Info Disclosure | Dados sensíveis de pagamento ou tokens de autenticação gravados nos logs de aplicação | 12 | Crítico | **Vetor Interno Passivo:** Exposição passiva de credenciais e dados financeiros a desenvolvedores e operadores, alimentando fraudes em massa. |
| **4º** | **R29** | T29 — Elevation of Priv. | Atendente de suporte executa operações administrativas abusivas (ex.: estornos ilimitados) por falta de validação no backend | 12 | Crítico | **Efeito Cascata Administrativo:** Controle de acesso ao painel de backoffice, que é o ativo mais crítico do sistema de controle. |
| **5º** | **R01** | T01 — Spoofing | Sequestro em massa de contas de clientes via *credential stuffing* por falta de MFA e restrição de login | 12 | Crítico | **Vetor de Entrada Ativo:** Ataque automatizável de alta frequência que serve como ponte para fraudes transacionais e roubo de dados. |
| **6º** | **R06** | T04 — Spoofing | Comprometimento massivo de contas de clientes por campanhas direcionadas de phishing no fluxo de senha | 12 | Crítico | **Proteção de Identidade:** Ataque direto contra a identidade digital do cliente, mitigado pelas mesmas barreiras robustas do R01. |
| **7º** | **R24** | T24 — Denial of Service | Indisponibilidade total da API de pedidos no horário de pico por ataque volumétrico (DDoS) | 12 | Crítico | **Continuidade de Negócio:** Interrupção catastrófica de vendas e danos imediatos à reputação e saúde dos lojistas parceiros. |
| **8º** | **R14** | T13 — Repudiation | Entregador simula falsamente a conclusão de entrega sem realizá-la, gerando ônus financeiro | 12 | Crítico | **Fraude Operacional (*Last-Mile*):** Fraude direta no faturamento, de alta frequência, dependente de novos processos físicos de verificação. |
| **9º** | **R15** | T14 — Repudiation | Cliente contesta falsamente o recebimento de pedidos para obter reembolsos indevidos de forma sistemática | 12 | Crítico | **Abuso do Consumidor:** Fraude transacional repetitiva que corrói as margens financeiras e exige conciliação probatória robusta. |
| **10º** | **R08** | T06 — Spoofing | Forjamento de webhook de pagamento aprovado, gerando liberação de pedidos sem lastro financeiro | 8 | Alto | **Integridade Transacional:** Risco financeiro severo (liberação automática de mercadorias grátis), contido pela facilidade técnica de mitigação. |
| **11º** | **R20** | T20 — Info Disclosure | Exfiltração de backup do banco de dados por exposição ou configuração inadequada | 8 | Alto | **Proteção de Infraestrutura:** Concentração máxima de dados históricos, protegida por controles de segurança em nível de nuvem e criptografia em repouso. |
| **12º** | **R31** | T31 — Elevation of Priv. | Modificação e adulteração da assinatura de tokens JWT para assumir privilégios superiores de restaurante/admin | 8 | Alto | **Segurança de Protocolo:** Vulnerabilidade técnica grave na camada de autenticação, cujo tratamento de backend blinda APIs inteiras. |
| **13º** | **R04** | T02 — Spoofing | Uso de contas de entregador alugadas ou compradas por terceiros não verificados | 9 | Alto | **Confiabilidade da Entrega:** Fraude de mercado comum que gera riscos de segurança física localizados na entrega física. |
| **14º** | **R21** | T21 — Info Disclosure | Extração da chave de API de mapas integrada diretamente no código do aplicativo móvel | 9 | Alto | **Perda Financeira Indireta:** Abuso de cota e custos imprevistos com o provedor de mapas, mitigável por restrições na console do parceiro. |
| **15º** | **R02** | T07 — Tampering | Alteração do valor do carrinho/pedido interceptando requisições antes do pagamento | 6 | Médio | **Consistência de Dados:** Fraude financeira isolada com barreira técnica média, facilmente corrigível com recálculo estrito no servidor. |

### 11.3 Justificativa Detalhada da Ordem de Tratamento

### Bloco 1 (1º ao 4º lugar): Blindagem de Dados de Clientes e Risco Físico (R03, R19, R22, R29)
*   **Fundamentação Científica:** No ecossistema do SaborExpress, o maior ativo a ser protegido são os dados residenciais e telefônicos dos clientes. O vazamento massivo por IDOR (**R03**) lidera a priorização por ser uma ameaça cibernética silenciosa e totalmente automatizável, cujos impactos legais (LGPD) e danos à reputação da plataforma são irreparáveis. 
*   Em seguida, a exposição prolongada de endereços no aplicativo móvel dos entregadores (**R19**) representa uma ameaça de alto risco para a segurança física de pessoas no mundo real, com potencial de evoluir para assédio ou perseguição física. 
*   A inclusão da gravação de dados sensíveis nos logs (**R22**) em 3º lugar se justifica porque logs desprotegidos expõem tokens e dados bancários de forma passiva para múltiplos agentes internos de desenvolvimento sem autorização formal. 
*   O risco **R29** (abuso administrativo por atendentes de suporte) fecha este primeiro bloco de atenção imediata por representar o vetor interno de maior impacto lateral nas finanças e dados de toda a base corporativa.

### Bloco 2 (5º ao 6º lugar): Defesa de Identidade e Controle de Acesso Ativo (R01, R06)
*   **Fundamentação Científica:** O comprometimento ativo de contas através de ataques de força bruta ou *credential stuffing* (**R01**) e campanhas de phishing focado em recuperação de senha (**R06**) representam a principal porta de entrada técnica externa para o roubo de dados individuais. 
*   Uma vez tomada a conta, o atacante obtém acesso automático aos meios de pagamento salvos e aos dados cadastrais da vítima. Mitigar estes riscos reduz drasticamente a superfície de ataque inicial do SaborExpress.

### Bloco 3 (7º lugar): Disponibilidade Operacional e Faturamento (R24)
*   **Fundamentação Científica:** A indisponibilidade da API de pedidos (**R24**) por ataques distribuídos de negação de serviço (DDoS) paralisa integralmente o modelo operacional de delivery em horários de maior tráfego. Sem a API, o SaborExpress é incapaz de processar transações, gerando perda imediata de faturamento para si e para os restaurantes parceiros, inviabilizando temporariamente a operação do negócio.

### Bloco 4 (8º ao 9º lugar): Confiabilidade Operacional na Entrega e Combate ao Repúdio (R14, R15)
*   **Fundamentação Científica:** As fraudes do último quilômetro de entrega (*last-mile*) — onde entregadores fingem entregar (**R14**) ou clientes alegam falsamente não terem recebido os pedidos (**R15**) para fraudar reembolsos — ocupam as posições 8º e 9º. Embora classificados como críticos pela pontuação matemática (Criticidade 12), estes riscos tratam de problemas de processo físico e operacional pulverizados. Suas consequências financeiras são restritas a transações individuais (pedido por pedido) e, diferentemente dos vazamentos de dados, admitem auditoria de geolocalização e estornos compensatórios a posteriori para remediação.

### Bloco 5 (10º ao 15º lugar): Controles Técnicos, Infraestrutura e Mitigação de Fraudes de Escala (R08, R20, R31, R04, R21, R02)
*   **Fundamentação Científica:** Este bloco agrupa riscos com pontuações de criticidade "Alta" e "Média". A mitigação do forjamento de webhook de pagamento aprovado (**R08**) surge em 10º lugar devido à necessidade urgente de garantir o fluxo correto de liquidação financeira da plataforma. 
*   O risco de exfiltração de backup (**R20**) e adulteração de JWT (**R31**) situam-se nos postos seguintes como defesas de infraestrutura profunda e sanitização de código de backend. 
*   Por fim, o aluguel de contas de entregador (**R04**), a extração de chaves de API (**R21**) e a alteração local de preços de pedidos (**R02**) completam o roteiro por apresentarem maior dificuldade de exploração automatizada ou menor severidade direta de dano sistêmico.

<!--
| Ordem | Risco | Pontuação | Nível | Motivo de estar nesta posição |
|---|---|---|---|---|
| 1º | R03 | 12 | Crítico | Mesma pontuação de R01, mas colocado à frente porque o vazamento em massa é irreversível: dados exfiltrados não podem ser recuperados, enquanto uma conta tomada pode ser bloqueada e as transações estornadas |
| 2º | R01 | 12 | Crítico | <TODOS> |
| 3º | — | — | — | <TODOS> |
-->
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

Conforme as diretrizes científicas do NIST CSF 2.0, as funções não devem ser marcadas automaticamente. Cada atribuição nesta matriz reflete uma necessidade técnica e operacional explícita na modelagem de segurança da plataforma SaborExpress, onde:
*   **Govern (GO)** incide quando o tratamento do risco exige a definição formal de políticas de conformidade, governança de identidade ou normas contratuais de parceiros.
*   **Identify (ID)** aplica-se quando o risco exige o mapeamento dinâmico de novos ativos, verificação cadastral contra bases governamentais externas ou classificação ativa de dados.
*   **Protect (PR)** é a base tecnológica de controle ativo (middlewares, criptografia, autenticação multifator, filtros de requisição) para impedir ou mitigar a ocorrência do risco na camada de aplicação.
*   **Detect (DE)** envolve o monitoramento e a geração de alarmes em tempo real para comportamentos ou requisições suspeitas baseadas em assinaturas ou anomalias operacionais.
*   **Respond (RS)** refere-se à capacidade de conter incidentes ativos na infraestrutura ou no negócio, aplicando regras de contenção automática (ex: rate-limit dinâmico ou bloqueio temporário).
*   **Recover (RC)** destina-se a processos de recuperação de estado e continuidade (ex: comunicação legal à ANPD, restauração de estados de dados íntegros, reversão de estornos de cartões).

<!-- TODO(Murillo): analisar cada relação. NÃO marcar todas as funções automaticamente — o
     enunciado adverte contra isso. Marque apenas quando houver um resultado de segurança
     concreto a ser alcançado naquela função, e explique as escolhas menos óbvias na coluna final. -->

#### Tabela de Mapeamento Completo de Riscos vs. Funções NIST CSF 2.0

| ID | Origem STRIDE | Evento de Risco | GO | ID | PR | DE | RS | RC | Observação Metodológica e Justificativa Científica |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **R01** | T01 — Spoofing | Tomada de contas de clientes (*Credential Stuffing*) | **X** | | **X** | **X** | **X** | **X** | **GO** obriga MFA para logins suspeitos. **PR** bloqueia força bruta por limite de requisições. **DE** sinaliza volumetria anômala. **RS** bloqueia o IP atacante. **RC** gera reset de senha e fluxo de auto-recuperação. |
| **R02** | T07 — Tampering | Alteração de valores de pedidos interceptando requisições | | | **X** | **X** | | | **PR** implementa middleware de recálculo estrito no servidor (API) ignorando valores do app móvel. **DE** emite logs comparativos automatizados de discrepâncias de carrinho. |
| **R03** | T18 — Info Disclosure | Vazamento em massa de dados via IDOR na API de pedidos | | | **X** | **X** | **X** | **X** | **PR** impõe controle de acesso contextual a nível de objeto e *rate limit*. **DE** monitora acessos horizontais atípicos. **RS** invalida sessões suspeitas. **RC** gerencia a notificação da ANPD sob a LGPD. |
| **R04** | T02 — Spoofing | Contas de entregadores compradas ou alugadas | | **X** | **X** | **X** | **X** | | **ID** requer checagem cadastral e selfies aleatórias contra CNH. **PR** exige autenticação periódica por dispositivo. **DE** cruza telemetria de velocidade. **RS** suspende a conta preventivamente. |
| **R05** | T03 — Spoofing | Entregador simulando localização via GPS falso (*Mock Location*) | | | **X** | **X** | **X** | | **PR** consome APIs nativas de geolocalização blindadas contra mocks. **DE** analisa incoerências de tempo/espaço (física de transporte). **RS** remove o entregador da corrida. |
| **R06** | T04 — Spoofing | Comprometimento de conta via campanha direcionada de Phishing | **X** | | **X** | **X** | **X** | | **GO** dita políticas de segurança e autenticação robustas. **PR** implementa MFA e DMARC/SPF/DKIM de e-mails oficiais. **DE** vigia logins geográficos impossíveis. **RS** encerra sessões ativas. |
| **R07** | T05 — Spoofing | Cadastro de restaurante fantasma para fraudar clientes | | **X** | **X** | **X** | | | **ID** valida CNPJ na base da Receita Federal. **PR** força carência financeira e teto transacional nos primeiros 15 dias. **DE** monitora pico súbito de saques. |
| **R08** | T06 — Spoofing | Falsificação de webhook para pagamento falso sem lastro real | | | **X** | **X** | | | **PR** exige validação criptográfica (HMAC-SHA256) na recepção do webhook do gateway de pagamento . **DE** loga assinaturas inválidas e ips fora de whitelist. |
| **R09** | T08 — Tampering | Criação massiva de contas descartáveis para abuso de cupons | | | **X** | **X** | | | **PR** restringe resgate por CPF único de pagamento, telefone validado por SMS e fingerprint de dispositivo. **DE** identifica registros duplicados a partir do mesmo IP. |
| **R10** | T09 — Tampering | Alteração de dados bancários de restaurantes para desvio de repasse | | | **X** | **X** | **X** | | **PR** impõe carência de 48h pós-modificação e token MFA. **DE** loga auditoria detalhada de alterações sensíveis. **RS** dispara notificações em múltiplos canais de alerta. |
| **R11** | T10 — Tampering | Divergência e flutuação de preços em cardápios durante o pedido | | | **X** | **X** | | | **PR** congela e assina digitalmente o preço no carrinho do checkout. **DE** audita o histórico imutável das mudanças de preço e emite relatórios de desvio operacional. |
| **R12** | T11 — Tampering | Manipulação ilícita ou apagamento indevido de avaliações | | | **X** | **X** | | **X** | **PR** garante autorização contextual de autoria em nível de API. **DE** captura anomalias de deleção em massa. **RC** recupera registros avaliativos usando soft-delete. |
| **R13** | T12 — Tampering | Alteração de endereço de entrega pós-pagamento confirmado | | | **X** | **X** | | | **PR** bloqueia alteração automática de CEP no backend após o recebimento financeiro. **DE** loga chamadas não-autorizadas e tentativas de reencaminhamento. |
| **R14** | T13 — Repudiation | Entregador marca pedido como entregue falsamente (*Last-mile*) | | | **X** | **X** | **X** | | **PR** exige digitação do código fornecido pelo cliente e valida geolocalização no ato. **DE** sinaliza divergência de raio de entrega. **RS** retém saldo da corrida para averiguação. |
| **R15** | T14 — Repudiation | Cliente alega falsamente não recebimento para obter estorno | | | **X** | **X** | **X** | | **PR** utiliza as evidências colhidas no R14 (código do cliente, geolocalização, foto opcional). **DE** analisa comportamento histórico do cliente. **RS** instaura disputa operacional. |
| **R16** | T15 — Repudiation | Emissão de estorno indevido por atendente de backoffice | **X** | | **X** | **X** | | | **GO** divide privilégios operacionais e teto financeiro por papel. **PR** vincula obrigatoriamente logs de estorno a credenciais nominativas de SSO. **DE** audita desvios. |
| **R17** | T16 — Repudiation | Restaurante rejeita ou nega aceite operacional do pedido | | | **X** | **X** | | | **PR** gera recibo digital com timestamp e hashes imutáveis das transações de aceite de fluxo. **DE** cria dashboard de SLAs de aceitação para mitigar disputas com lojistas. |
| **R19** | T19 — Info Disclosure | Dados residenciais expostos nos celulares de entregadores pós-entrega | | | **X** | **X** | | | **PR** expira e remove completamente informações sensíveis (endereço, telefone) do app do entregador pós-finalização. **DE** audita requisições à cache offline de pedidos encerrados. |
| **R20** | T20 — Info Disclosure | Exposição de backups do banco de dados por má configuração | **X** | **X** | **X** | **X** | | | **GO** rege políticas formais de ciclo de vida de cópias e chaves criptográficas. **ID** mapeia servidores e buckets de storage. **PR** ativa criptografia de ponta em repouso. **DE** loga e alarma downloads atípicos. |
| **R21** | T21 — Info Disclosure | Chave do provedor de mapas extraída por engenharia reversa no app | | **X** | **X** | **X** | | | **ID** cataloga tokens e dependências ativas no código. **PR** restringe a chave na console externa por pacote de app móvel e domínio. **DE** monitora picos de cota e faturamento no console de mapas. |
| **R22** | T22 — Info Disclosure | Logs da aplicação expondo dados de cartões ou tokens de sessão | **X** | | **X** | **X** | | | **GO** prescreve padrões e políticas regulatórias de desenvolvimento seguro (OWASP Top 10 / PCI-DSS). **PR** limpa e sanitiza strings no middleware de log. **DE** monitora padrões de vazamento de chaves via SAST. |
| **R23** | T23 — Info Disclosure | Enumeração automatizada de endereços de e-mail pelo login | | | **X** | **X** | | | **PR** retorna respostas genéricas de erro no login e ativa CAPTCHAs dinâmicos. **DE** alerta logins sucessivos de emails inexistentes de uma mesma origem IP. |
| **R24** | T24 — Denial of Service | Indisponibilidade da API de pedidos em horário de pico por DDoS | | | **X** | **X** | **X** | **X** | **PR** emprega WAF corporativo e CDN. **DE** sinaliza anomalia volumétrica. **RS** escala infraestrutura horizontalmente e ativa scrubbers de tráfego. **RC** restabelece planos de continuidade operacional. |
| **R25** | T25 — Denial of Service | Sobrecarga de restaurante com pedidos automatizados falsos | | | **X** | **X** | **X** | | **PR** força análise antifraude comportamental e CAPTCHA de checkout. **DE** vigia surtos súbitos de pedidos em restaurantes de uma mesma zona. **RS** alerta e suspende novas entradas temporariamente. |
| **R26** | T26 — Denial of Service | Grupo de entregadores manipulando corridas por aceites/cancelamentos | | | **X** | **X** | **X** | | **PR** estabelece cotas máximas e intervalos progressivos para desistências no dia. **DE** mapeia geolocalização sincronizada de cancelamentos. **RS** suspende o entregador do dia. |
| **R27** | T27 — Denial of Service | Consumo de cota financeira abusando do SMS de autenticação | | | **X** | **X** | | | **PR** limita o disparo de mensagens para o mesmo celular/IP (rate-limit cumulativo). **DE** reporta consumo incomum nas filas de envio do gateway de telecom. |
| **R28** | T28 — Denial of Service | Esgotamento de armazenamento por upload de imagens gigantes | | | **X** | **X** | | | **PR** aplica redimensionamento compulsório e teto de tamanho de arquivo (ex: máximo 5MB) na entrada de dados. **DE** sinaliza taxa atípica de crescimento de disco. |
| **R29** | T29 — Elevation of Priv. | Atendente chamando endpoints sem validação backend (*BOLA/IDOR*) | **X** | | **X** | **X** | | | **GO** cria matriz formal de controle de permissão corporativa baseada em papéis. **PR** valida privilégios criptográficos no backend em cada request administrativo. **DE** audita o console do backoffice. |
| **R30** | T30 — Elevation of Priv. | Cliente obtém permissões de lojista alterando campos do formulário | | | **X** | **X** | | | **PR** valida todas as rotas e tipos de privilégio na camada de dados do backend. **DE** sinaliza se contas com e-mails civis/comuns passam a executar ações administrativas de restaurantes. |
| **R31** | T31 — Elevation of Priv. | Escalonamento vertical de privilégios adulterando o token JWT | | | **X** | **X** | | | **PR** valida chaves secretas e assinaturas criptográficas de tokens em servidores sem estado local. **DE** loga tentativas de bypass com assinaturas corrompidas ou nulas. |
| **R32** | T32 — Elevation of Priv. | Restaurante espionando ou alterando pedidos de outro restaurante | | | **X** | **X** | | | **PR** aplica controle de isolamento multi-tenant (vínculo do ID do restaurante com o ID do pedido no banco de dados). **DE** monitora requisições de lojistas a recursos de lojas alheias. |
| **R18** | *T18 — Info Disclosure* | *Consolidado no R03 (Risco IDOR de API de Pedidos duplicado)* | *—* | *—* | *—* | *—* | *—* | *—* | *Risco duplicado retirado para manter mapeamento 1-para-1 com as 31 ameaças do STRIDE de forma consistente em todas as seções.* |

#### Análise Metodológica do Mapeamento

1.  **Predomínio da Função Protect (PR):** Conforme esperado para um ecossistema transacional baseado em APIs web e aplicativos móveis, a função **Protect** obteve adesão em **100% dos riscos reais ativos (31 de 31)**. Isso reflete o fato de que, sob o prisma acadêmico da engenharia de software seguro, a prevenção por blindagem ativa de código (como criptografia de logs, middlewares BOLA/IDOR, HMAC em webhooks e rate-limiting) constitui a barreira de defesa fundamental contra a exploração de vulnerabilidades.
2.  **Uso Seletivo de Govern (GO) e Identify (ID):** Para evitar a trivialização do NIST CSF, a função **Govern** foi demarcada de forma restrita somente para cenários onde as decisões de segurança demandam a definição formal de políticas sistêmicas abrangentes (como regras corporativas de MFA, classificação de dados para logs de auditoria e segregação de deveres no backoffice administrativo). Semelhantemente, **Identify** foi aplicada exclusivamente nos riscos que exigem processos ativos de mapeamento de terceiros (homologação de CNH/selfies de entregadores e consulta cadastral corporativa na Receita Federal).
3.  **Resiliência via Detect, Respond e Recover:** Os riscos críticos prioritários como sequestro de contas (**R01**), DDoS (**R24**), IDOR massivo (**R03**) e fraudes do último quilômetro (**R14** e **R15**) dependem de um ciclo integrado de defesa em profundidade. A sua inserção nas colunas **Detect**, **Respond** e **Recover** garante que o SaborExpress não se apoie apenas em controles estáticos de barreira, mas conte com rotinas ativas para registrar incidentes operacionais, bloquear os IPs atacantes e recuperar a integridade regulatória e financeira perante o mercado e os reguladores da ANPD.

<!--
| Risco | Govern | Identify | Protect | Detect | Respond | Recover | Observação |
|---|:---:|:---:|:---:|:---:|:---:|:---:|---|
| R01 | X | | X | X | X | X | *Govern* entra porque é preciso uma política que torne o MFA obrigatório; *Identify* não foi marcado porque o ativo e a vulnerabilidade já estão plenamente mapeados e não há trabalho adicional de descoberta |
| R02 *(Tampering)* | | | X | X | |  | *Protect* é vital para implementar a verificação no lado do servidor, impedindo que requisições adulteradas sejam aceitas. *Detect* entra para registrar logs de discrepâncias entre os valores enviados pelo aplicativo e o catálogo real de preços. |
| R03 *(Inf. Disclosure)* | | | **X** | **X** | **X** | **X** | *Protect* trata de impedir o acesso não autorizado IDOR e de aplicar *rate limit*. *Detect* monitora picos de chamadas na API. *Respond* é necessário para revogar tokens suspeitos. *Recover* é crucial para restaurar credenciais, comunicar o vazamento à ANPD e reestabelecer a privacidade dos dados. |
-->
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
| R02 — Manipulação do valor do pedido | Reduzir | (1) Servidor recalcula o valor total do pedido a partir do catálogo de preços vigente, ignorando qualquer valor enviado pelo app; (2) requisição de checkout validada com hash server-side sobre itens e preços; (3) log de auditoria comparando valor enviado × valor calculado, sinalizando divergências | Protect, Detect | Time de Backend | Teste automatizado enviando payload de checkout adulterado, confirmando que o valor cobrado é sempre o do catálogo; auditoria de logs comprovando 100% dos pedidos recalculados no servidor |
| R03 | <!-- TODO --> | | | | |
| R04 — Conta de entregador alugada ou comprada | Reduzir | (1) Verificação periódica de identidade (selfie comparada à CNH) em checagens aleatórias; (2) análise de padrão de dispositivo/localização para detectar múltiplos usuários numa mesma conta; (3) suspensão automática temporária quando detectada anomalia, até nova verificação | Identify, Protect, Detect, Respond | Time de Cadastro/Verificação de Parceiros + Segurança | Relatório de execuções da verificação periódica; teste simulando troca de dispositivo/localização incomum confirmando o bloqueio; taxa semanal de contas suspensas |
| R05 — GPS falso do entregador | Reduzir | (1) Detecção de *mock location* via API nativa do sistema operacional, recusando a corrida quando identificado; (2) checagem de deslocamento fisicamente impossível entre atualizações de localização; (3) suspensão temporária progressiva para reincidência confirmada | Protect, Detect, Respond | Time Mobile (app do entregador) | Teste manual com app de GPS falso confirmando rejeição da corrida; log de eventos de detecção de *mock location* |
| R06 — Comprometimento de conta via phishing | Reduzir | (1) MFA obrigatório (mesmo controle do R01, mesma vulnerabilidade de fundo); (2) configuração de SPF/DKIM/DMARC nos e-mails oficiais para dificultar falsificação de remetente; (3) aviso fixo no fluxo de recuperação de senha alertando que o link oficial só vem do domínio da plataforma; (4) exigência de segunda confirmação para login de dispositivo/local incomum | Govern, Protect, Detect, Respond | Segurança da Informação + Comunicação | Auditoria de configuração SPF/DKIM/DMARC; simulação interna de phishing medindo taxa de cliques; log de bloqueios por local incomum |
| R07 — Cadastro de restaurante fictício | Reduzir | (1) Validação cruzada do CNPJ na Receita Federal via API pública no momento do cadastro; (2) confirmação de que a conta bancária de repasse pertence ao mesmo CNPJ homologado; (3) limite de valor/pedidos diários nos primeiros 15 dias, até validação manual completa | Identify, Protect | Time de Cadastro/Compliance de Parceiros | Log de consultas à Receita Federal por cadastro; relatório de restaurantes suspensos no período de observação por inconsistência |
| R08 — Falsificação do webhook de pagamento | Reduzir | (1) Validação obrigatória da assinatura HMAC do gateway em toda requisição recebida, rejeitando o que não tiver assinatura válida; (2) *whitelist* de IPs do gateway no endpoint; (3) idempotência por identificador de transação, evitando reprocessamento duplicado | Protect, Detect | Time de Backend/Integrações de Pagamento | Teste automatizado enviando requisição sem assinatura válida, confirmando rejeição HTTP 401/403; revisão da configuração de *whitelist* de IP |
| R09 — Fábrica de contas para abuso de cupom | Reduzir | (1) Cupom de primeira compra vinculado a CPF e/ou meio de pagamento único, não só ao e-mail; (2) verificação de telefone obrigatória antes da liberação do cupom; (3) limite de cupons por *fingerprint* de dispositivo numa janela de tempo | Protect, Detect | Time de Growth/Marketing + Backend | Teste criando contas com mesmo CPF/cartão confirmando bloqueio do segundo cupom; relatório de cupons resgatados por dispositivo |
| R10 — Alteração de dados bancários de repasse | Reduzir | (1) Alteração de dados bancários exige confirmação por e-mail **e** SMS ao titular, com carência de 48h antes de valer para o próximo repasse; (2) log de auditoria de toda alteração, com IP e usuário responsável; (3) notificação automática ao restaurante a cada alteração | Protect, Detect, Respond | Time Financeiro/Backend | Teste de alteração de dados bancários confirmando o período de carência e o disparo das notificações; amostragem de logs |
| R11 — Alteração de preço do cardápio | Reduzir | (1) Alteração de preço em item já publicado só vale a partir do próximo pedido, nunca durante um carrinho já aberto; (2) histórico de preços por item, auditável; (3) alerta automático para variações acima de um limiar percentual em curto intervalo | Protect, Detect | Time de Backend (painel do restaurante) | Teste de alteração de preço durante carrinho aberto confirmando que o valor do pedido em andamento não muda; relatório de histórico de preços |
| R12 — Manipulação de avaliações | Reduzir | (1) Autorização por objeto na API de avaliações — só o autor original ou a moderação (com log) pode editar/excluir; (2) log de toda alteração/exclusão identificando quem executou; (3) restauração automática de avaliação removida fora do fluxo oficial | Protect, Detect, Recover | Time de Backend | Teste tentando editar avaliação de terceiro e confirmando rejeição (403/404); auditoria de exclusões comparando com o autor autenticado |
| R13 — Alteração de endereço de entrega após pagamento | Reduzir | (1) Bloqueio de alteração de endereço após confirmação de pagamento — exige cancelar e criar novo pedido; (2) se permitida antes da coleta, exige reautenticação e notifica entregador e restaurante da mudança; (3) log de toda tentativa de alteração pós-pagamento | Protect, Detect | Time de Backend | Teste de chamada à API tentando alterar endereço pós-pagamento, validando rejeição; log de tentativas bloqueadas |
| R14 — Entregador marca entrega sem entregar | Reduzir | (1) Confirmação de entrega exige código informado pelo cliente **e** geolocalização do entregador registrada no momento; (2) foto obrigatória quando o cliente não está presente; (3) retenção temporária do repasse daquela entrega até a evidência ser validada | Protect, Detect, Respond | Time Mobile (app do entregador) + Operações | Teste confirmando que o app não permite marcar "entregue" sem código, foto e geolocalização; auditoria por amostragem de entregas |
| R15 — Cliente nega recebimento do pedido | Reduzir | (1) Usa a mesma evidência de entrega do R14 (código + geo + foto) como prova para contestar pedidos de reembolso; (2) reembolso por "não recebimento" passa por checagem cruzada com a evidência antes de aprovação automática; (3) sinalização de clientes com padrão recorrente de contestação para revisão manual | Protect, Detect, Respond | Time de Atendimento + Backend | Teste do fluxo de reembolso confirmando que pedidos com evidência completa não são reembolsados automaticamente sem análise; relatório de reincidência por cliente |
| R16 — Estorno sem rastreabilidade do atendente | Reduzir | (1) Todo estorno fica vinculado obrigatoriamente ao ID do atendente autenticado — sem opção de estorno "anônimo"; (2) segunda aprovação obrigatória para estornos acima de um valor-limite; (3) relatório periódico de estornos por atendente | Govern, Protect, Detect | Time de Backoffice/Compliance | Teste tentando emitir estorno sem autenticação individual e confirmando rejeição; auditoria mensal de estornos por atendente |
| R17 — Restaurante nega aceite do pedido | Reduzir | (1) Registro imutável com timestamp do momento exato do aceite, incluindo IP/dispositivo do painel; (2) notificação automática ao cliente e à plataforma no instante do aceite, criando trilha independente; (3) métrica de tempo médio de aceite por restaurante para identificar padrão de negativa recorrente | Protect, Detect | Time de Backend (painel do restaurante) | Teste confirmando geração do registro de aceite com timestamp íntegro; relatório de tempo médio de aceite por restaurante |
| R18 — Extração de dados pessoais pela API | Reduzir | (1) Validar no backend se o pedido consultado pertence ao usuário autenticado antes de retornar qualquer dado; (2) utilizar autorização por objeto em todos os endpoints de consulta de pedidos; (3) limitar requisições de consulta por usuário/IP para dificultar enumeração automatizada; (4) registrar tentativas de acesso a pedidos pertencentes a outros usuários | Protect, Detect | Time de Backend + Segurança da Informação | Teste tentando consultar pedido pertencente a outro usuário, confirmando rejeição HTTP 403/404; teste de múltiplas consultas sequenciais verificando aplicação do *rate limit*; auditoria dos logs de tentativas de acesso indevido |
| R19 — Exposição prolongada do endereço dos clientes | Reduzir | (1) Remover ou ocultar o endereço completo do aplicativo do entregador após a conclusão da entrega; (2) permitir acesso ao endereço somente enquanto a entrega estiver ativa; (3) registrar acessos ao endereço após a finalização para identificar tentativas indevidas | Protect, Detect | Time Mobile + Backend + Segurança da Informação | Teste concluindo uma entrega e verificando que o endereço completo deixa de estar disponível; teste de chamada à API após a conclusão confirmando rejeição; auditoria dos registros de acesso |
| R20 — Exposição de backup do banco de dados | Reduzir | (1) Armazenar backups em repositório privado sem acesso público; (2) restringir o acesso aos backups somente às contas de infraestrutura autorizadas; (3) criptografar os arquivos de backup; (4) registrar e revisar acessos aos arquivos de backup | Protect, Detect | Time de Infraestrutura + Segurança da Informação | Teste tentando acessar o armazenamento sem credenciais autorizadas, confirmando rejeição; verificação da configuração de acesso privado; teste de restauração de backup criptografado; auditoria dos logs de acesso |
| R21 — Exposição de chave de API do provedor de mapas | Reduzir | (1) Remover chaves privilegiadas do aplicativo móvel; (2) manter as credenciais do provedor somente no backend; (3) restringir cada chave por serviço, origem e limite de utilização quando suportado pelo provedor; (4) substituir imediatamente uma chave identificada como exposta | Protect, Detect, Respond | Time de Backend + Mobile + Segurança da Informação | Análise do aplicativo confirmando ausência de chaves privilegiadas; revisão das restrições configuradas no provedor; teste utilizando uma chave exposta para confirmar que ela não possui acesso indevido; registro de rotação de credenciais |
| R22 — Exposição de dados sensíveis nos logs | Reduzir | (1) Mascarar tokens, dados de pagamento e outras informações sensíveis antes da gravação dos logs; (2) impedir que tokens completos sejam registrados; (3) restringir acesso ao serviço de logs por função; (4) criar alerta para detectar padrões de dados sensíveis nos logs | Protect, Detect | Time de Backend + Segurança da Informação | Teste executando operações de pagamento e autenticação e verificando que os valores sensíveis não aparecem nos logs; revisão das permissões do serviço de logs; relatório de alertas de dados sensíveis |
| R23 — Enumeração de usuários pelo login | Reduzir | (1) Utilizar a mesma mensagem de resposta para e-mails cadastrados e não cadastrados; (2) aplicar limite de tentativas no login e recuperação de senha; (3) registrar tentativas repetidas de enumeração; (4) bloquear temporariamente origens que apresentem comportamento automatizado | Protect, Detect | Time de Backend + Segurança da Informação | Teste comparando respostas para e-mail existente e inexistente, confirmando que não há diferença observável; teste de múltiplas tentativas confirmando aplicação do *rate limit*; análise dos logs de tentativas |
| R24 — Indisponibilidade da API de pedidos por ataque volumétrico | Reduzir | (1) Aplicar *rate limiting* por IP, usuário e origem na API de pedidos; (2) utilizar proteção contra tráfego volumétrico antes da API; (3) definir limites de requisições para endpoints críticos; (4) criar alerta para aumento anormal de tráfego | Protect, Detect, Respond | Time de Infraestrutura + Backend + Segurança da Informação | Teste de carga controlado verificando aplicação dos limites; relatório de tráfego bloqueado; teste de alerta para aumento anormal de requisições; registro de disponibilidade da API durante o teste |
| R25 — Pedidos falsos em massa contra um restaurante | Reduzir | (1) Limitar a quantidade de pedidos criados por conta, dispositivo e meio de pagamento em determinado período; (2) detectar múltiplos pedidos direcionados ao mesmo restaurante em curto intervalo; (3) exigir validação adicional quando o comportamento for identificado como automatizado; (4) bloquear temporariamente contas que ultrapassem os limites definidos | Protect, Detect, Respond | Time de Backend + Operações | Teste criando pedidos acima do limite em ambiente de homologação; relatório de pedidos bloqueados; teste com múltiplas contas verificando detecção do padrão; auditoria das contas temporariamente bloqueadas |
| R26 — Abuso de aceites e cancelamentos de corridas | Reduzir | (1) Definir limite de cancelamentos realizados por entregador em uma janela de tempo; (2) identificar padrões anormais de aceite seguido de cancelamento; (3) aplicar suspensão temporária após reincidência; (4) registrar todos os eventos de aceite e cancelamento para análise | Protect, Detect, Respond | Time de Backend + Operações de Entregadores | Teste realizando múltiplos aceites e cancelamentos e verificando aplicação do limite; relatório de entregadores sinalizados; auditoria dos eventos registrados |
| R27 — Abuso do envio de SMS de verificação | Reduzir | (1) Limitar solicitações de SMS por telefone, conta, dispositivo e endereço IP; (2) aplicar intervalo mínimo entre solicitações sucessivas; (3) bloquear temporariamente origens que ultrapassem o limite; (4) registrar o volume de mensagens enviadas por origem | Protect, Detect | Time de Backend + Infraestrutura | Teste realizando solicitações consecutivas e confirmando aplicação do intervalo e limite; relatório de SMS bloqueados; monitoramento do volume por telefone, dispositivo e IP |
| R28 — Sobrecarga por upload de arquivos excessivamente grandes | Reduzir | (1) Definir tamanho máximo para cada tipo de arquivo aceito; (2) limitar quantidade de uploads por usuário em uma janela de tempo; (3) rejeitar arquivos que ultrapassem os limites antes do processamento; (4) registrar tentativas de upload acima do limite | Protect, Detect | Time de Backend + Infraestrutura | Teste enviando arquivos acima do tamanho permitido e confirmando rejeição; teste de múltiplos uploads verificando aplicação do limite; relatório de tentativas bloqueadas |
| R29 — Execução de operações administrativas por atendente de suporte | Reduzir | (1) Validar no backend o perfil e as permissões do atendente em cada endpoint administrativo; (2) separar permissões de suporte das permissões administrativas; (3) exigir segunda aprovação para operações financeiras acima de determinado limite; (4) registrar toda operação administrativa com o usuário responsável | Protect, Detect | Time de Backend + Backoffice + Segurança da Informação | Teste tentando executar endpoint administrativo com conta de suporte, confirmando rejeição; teste de operação financeira acima do limite; auditoria dos logs de operações administrativas |
| R30 — Obtenção indevida de privilégios de restaurante | Reduzir | (1) Ignorar campos de perfil enviados pelo cliente durante o cadastro; (2) permitir alteração de perfil somente por fluxo administrativo autorizado; (3) validar no backend o papel atribuído ao usuário antes de cada operação restrita; (4) registrar alterações de perfil | Protect, Detect | Time de Backend + Cadastro | Teste enviando requisição de cadastro com perfil de restaurante e confirmando que o privilégio não é concedido; teste de tentativa de alteração de perfil pela API; auditoria dos registros de alteração |
| R31 — Falsificação de privilégios por alteração de JWT | Reduzir | (1) Validar obrigatoriamente a assinatura do JWT no backend; (2) rejeitar tokens com assinatura inválida ou algoritmo não permitido; (3) validar expiração e emissor do token; (4) não confiar em informações de privilégio fornecidas sem validação criptográfica | Protect, Detect | Time de Backend + Segurança da Informação | Teste enviando JWT com assinatura alterada e confirmando rejeição; teste com token expirado; teste com algoritmo não permitido; revisão automatizada da validação dos tokens |
| R32 — Acesso de funcionário a pedidos de outra loja | Reduzir | (1) Validar no backend o vínculo entre funcionário, restaurante e pedido em toda consulta ou alteração; (2) impedir que o identificador do pedido seja suficiente para conceder acesso; (3) registrar tentativas de acesso a pedidos de outros restaurantes; (4) bloquear ou sinalizar contas que apresentem tentativas repetidas | Protect, Detect, Respond | Time de Backend + Segurança da Informação | Teste utilizando funcionário de uma loja para consultar pedido de outra, confirmando rejeição; teste de alteração de pedido de terceiro; auditoria dos logs de tentativas de acesso indevido |


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
