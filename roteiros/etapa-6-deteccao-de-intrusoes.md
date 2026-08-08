# Etapa 6 — Monitoramento e Detecção de Intrusões

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 5 — Verificação de Vulnerabilidades](../docs/etapa-5-verificacao-vulnerabilidades.md)
**Última atualização:** <!-- atualize a data ao editar --> 07/08/2026

<!-- RESPONSÁVEL: Felipe -->

> Este é um **roteiro descritivo**. O enunciado é explícito: não é necessário instalar nem
> implementar um sistema de detecção de intrusões.

---

## 1. O que é detecção de intrusões

<!-- TODO(Felipe): 1 ou 2 parágrafos. Explicar com as próprias palavras, aplicado ao
     SaborExpress — o que significa observar o sistema em operação para identificar
     comportamento que indica ataque em andamento ou já ocorrido. -->

---

## 2. Prevenir e detectar não são a mesma coisa

<!-- TODO(Felipe): 1 ou 2 parágrafos. O ponto central: prevenção age ANTES, tentando impedir que
     o ataque funcione (ex.: exigir MFA no login); detecção age DURANTE ou DEPOIS, percebendo que
     algo errado está acontecendo (ex.: notar 400 tentativas de login em 2 minutos).
     Argumento que vale desenvolver: nenhuma prevenção é perfeita, e o que não é prevenido só
     pode ser tratado se for percebido. Vale amarrar com os controles da Etapa 2 — vários deles
     foram classificados nas funções Detect e Respond do NIST justamente por isso. -->

---

## 3. Eventos que o sistema deveria registrar

<!-- TODO(Felipe): listar os eventos, justificando brevemente. A lista abaixo é um ponto de
     partida derivado dos ativos e ameaças das Etapas 1 e 2 — revise e complete. -->

| Evento | Por que registrar | Ameaça relacionada |
|---|---|---|
| Tentativas de login, com sucesso ou falha, e o IP de origem | Base para detectar credential stuffing e força bruta | T01 |
| Autenticação a partir de dispositivo novo | Indica possível tomada de conta | T01 |
| Alteração de e-mail, telefone, senha ou meio de pagamento | Costuma ser o primeiro passo depois de invadir uma conta | T01 |
| Acesso negado por falta de permissão (403/404 de autorização) | Enumeração e escalonamento de privilégio aparecem aqui | T18, T29 |
| Operações administrativas: estorno, cupom, homologação, suspensão | Sem isso, ninguém consegue provar quem fez o quê | T29, repúdio |
| Alteração de dados bancários de repasse | Fraude clássica contra restaurante e entregador | T07+ |
| Confirmação de entrega, com localização registrada | Base para resolver disputa de "não recebi" | T13 |
| Aplicação de cupom | Detecta abuso em escala por contas descartáveis | T08 |
| Volume de requisições por IP, conta e endpoint | Base para detectar DoS e raspagem de dados | T24, T18 |

<!-- TODO(Felipe): acrescentar uma nota sobre o que NÃO deve entrar no log — senha, token de
     sessão, dado de cartão, CPF completo. Log é um ativo (A08) e vaza como qualquer outro. -->

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
