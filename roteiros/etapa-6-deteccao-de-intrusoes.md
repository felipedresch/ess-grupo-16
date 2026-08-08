# Etapa 6 — Monitoramento e Detecção de Intrusões

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 5 — Verificação de Vulnerabilidades](../docs/etapa-5-verificacao-vulnerabilidades.md)
**Última atualização:** <!-- atualize a data ao editar --> 08/08/2026

<!-- RESPONSÁVEL: Felipe -->

> Este é um **roteiro descritivo**. O enunciado é explícito: não é necessário instalar nem
> implementar um sistema de detecção de intrusões.

---

## 1. O que é detecção de intrusões

Detecção de intrusões é observar o sistema enquanto ele funciona, procurando comportamento que
indique um ataque em andamento ou já ocorrido. O material dessa observação é aquilo que o próprio
sistema produz enquanto opera: tentativas de login, requisições à API, erros devolvidos, operações
administrativas e alterações de cadastro.

No SaborExpress o ponto de partida é reconhecer que um cliente pedindo comida e um atacante
tentando entrar em contas alheias chegam ao mesmo endereço, pelo mesmo aplicativo, usando a mesma
API. Requisição por requisição, as duas são praticamente idênticas. A diferença aparece no
conjunto: um cliente legítimo erra a senha duas ou três vezes, enquanto um ataque de
*credential stuffing* produz centenas de falhas em minutos, contra contas diferentes, a partir do
mesmo endereço de rede. Detectar é conseguir ler esse conjunto.

Vale separar dois conceitos que costumam ser confundidos. Registrar não é detectar. O log é a
matéria-prima, e sozinho ele só acumula linhas que ninguém lê. A detecção acontece quando existem
regras aplicadas sobre esse registro, capazes de dizer que determinada combinação de eventos é
suspeita e merece uma resposta.

---

## 2. Prevenir e detectar não são a mesma coisa

A prevenção age antes, tentando impedir que o ataque funcione. É o caso do segundo fator no login,
da verificação de autorização no servidor e do recálculo do valor do pedido sem confiar no que o
aplicativo enviou. A detecção age durante ou depois, percebendo que algo errado está acontecendo
ou já aconteceu. Ela não impede a tentativa, mas faz com que ela não passe despercebida.

O grupo entende que as duas são necessárias, por três motivos que aparecem na própria análise
feita nas etapas anteriores.

O primeiro é que todo controle pode falhar. Ele pode ser mal configurado, pode ser desativado por
engano numa alteração de código, ou pode ter uma exceção que ninguém previu. Sem detecção, essa
falha só é descoberta pelo prejuízo que causa.

O segundo é que a superfície de ataque muda com o tempo. Vulnerabilidades novas aparecem no código
e nas dependências depois que o sistema já está no ar, e nenhuma decisão tomada na Etapa 3 protege
contra uma falha que ainda não existia quando a arquitetura foi desenhada.

O terceiro é o mais importante no caso de uma plataforma de delivery. Boa parte dos abusos que o
grupo levantou é cometida por usuários legítimos, usando permissões que eles de fato possuem. O
entregador de T13 tem permissão para marcar o pedido como entregue. O cliente de T14 tem direito de
abrir chamado de reembolso. O atendente de T29 e o entregador de T19 estão acessando dados que o
trabalho deles exige. Não existe prevenção capaz de bloquear essas ações sem inviabilizar o
serviço, porque a ação em si é permitida. O que diferencia o uso normal do abuso é a frequência, o
padrão e o contexto, que só o registro e a análise revelam.

Isso explica por que, no mapeamento do NIST CSF feito na Etapa 2, vários riscos foram associados
às funções **Detect** e **Respond** e não apenas a **Protect**. Há ainda um ganho específico para
as ameaças de Repudiation: sem registro íntegro do que aconteceu, a plataforma não consegue provar
nada e qualquer disputa vira palavra contra palavra.

---

## 3. Eventos que o sistema deveria registrar

A lista abaixo foi derivada das ameaças da Etapa 1. O critério de escolha foi simples: registrar
aquilo que, olhado em conjunto, permite distinguir uso normal de abuso, e aquilo que precisa
existir como prova quando houver disputa.

| Evento registrado | Por que registrar | Ameaças relacionadas |
|---|---|---|
| Tentativa de login, com sucesso ou falha, guardando conta alvo, IP e identificação do dispositivo | É a base para separar o erro de senha de um cliente de um ataque automatizado contra muitas contas | T01, T04 |
| Autenticação a partir de dispositivo ou local não reconhecido | Primeiro sinal visível de que alguém entrou em uma conta que não é sua | T01, T02 |
| Alteração de e-mail, telefone, senha ou meio de pagamento | É o passo seguinte típico de quem toma uma conta, porque garante o acesso e corta o aviso ao dono | T01 |
| Requisição negada por falta de permissão, separando "não existe" de "não é seu" | Enumeração de identificadores e tentativa de escalonamento aparecem como sequências de negativas | T18, T29, T30, T32 |
| Operação administrativa: estorno, criação de cupom, homologação, suspensão e alteração de comissão | Sem o registro de qual conta executou e com qual justificativa, nenhuma investigação interna chega ao responsável | T15, T29 |
| Alteração de dados bancários de repasse do restaurante ou do entregador | Mudança silenciosa desses dados desvia dinheiro real e costuma ser percebida só no fechamento | T09 |
| Confirmação de coleta e de entrega, com data, hora e localização do entregador no momento | É a evidência que resolve a disputa entre quem diz que entregou e quem diz que não recebeu | T13, T14, T16 |
| Aplicação de cupom, com conta, dispositivo, endereço e meio de pagamento associados | Abuso de cupom de primeira compra só fica visível quando se cruzam contas diferentes com o mesmo dispositivo ou cartão | T08 |
| Alteração de preço no cardápio e do valor final do pedido, guardando o valor anterior | Permite reconstruir divergências entre o que foi anunciado e o que foi cobrado | T07, T10 |
| Criação, edição e exclusão de avaliação | Manipulação de reputação em massa só é detectável com o histórico das alterações | T11 |
| Alteração do endereço de entrega depois do pagamento confirmado | Operação legítima, porém rara, e usada em golpe de redirecionamento de mercadoria | T12 |
| Volume de requisições por IP, por conta e por endpoint, em janelas curtas | Base comum para detectar negação de serviço, raspagem de dados e enumeração | T18, T24, T26 |
| Envio de SMS ou e-mail de verificação, com número alvo e origem da solicitação | Cada mensagem tem custo, e o abuso desse mecanismo aparece como repetição contra muitos números | T27 |
| Recebimento de webhook do gateway, com resultado da validação da assinatura | Uma confirmação de pagamento falsa só é identificável se a validação for registrada, e não apenas executada | T06 |
| Acesso a documentos de entregadores e restaurantes, e a endereços fora de corrida ativa | Consulta a esses dados fora do fluxo de trabalho é o indício de coleta indevida | T19, T20 |

### 3.1 O que não deve entrar no log

O registro também é um ativo, catalogado como **A08**, e vaza como qualquer outro. A ameaça
**T22** descreve exatamente esse cenário: logs que gravam dados sensíveis acabam expondo a quem
tem acesso ao ambiente de operação aquilo que o sistema protege em todos os outros lugares.

Por isso, nunca devem ser gravados:

- senhas, mesmo que erradas, e respostas de recuperação de conta;
- tokens de sessão, tokens de recuperação e segredos de MFA;
- dados de cartão, incluindo o número completo e o código de segurança;
- chaves de API dos serviços externos, que são o ativo A13;
- CPF e documentos completos, quando os últimos dígitos bastarem para identificar o registro.

A regra prática é registrar **quem fez, o que fez, quando e de onde**, sem copiar para o log o
conteúdo protegido em si. O acesso aos registros também precisa ser restrito e, ele próprio,
registrado.

---

## 4. Regras de detecção

<!-- TODO(Felipe): TRÊS regras. O enunciado diz que três bastam.
     Cada regra precisa dos quatro campos. A primeira está preenchida como modelo.
     Importante: cada regra deve apontar para um risco ou caso de abuso REAL das etapas
     anteriores — é isso que o professor avalia como "relação com os riscos do projeto". -->

### Regra 1 — Tentativas de login em massa

| Campo | Conteúdo |
|---|---|
| **Risco observado** | R01 — tomada de contas de clientes (origem: T01, Spoofing) |
| **Fonte de dados** | Logs de autenticação da API |
| **Condição de alerta** | Mais de 20 falhas de login vindas do mesmo IP em 5 minutos, **ou** falhas contra mais de 10 contas distintas a partir do mesmo IP na mesma janela |
| **Resposta inicial** | Bloquear temporariamente o IP, exigir segundo fator para as contas alvo no próximo login e notificar a equipe de segurança |

> **Por que duas condições:** a primeira pega força bruta contra uma conta; a segunda pega
> *credential stuffing*, em que o atacante tenta uma senha em milhares de contas diferentes e
> quase não gera falhas repetidas na mesma conta. Só a primeira condição deixaria passar
> justamente o ataque mais provável.

### Regra 2 — <!-- TODO(Felipe): título -->

| Campo | Conteúdo |
|---|---|
| **Risco observado** | |
| **Fonte de dados** | |
| **Condição de alerta** | |
| **Resposta inicial** | |

### Regra 3 — <!-- TODO(Felipe): título -->

| Campo | Conteúdo |
|---|---|
| **Risco observado** | |
| **Fonte de dados** | |
| **Condição de alerta** | |
| **Resposta inicial** | |

<!-- TODO(Felipe): sugestões para as regras 2 e 3, escolher as que casarem com os riscos que
     ficaram no topo da priorização —
     - Raspagem de dados: uma mesma conta consulta mais de N pedidos distintos em X minutos, ou
       recebe muitos 404 de autorização em sequência (indica enumeração de IDs → R03).
     - Entregador que marca entrega sem entregar: confirmação de entrega registrada a mais de
       300 metros do endereço de destino (→ T13, repúdio).
     - Abuso de cupom: mais de N contas novas usando cupom de primeira compra no mesmo
       dispositivo, cartão ou endereço em 24h (→ T08).
     - Escalonamento de privilégio: conta de suporte chamando endpoint administrativo fora da
       sua alçada (→ T29).
     - Pico anômalo de requisições em horário de pico (→ T24, DoS). -->

---

## 5. O que acontece depois de um alerta

<!-- TODO(Felipe): descrever o fluxo em 5 a 8 passos. Quem recebe o alerta, o que faz primeiro,
     como decide se é incidente real ou falso positivo, quando escala, quem comunica os usuários
     afetados, quando a ANPD precisa ser notificada (LGPD), e como o aprendizado volta para as
     regras.
     Amarre com as funções Respond e Recover do NIST que o Fernando mapeou na Etapa 2 — é o mesmo
     raciocínio, agora em nível operacional. -->

---

## 6. Limitações

<!-- TODO(Felipe): registrar que as regras não foram implementadas nem testadas, que os limiares
     (20 falhas, 5 minutos, 300 metros) são estimativas iniciais que precisariam ser calibradas
     com dados reais de uso, e que regra mal calibrada gera ruído — alerta demais faz a equipe
     parar de olhar, o que é pior do que não ter alerta. -->

---

**Continua em:** [Etapa 7 — DevSecOps e Vídeo Final](etapa-7-devsecops-e-video-final.md)
