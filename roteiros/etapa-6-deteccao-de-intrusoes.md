# Etapa 6 — Monitoramento e Detecção de Intrusões

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 5 — Verificação de Vulnerabilidades](../docs/etapa-5-verificacao-vulnerabilidades.md)
**Última atualização:** <!-- atualize a data ao editar --> 12/08/2026

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

O grupo definiu três regras. A escolha seguiu a priorização de riscos da Etapa 2 e um segundo
critério: cobrir fontes de dados diferentes, para que a detecção não dependa de um único ponto de
instrumentação. A regra 1 lê os logs de autenticação, a regra 2 os logs de autorização da API e a
regra 3 os registros operacionais de entrega.

Os valores numéricos das condições (quantidades, distâncias e janelas de tempo) são estimativas
iniciais. A seção 6 trata do que isso implica.

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

### Regra 2 — Extração em massa de dados pela API

| Campo | Conteúdo |
|---|---|
| **Risco observado** | R03, primeiro colocado na priorização da Etapa 2. Vazamento de dados pessoais de clientes pela API de pedidos (origem: T18, Information Disclosure) |
| **Fonte de dados** | Logs de autorização da API de pedidos, com a conta autenticada, o identificador solicitado e o resultado da verificação de propriedade |
| **Condição de alerta** | Qualquer uma das três: uma conta consulta mais de 30 identificadores de pedido distintos em 10 minutos; uma conta recebe mais de 20 negativas de autorização em 5 minutos; a proporção entre pedidos consultados e pedidos que a conta realmente possui passa de 3 para 1 em uma janela de 24 horas |
| **Resposta inicial** | Encerrar a sessão e invalidar o token da conta, exigir reautenticação com segundo fator, aplicar limite de requisições agressivo àquela conta e ao IP, e acionar a equipe de segurança para medir quantos registros foram efetivamente lidos |

> **Por que três condições:** a primeira pega quem enumera identificadores sequenciais e acerta
> alvos válidos. A segunda pega quem erra, gerando negativas em série. A terceira existe para o
> caso de o atacante ir devagar o bastante para ficar abaixo das duas primeiras, situação em que a
> desproporção entre o que ele consulta e o que ele possui continua visível. Esta é a regra mais
> importante das três, porque o dano do R03 é irreversível: dado exfiltrado não volta atrás, e a
> LGPD ainda impõe o dever de notificar.

### Regra 3 — Entrega confirmada longe do endereço de destino

| Campo | Conteúdo |
|---|---|
| **Risco observado** | R14 e R15, oitavo e nono na priorização. O entregador marca como entregue sem entregar, ou o cliente nega ter recebido (origem: T13 e T14, Repudiation) |
| **Fonte de dados** | Registro de confirmação de entrega, com data, hora e coordenadas do entregador no momento, comparado ao endereço do pedido |
| **Condição de alerta** | Confirmação registrada a mais de 300 metros do endereço de destino, ou sem leitura de localização disponível, ou com intervalo menor que 2 minutos entre a coleta no restaurante e a confirmação. O alerta sobe de prioridade quando o mesmo entregador acumula três ocorrências em 7 dias |
| **Resposta inicial** | Reter o repasse daquela corrida até a apuração, notificar o cliente pedindo confirmação, e abrir disputa com os dados registrados. Na reincidência, suspender o entregador preventivamente e revisar as entregas anteriores dele |

> **Por que esta regra cobre dois riscos:** o R14 e o R15 são o mesmo evento visto de lados
> opostos, e a evidência que resolve um resolve o outro. Registrar onde e quando a entrega foi
> confirmada permite tanto responsabilizar o entregador que não entregou quanto recusar o
> reembolso de quem recebeu e alegou o contrário. É também o exemplo mais claro do argumento da
> seção 2: as duas ações são executadas por usuários legítimos, dentro das permissões deles, e
> nenhuma prevenção conseguiria bloqueá-las sem inviabilizar a operação.

**Riscos de alta prioridade não cobertos por estas três regras.** O R19, o R22 e o R29 estão acima
do R14 na priorização e não geraram regra própria. O R19 e o R22 são tratados de forma preventiva,
por expiração do endereço no aplicativo e por sanitização dos logs, e a verificação deles cabe
melhor a uma auditoria periódica do que a um alerta em tempo real. O R29 é parcialmente coberto
pela segunda condição da regra 2, já que a chamada de um endpoint administrativo fora da alçada
também produz negativa de autorização registrada. Em uma próxima rodada, valeria uma regra
dedicada ao backoffice.

---

## 5. O que acontece depois de um alerta

Um alerta que ninguém trata tem o mesmo valor prático de um alerta que nunca disparou. O fluxo
abaixo descreve o caminho entre a detecção e o encerramento do incidente. Ele corresponde, no
nível operacional, às funções **Respond** e **Recover** que o grupo mapeou na Etapa 2.

1. **Recebimento.** O alerta chega ao canal da equipe de plantão com o mínimo necessário para
   agir: qual regra disparou, qual conta, IP ou entregador está envolvido, a janela de tempo e o
   trecho do registro que motivou o disparo. Alerta sem contexto obriga a investigação a começar
   do zero e atrasa a contenção.

2. **Triagem.** A pessoa de plantão classifica o alerta em incidente real, falso positivo ou
   ruído conhecido. Essa decisão tem prazo, porque um alerta parado na fila é indistinguível de um
   alerta ignorado. O grupo adota 15 minutos para as regras 1 e 2, que tratam de dados e contas, e
   até 2 horas para a regra 3, cujo dano é financeiro e não se agrava na mesma velocidade.

3. **Contenção.** Confirmado o incidente, aplica-se de imediato a resposta inicial prevista na
   própria regra, como invalidar sessões, limitar requisições ou reter o repasse. A contenção vem
   antes de entender o caso por completo, porque cada minuto a mais é mais dado lido ou mais
   dinheiro perdido.

4. **Escalonamento.** Se o incidente envolve dados pessoais, dinheiro ou mais de um usuário, ele
   deixa de ser tratado apenas pelo plantão. São acionados a pessoa responsável pelo componente
   afetado, a liderança técnica e, nos casos de dado pessoal, o encarregado de proteção de dados.

5. **Investigação.** Determinar o alcance: quantos usuários foram afetados, quais dados foram
   acessados, desde quando aquilo estava acontecendo e por qual falha entrou. Essa etapa depende
   inteiramente da qualidade do registro descrito na seção 3.

6. **Comunicação.** Os usuários afetados são avisados do que aconteceu e do que precisam fazer,
   como trocar a senha ou conferir a fatura. Quando há exposição de dados pessoais com risco
   relevante às pessoas, a LGPD obriga a comunicação à ANPD e aos titulares, e a decisão sobre
   isso é do jurídico com o encarregado, não do plantão.

7. **Recuperação.** Restaurar o estado íntegro: revogar credenciais e tokens comprometidos,
   estornar transações fraudulentas, reverter alterações indevidas e reabilitar o que foi
   suspenso preventivamente e se mostrou legítimo.

8. **Aprendizado.** Todo incidente encerrado gera duas saídas. A primeira é o ajuste da própria
   detecção, calibrando limiares, criando regra nova ou removendo a que só gerou ruído. A segunda
   é devolver o caso à modelagem de ameaças: o que aconteceu em produção vira ameaça conhecida na
   próxima rodada de STRIDE. É o fechamento do ciclo descrito no pipeline da
   [Etapa 7](etapa-7-devsecops-e-video-final.md).

---

## 6. Limitações

O grupo registra as seguintes limitações desta etapa.

**Nada foi implementado nem testado.** As três regras são propostas em papel. A eficácia real
delas só poderia ser afirmada depois de implantadas, com medição de quantos incidentes reais
foram pegos e quantos passaram.

**Os limiares são chutes informados.** Os números escolhidos, como 20 falhas de login, 30
identificadores em 10 minutos e 300 metros de distância, não vieram de dados históricos, porque o
sistema não existe e o grupo não tem base de incidentes para analisar. Eles servem para tornar as
regras concretas e verificáveis. Em operação real, precisariam ser calibrados a partir do
comportamento observado dos usuários legítimos.

**Regra mal calibrada é pior que regra ausente.** Um limiar apertado demais enche a fila de falsos
positivos, e uma equipe que recebe alerta demais aprende a ignorar todos, inclusive o verdadeiro.
Um limiar frouxo demais deixa passar o ataque. Esse ajuste é contínuo e não tem resposta correta
definida de antemão.

**A detecção depende do registro, que também pode falhar.** Se o log não for gravado, se o serviço
de registro cair ou se alguém com acesso conseguir apagar linhas, a regra simplesmente não
dispara. O registro é o ativo A08 e está sujeito às mesmas ameaças que o resto do sistema, o que
inclui a alteração por um usuário interno descrita em T15.

**A cobertura é parcial.** Três regras alcançam quatro dos trinta e um riscos. A escolha foi por
prioridade e por variedade de fonte de dados, mas riscos altos ficaram sem detecção dedicada,
conforme registrado ao final da seção 4.

**Ataque lento escapa.** Todas as três regras dependem de acumular eventos dentro de uma janela de
tempo. Um atacante paciente, que consulte poucos registros por dia durante semanas, permanece
abaixo de qualquer limiar e não é detectado por nenhuma delas.

---

**Continua em:** [Etapa 7 — DevSecOps e Vídeo Final](etapa-7-devsecops-e-video-final.md)
