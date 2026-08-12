# Etapa 5 — Verificação de Vulnerabilidades

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 4 — Código Seguro](etapa-4-codigo-seguro.md)
**Última atualização:** 11/08/2026

<!-- RESPONSÁVEL: Murillo -->

> ### ⚠️ Ambiente autorizado
>
> Esta verificação é executada **exclusivamente** contra o **OWASP Juice Shop**, uma aplicação
> criada e distribuída pela própria OWASP para treinamento em segurança, executada localmente na
> máquina do integrante responsável.
>
> O enunciado é explícito: **é proibido testar sistemas de terceiros sem autorização**. Nenhum
> sistema real, de colega, da universidade ou de qualquer empresa foi ou será alvo desta
> verificação.
>
> Como o SaborExpress não foi implementado, o Juice Shop cumpre o papel de aplicação de teste —
> possibilidade prevista no item 24 do enunciado.

---

## 1. Ambiente e ferramenta


| Item | Valor |
|---|---|
| Sistema testado | OWASP Juice Shop v20.2.0 |
| Endereço | `http://localhost:3000` (execução local) |
| Ferramenta | OWASP ZAP 2.17.0 |
| Tipo de varredura | Automated Scan |
| Data e hora da execução | 11/08/2026 às 23:06:09 BRT |
| Executado por | Murillo Dias Nunes |


O Juice Shop foi executado localmente com Docker:

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

Depois, a aplicação foi acessada pelo navegador em:

```text
http://localhost:3000
```

O funcionamento da aplicação foi confirmado antes da execução da verificação.

O OWASP ZAP foi então utilizado para realizar um **Automated Scan** contra exclusivamente
o endereço local da aplicação. A configuração utilizada incluiu a política de scan
**Pentest**, o **Modern Spider**, o navegador **Chrome** e a opção **If Modern** para o
tratamento de aplicações web modernas.

---

## 2. Configuração básica do teste

A URL alvo utilizada durante a verificação foi:

```text
http://localhost:3000
```

correspondente à instância local do OWASP Juice Shop.

Foi executado um **Automated Scan** no OWASP ZAP, utilizando a política **Pentest**.
O crawling foi realizado com o **Modern Spider**, utilizando o navegador Chrome e a
opção **If Modern** para o tratamento de aplicações web modernas.

O escopo da verificação ficou restrito ao host local `localhost:3000`. Não foram
realizados testes contra sistemas externos ou de terceiros. A aplicação foi testada
sem autenticação nesta execução.

Uma requisição registrada na sessão do ZAP confirma o acesso ao alvo local:

```text
Tue Aug 11 23:06:09 BRT 2026
GET http://localhost:3000
200 OK
```

Essa requisição confirma que, durante a sessão de verificação, o ZAP acessou a
aplicação local e recebeu uma resposta HTTP `200 OK`.

---

## 3. Evidência da execução
A execução do ZAP apresentou seis resultados. Três deles foram selecionados para
análise detalhada, conforme solicitado pela atividade. Os demais foram registrados
na seção 4.2 por serem informativos ou por apresentarem baixa confiança.

A verificação realizada com o OWASP ZAP foi registrada por meio de capturas de tela e do
relatório gerado pela ferramenta. As evidências permitem reproduzir e conferir a configuração
utilizada, os resultados encontrados e os alertas analisados.

### Print 01 — Configuração do ataque

A primeira captura registra a configuração utilizada antes da execução do Automated Scan,
incluindo a política de scan **Pentest**, o **Modern Spider**, o navegador **Chrome** e a opção
**If Modern**.

![Print 01 — Configuração do ataque](evidencias/etapa-5/capturas-de-tela/01-zap-automated-scan-configuracao.png)

**Arquivo:**

`evidencias/etapa-5/capturas-de-tela/01-zap-configuracao-ataque.png`

### Print 02 — Cross-Domain Misconfiguration

A segunda captura registra o alerta **Cross-Domain Misconfiguration** apresentado pelo ZAP.

![Print 02 — Cross-Domain Misconfiguration](../evidencias/etapa-5/capturas-de-tela/02-zap-alerta-cross-domain-misconfiguration.png)

**Arquivo:**

`evidencias/etapa-5/capturas-de-tela/02-zap-alerta-cross-domain-misconfiguration.png`

### Print 03 — Content Security Policy (CSP) Header Not Set

A terceira captura registra o alerta **Content Security Policy (CSP) Header Not Set** apresentado
pelo ZAP.

![Print 03 — Content Security Policy (CSP) Header Not Set](../evidencias/etapa-5/capturas-de-tela/03-zap-alerta-csp-header-not-set.png)

**Arquivo:**

`evidencias/etapa-5/capturas-de-tela/03-zap-alerta-csp-header-not-set.png`

### Print 04 — HTTP Only Site

A quarta captura registra o alerta **HTTP Only Site** apresentado pelo ZAP.

![Print 04 — HTTP Only Site](../evidencias/etapa-5/capturas-de-tela/04-zap-alerta-http-only-site.png)

**Arquivo:**

`evidencias/etapa-5/capturas-de-tela/04-zap-alerta-http-only-site.png`

### Print 05 — Resumo geral dos alertas

A quinta captura apresenta a tela geral de resultados do ZAP, contendo os **seis
alertas/resultados** encontrados durante a execução.

![Print 05 — Resumo geral dos alertas](../evidencias/etapa-5/capturas-de-tela/05-zap-resumo-geral-alertas.png)

**Arquivo:**

`evidencias/etapa-5/capturas-de-tela/05-zap-resumo-geral-alertas.png`

### Relatório da execução

Além das capturas de tela, o relatório gerado pelo OWASP ZAP deve ser mantido no diretório de
evidências da Etapa 5. O relatório contém os resultados registrados pela ferramenta durante a
verificação.

**Relatório HTML:**

`evidencias/etapa-5/relatorio-zap-2026-08-12.html`

O relatório bruto da ferramenta complementa as capturas de tela, permitindo consultar os
resultados da execução sem depender exclusivamente das imagens.

![Resumo geral dos resultados da verificação](../evidencias/etapa-5/capturas-de-tela/05-zap-resumo-geral-alertas.png)

## 4. Análise dos achados

A execução do OWASP ZAP identificou seis tipos de alertas no OWASP Juice Shop. A análise abaixo
considera os resultados registrados no relatório exportado pelo ZAP, preservando os níveis de
risco, confiança, ocorrências e identificações CWE/OWASP apresentadas pela ferramenta.

Os três alertas classificados como **Medium** foram selecionados para análise detalhada. Os
demais resultados permanecem registrados na seção 4.2, pois um deles possui risco **Low** e os
outros dois possuem caráter **Informational**.

| ID | Alerta ou achado | Risco | Confiança | Ocorrências | Relação com OWASP/CWE | Evidência | Correção proposta |
|---|---|---|---|---:|---|---|---|
| A01 | **Content Security Policy (CSP) Header Not Set** | Medium | High | 5 | **CWE-693**; **OWASP 2021 A05** | `evidencias/etapa-5/capturas-de-tela/03-zap-alerta-csp-header-not-set.png` e relatório do ZAP | Configurar o cabeçalho `Content-Security-Policy` com uma política restritiva e compatível com os recursos legítimos da aplicação. |
| A02 | **Cross-Domain Misconfiguration** | Medium | Medium | 5 | **CWE-264**; **OWASP 2021 A01** | `evidencias/etapa-5/capturas-de-tela/02-zap-alerta-cross-domain-misconfiguration.png` e relatório do ZAP | Restringir as origens autorizadas e evitar configurações excessivamente permissivas de compartilhamento entre origens. |
| A03 | **HTTP Only Site** | Medium | Medium | 1 | **CWE-311**; **OWASP 2021 A05**; **OWASP 2025 A04** | `evidencias/etapa-5/capturas-de-tela/04-zap-alerta-http-only-site.png` e relatório do ZAP | Disponibilizar a aplicação por HTTPS e redirecionar as requisições HTTP para HTTPS. |

### 4.1 Comentário sobre cada achado

#### A01 — Content Security Policy (CSP) Header Not Set

O ZAP identificou a ausência do cabeçalho `Content-Security-Policy` nas respostas analisadas.
O alerta foi classificado como **Medium**, com **High confidence**, e apresentou **5 ocorrências**.

A CSP funciona como uma camada adicional de proteção no navegador, permitindo que a aplicação
defina quais origens e tipos de conteúdo podem ser carregados ou executados. A ausência desse
cabeçalho não significa, isoladamente, que exista uma exploração confirmada, mas representa a
falta de uma camada de defesa que pode reduzir o impacto de determinados ataques relacionados
ao conteúdo executado pelo navegador.

O relatório relaciona o achado à **CWE-693** e ao **OWASP Top 10 2021 A05**.

Como correção, deve ser configurado o cabeçalho `Content-Security-Policy` com uma política
adequada aos recursos legítimos da aplicação, evitando permissões desnecessariamente amplas.

**Evidência:**

![A01 — Content Security Policy (CSP) Header Not Set](../evidencias/etapa-5/capturas-de-tela/03-zap-alerta-csp-header-not-set.png)

---

#### A02 — Cross-Domain Misconfiguration

O ZAP identificou uma configuração de comunicação entre origens que merece atenção. O alerta
foi classificado como **Medium**, com **Medium confidence**, e apresentou **5 ocorrências**.

O relatório registra a presença de uma configuração permissiva de compartilhamento entre
origens, incluindo o valor `Access-Control-Allow-Origin: *`.

Uma configuração desse tipo pode permitir que recursos sejam acessados por origens mais amplas
do que o necessário. Em uma aplicação real, isso pode aumentar a superfície de exposição dos
recursos disponibilizados pela aplicação.

O relatório relaciona o achado à **CWE-264** e ao **OWASP Top 10 2021 A01**.

Como correção, as origens autorizadas devem ser definidas explicitamente de acordo com a
necessidade da aplicação, evitando o uso de permissões mais amplas do que o necessário.

**Evidência:**

![A02 — Cross-Domain Misconfiguration](../evidencias/etapa-5/capturas-de-tela/02-zap-alerta-cross-domain-misconfiguration.png)

---

#### A03 — HTTP Only Site

O ZAP identificou que o site estava sendo disponibilizado por **HTTP**, sem a proteção de
transporte fornecida pelo HTTPS. O alerta foi classificado como **Medium**, com **Medium
confidence**, e apresentou **1 ocorrência**.

No contexto de uma aplicação real, a utilização de HTTP deixa a comunicação sem as garantias de
confidencialidade e integridade fornecidas pelo HTTPS. Um intermediário capaz de observar o
tráfego poderia, dependendo do cenário, ter acesso aos dados transmitidos ou interferir na
comunicação.

O relatório relaciona o achado à **CWE-311**, ao **OWASP Top 10 2021 A05** e ao **OWASP Top 10
2025 A04**.

A correção proposta é disponibilizar a aplicação utilizando HTTPS e redirecionar as requisições
HTTP para HTTPS.

Neste trabalho, é importante considerar a limitação do ambiente: o Juice Shop foi executado
localmente em `http://localhost:3000`. Portanto, o alerta representa uma característica
observada no ambiente de teste e não uma vulnerabilidade confirmada do SaborExpress.

**Evidência:**

![A03 — HTTP Only Site](../evidencias/etapa-5/capturas-de-tela/04-zap-alerta-http-only-site.png)

---

### 4.2 Alertas descartados e resultados não selecionados para análise detalhada

Além dos três alertas analisados acima, o relatório do ZAP registrou outros três tipos de
resultado. Eles não foram selecionados para a análise detalhada porque apresentam menor
prioridade no contexto desta atividade.

É importante diferenciar os resultados: **dois são classificados pelo ZAP como
Informational**, enquanto **Timestamp Disclosure - Unix** é classificado como **Low** e possui
**Low confidence**.

| Resultado | Risco | Confiança | Ocorrências | Tratamento |
|---|---|---|---:|---|
| **Timestamp Disclosure - Unix** | Low | Low | 5 | Não selecionado para análise detalhada devido ao baixo risco e à baixa confiança indicados pelo ZAP. |
| **Modern Web Application** | Informational | Medium | 5 | Resultado informativo, mantido no relatório bruto e não tratado como vulnerabilidade. |
| **User Agent Fuzzer** | Informational | Medium | 5 | Resultado informativo, mantido no relatório bruto e não tratado como vulnerabilidade. |

Os resultados acima continuam disponíveis no relatório HTML exportado pelo ZAP. A decisão de
não analisá-los em profundidade não significa que todos sejam falsos positivos. Em especial,
os dois resultados **Informational** são registros informativos da ferramenta, enquanto
**Timestamp Disclosure - Unix** foi apenas considerado de menor prioridade para esta análise.

Dessa forma, todos os seis tipos de resultado encontrados na execução permanecem documentados:
três foram analisados detalhadamente na seção 4.1 e três foram registrados nesta seção como
resultados não selecionados para análise aprofundada.

## 5. Relação com a análise do SaborExpress

<!-- TODO(Murillo): 1 ou 2 parágrafos ligando o que a ferramenta encontrou no Juice Shop com os
     riscos que o grupo levantou nas Etapas 1 e 2. Esse é o ponto que o professor avalia como
     "relação com riscos e vulnerabilidades estudados".
     Perguntas que ajudam: algum alerta corresponde a uma ameaça T## que o grupo já tinha
     previsto? Algum achado revela um tipo de problema que o grupo NÃO tinha considerado e que
     valeria acrescentar? Os controles propostos na Etapa 2 impediriam esses achados? -->

---

## 6. Limitações desta verificação

<!-- TODO(Murillo): registrar honestamente. Exemplos que valem: a aplicação testada não é o
     SaborExpress, então os achados não são do sistema analisado; foi uma única sessão de
     varredura automatizada; o scan automatizado não encontra falhas de lógica de negócio (como
     a manipulação de preço do pedido), que são justamente algumas das ameaças mais graves que o
     grupo identificou; não houve teste autenticado. -->

---

**Continua em:** [Etapa 6 — Monitoramento e Detecção de Intrusões](../roteiros/etapa-6-deteccao-de-intrusoes.md)
