# Etapa 4 — Código Seguro e Testes de Segurança

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 3 — Arquitetura Segura](etapa-3-arquitetura-segura.md)
**Última atualização:** <!-- atualize a data ao editar --> 07/08/2026

> **Forma de realização escolhida pelo grupo:** descritiva, com pseudocódigo.
> O enunciado aceita as duas formas e avalia principalmente a coerência com os riscos, a
> qualidade das decisões e as justificativas — não a existência de um sistema executável.
> <!-- TODO(grupo): se alguém quiser escrever código executável de verdade, troque esta nota.
>      O código vai em codigo/etapa-4/implementacao/ e os testes em codigo/etapa-4/testes/. -->

**Regra de ouro desta etapa:** os testes são escritos **antes** da implementação. O enunciado
avalia explicitamente a "definição dos testes antes da solução", então mantenha essa ordem no
documento — teste primeiro, pseudocódigo depois.

---

## Prática 1 — Controle de autorização no servidor

### 1.1 Risco e requisito relacionados

| Campo | Conteúdo |
|---|---|
| Risco | R03 — extração em massa de dados pessoais pela API |
| Requisito | RS02 |
| Decisão de arquitetura | D03 — Isolamento Lógico Multi-Tenant com Middleware de Autorização Contextual |
| Referência OWASP | [Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html); OWASP API Security Top 10, API1:2023 — *Broken Object Level Authorization* |

### 1.2 Testes definidos **antes** da implementação

| Teste | Entrada ou ação | Resultado esperado |
|---|---|---|
| TS01 | Cliente autenticado como usuário A requisita `GET /pedidos/{id}` de um pedido pertencente ao usuário B | A requisição é recusada com 404 (e não 403, para não confirmar que o pedido existe); o evento é registrado em log com o identificador de quem tentou |
| TS02 | Cliente autenticado como usuário A requisita `GET /pedidos/{id}` de um pedido próprio | A requisição é permitida e retorna os dados do pedido |

> **Por que 404 e não 403 no TS01:** responder 403 confirma ao atacante que aquele identificador
> existe, o que permite enumerar a base mesmo sem ler os dados. Retornar 404 trata "não existe" e
> "não é seu" da mesma forma.

### 1.3 Implementação, pseudocódigo ou descrição

```
função obterPedido(requisicao):
    usuario = autenticar(requisicao.token)        # identidade vem do token, nunca do corpo
    se usuario é nulo:
        registrarLog("acesso sem autenticação", requisicao.ip)
        retornar 401

    pedido = repositorio.buscarPedidoPorId(requisicao.parametros.id)

    # verificação de autorização POR OBJETO: o pedido pertence a quem está pedindo?
    se pedido é nulo OU pedido.clienteId != usuario.id:
        registrarLog("tentativa de acesso a pedido de terceiro",
                     usuario.id, requisicao.parametros.id)
        retornar 404

    retornar 200, pedido
```

**Pontos que o pseudocódigo demonstra:**
1. A identidade vem **do token de sessão**, nunca de um campo enviado pelo cliente.
2. A verificação compara o dono do objeto com o usuário autenticado — não basta estar logado.
3. A tentativa negada gera log, que é o que alimenta as regras de detecção da Etapa 6.

### 1.4 Resultado esperado

Nenhum cliente autenticado consegue ler pedidos de outro cliente, mesmo conhecendo ou adivinhando
o identificador. Tentativas ficam registradas e podem disparar alerta quando repetidas.

---

## Prática 2 — <!-- TODO(Luis): escolher e preencher -->

<!-- RESPONSÁVEL: Luis Fillipe -->
<!-- TODO(Luis): escolher UMA prática, diferente da do Gabriel, ligada a um risco que VOCÊ
     registrou na Etapa 2 (Information Disclosure, DoS ou Elevation of Privilege).
     Opções listadas pelo enunciado: validação de entrada; consultas parametrizadas; controle
     de autorização (já usado); armazenamento seguro de senhas; proteção de sessões;
     tratamento seguro de erros; proteção de segredos; geração de logs sem exposição de dados
     sensíveis.
     Sugestões que casam bem com as suas ameaças:
     - Armazenamento seguro de senhas (hash com Argon2id ou bcrypt + salt) — casa com R01.
     - Geração de logs sem exposição de dados sensíveis — casa com o vazamento por logs (T18+).
     - Proteção de segredos (cofre em vez de chave embutida no app) — casa com o ativo A13. -->

### 2.1 Risco e requisito relacionados

| Campo | Conteúdo |
|---|---|
| Risco | |
| Requisito | |
| Decisão de arquitetura | |
| Referência OWASP | |

### 2.2 Testes definidos **antes** da implementação

| Teste | Entrada ou ação | Resultado esperado |
|---|---|---|
| TS03 | <!-- caso malicioso, inválido ou não autorizado --> | |
| TS04 | <!-- caso de uso válido --> | |

### 2.3 Implementação, pseudocódigo ou descrição

```
<!-- TODO(Luis) -->
```

### 2.4 Resultado esperado

<!-- TODO(Luis) -->

---

## Onde ficam os arquivos

| Conteúdo | Caminho |
|---|---|
| Código ou pseudocódigo em arquivo separado | `codigo/etapa-4/implementacao/` |
| Testes | `codigo/etapa-4/testes/` |

Pseudocódigo curto pode ficar direto neste documento, como na Prática 1. Se algum trecho crescer,
mova para `codigo/etapa-4/` e cite o arquivo aqui.

---

**Continua em:** [Etapa 5 — Verificação de Vulnerabilidades](etapa-5-verificacao-vulnerabilidades.md)
