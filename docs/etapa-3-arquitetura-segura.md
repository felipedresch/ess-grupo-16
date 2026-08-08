# Etapa 3 — Projeto de uma Arquitetura Segura

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 2 — Análise e Tratamento de Riscos](etapa-2-riscos-nist.md)
**Última atualização:** <!-- atualize a data ao editar --> 07/08/2026

> Esta etapa transforma os riscos e controles da Etapa 2 em **requisitos de segurança** e
> **decisões de arquitetura**. Os riscos `R##` citados aqui são os mesmos da Etapa 2.

---

## Sumário

1. [Requisitos de segurança](#1-requisitos-de-segurança)
2. [Vulnerabilidades catalogadas](#2-vulnerabilidades-catalogadas)
3. [Diagrama da arquitetura segura](#3-diagrama-da-arquitetura-segura)
4. [Decisões de arquitetura](#4-decisões-de-arquitetura)

---

## 1. Requisitos de segurança

<!-- RESPONSÁVEL: Fernando -->
<!-- TODO(Fernando): selecionar TRÊS riscos classificados como críticos ou altos na Etapa 2 e
     derivar um requisito de cada. Nem mais nem menos que três — o enunciado pede exatamente
     isso. RS01 abaixo é o modelo.
     Atenção: o requisito precisa ser VERIFICÁVEL. O teste é conseguir escrever, na última
     coluna, uma frase do tipo "a operação é recusada quando X" que alguém consiga executar.
     O enunciado rejeita explicitamente requisitos genéricos como "o sistema deverá ser seguro". -->

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| RS01 | R01 — Tomada de contas de clientes | O sistema deverá exigir um segundo fator de autenticação sempre que o login ocorrer a partir de um dispositivo não reconhecido, e deverá exigir reautenticação antes de alterar e-mail, telefone ou meio de pagamento | O login a partir de um dispositivo novo deve ser recusado enquanto o segundo fator não for informado; a alteração de meio de pagamento deve ser recusada quando a reautenticação não ocorrer nos últimos 5 minutos |
| RS02 | <!-- TODO --> | | |
| RS03 | <!-- TODO --> | | |

<!-- TODO(Fernando): sugestão de riscos a usar em RS02 e RS03, se eles se confirmarem como
     críticos na priorização: R03 (extração em massa de dados pessoais pela API) → requisito de
     autorização por objeto no servidor; e o risco derivado de T29 (escalonamento de privilégio
     no backoffice) → requisito de verificação de permissão no servidor em toda operação
     administrativa. Confirme com a tabela de priorização da Etapa 2 antes de fechar. -->

---

## 2. Vulnerabilidades catalogadas

<!-- RESPONSÁVEL: Gabriel -->
<!-- TODO(Gabriel): para cada um dos três requisitos acima, encontrar UMA vulnerabilidade
     correspondente em catálogo reconhecido e preencher a linha. Três mapeamentos bastam.
     Catálogos aceitos pelo enunciado: CWE, OWASP Top 10, OWASP ASVS, OWASP Cheat Sheet Series.
     Cite o identificador exato (ex.: "CWE-287", "A01:2021") e confira o número no site antes
     de commitar — identificador errado é pior do que identificador ausente. -->

| Risco | Requisito | Vulnerabilidade ou categoria | Referência utilizada | Relação com o SaborExpress |
|---|---|---|---|---|
| R01 | RS01 | Autenticação imprópria / falha no gerenciamento de sessão | CWE-287 (*Improper Authentication*) e OWASP Top 10 A07:2021 — *Identification and Authentication Failures* | Sem segundo fator e sem limite de tentativas, uma lista de credenciais vazadas de outros serviços permite entrar em contas de clientes e usar o cartão salvo |
| R03 | RS02 | <!-- TODO: sugestão — CWE-639 (*Authorization Bypass Through User-Controlled Key*) e OWASP API Security Top 10 API1:2023 (BOLA) --> | | |
| — | RS03 | <!-- TODO: sugestão — CWE-602 (*Client-Side Enforcement of Server-Side Security*) e OWASP Top 10 A01:2021 — *Broken Access Control* --> | | |

**Links dos catálogos:** [CWE](https://cwe.mitre.org/), [OWASP Top 10](https://owasp.org/Top10/),
[OWASP API Security Top 10](https://owasp.org/API-Security/),
[OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) e
[OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

---

## 3. Diagrama da arquitetura segura

<!-- RESPONSÁVEL: Deivid -->
<!-- TODO(Deivid): produzir o diagrama e salvar fonte + imagem em diagramas/etapa-3/.
     Depois substituir este bloco pela imagem, no mesmo formato usado na Etapa 1. -->

> **Pendente.**

O enunciado (item 18.3) exige que o diagrama mostre:

- [ ] usuários;
- [ ] interface ou aplicação;
- [ ] **serviço de autenticação** (novo em relação ao diagrama da Etapa 1);
- [ ] **regras de autorização** (onde a permissão é verificada);
- [ ] banco de dados;
- [ ] **logs ou monitoramento** (novo);
- [ ] serviços externos relevantes;
- [ ] **posição dos principais controles** — ou seja, em que ponto do fluxo cada controle da
      Etapa 2 atua.

> **Dica:** a maneira mais rápida é partir do diagrama de contexto da Etapa 1 e acrescentar os
> três elementos que faltam (autenticação, autorização e logs), marcando sobre as setas onde
> cada controle entra. O que diferencia este diagrama do anterior é justamente mostrar **onde os
> controles ficam**, e não apenas quem fala com quem.

```
![Arquitetura segura](../diagramas/etapa-3/arquitetura-segura.png)
```

---

## 4. Decisões de arquitetura

<!-- RESPONSÁVEL: Fernando -->
<!-- TODO(Fernando): registrar TRÊS decisões. Cada uma precisa dos cinco campos abaixo.
     A D01 é o modelo. Uma decisão de arquitetura responde "o que escolhemos fazer e por quê",
     não "o que é bom em geral" — deve haver uma alternativa que foi descartada. -->

### D01 — Verificar autorização no servidor em todas as operações administrativas

| Campo | Conteúdo |
|---|---|
| **Problema ou risco tratado** | Risco derivado de T29 — um atendente do suporte chama diretamente endpoints administrativos que não aparecem na interface dele |
| **Decisão tomada** | Toda operação administrativa verificará o perfil e o limite de alçada no servidor, a cada requisição, a partir do token de sessão — nunca a partir de um campo enviado pelo cliente |
| **Motivo** | Ocultar opções na interface não impede o acesso direto à API. O frontend é código que roda na máquina do usuário e pode ser modificado; a única verificação confiável é a do servidor |
| **Componente afetado** | API administrativa (P04) e o painel de backoffice (A12) |
| **Resultado esperado** | Requisições administrativas feitas por perfis sem permissão são recusadas com 403 e o evento é registrado em log de auditoria, mesmo quando a chamada não passa pela interface |

### D02 — <!-- TODO(Fernando): título -->

| Campo | Conteúdo |
|---|---|
| **Problema ou risco tratado** | |
| **Decisão tomada** | |
| **Motivo** | |
| **Componente afetado** | |
| **Resultado esperado** | |

### D03 — <!-- TODO(Fernando): título -->

| Campo | Conteúdo |
|---|---|
| **Problema ou risco tratado** | |
| **Decisão tomada** | |
| **Motivo** | |
| **Componente afetado** | |
| **Resultado esperado** | |

<!-- TODO(Fernando): sugestões de decisões, caso ajudem —
     - Recalcular sempre o valor do pedido no servidor, ignorando o total enviado pelo app
       (trata o risco de Tampering de preço).
     - Validar a assinatura do webhook do gateway de pagamento antes de aceitar a confirmação
       (trata a falsificação de confirmação de pagamento em P08).
     - Guardar os segredos em cofre de segredos e nunca no código do aplicativo móvel
       (trata o vazamento de chaves de API, ativo A13).
     - Mascarar o endereço do cliente no app do entregador após a conclusão da entrega
       (trata o risco de coleta de endereços, T19). -->

---

**Continua em:** [Etapa 4 — Código Seguro e Testes de Segurança](etapa-4-codigo-seguro.md)
