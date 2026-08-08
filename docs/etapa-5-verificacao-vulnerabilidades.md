# Etapa 5 — Verificação de Vulnerabilidades

**Sistema:** SaborExpress — plataforma de delivery de comida
**Grupo:** 16 — Engenharia de Software Seguro
**Continuidade de:** [Etapa 4 — Código Seguro](etapa-4-codigo-seguro.md)
**Última atualização:** <!-- atualize a data ao editar --> 07/08/2026

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

<!-- TODO(Murillo): preencher com os dados reais da sua execução — versões, data e hora. -->

| Item | Valor |
|---|---|
| Sistema testado | OWASP Juice Shop <!-- TODO: versão --> |
| Endereço | `http://localhost:3000` (execução local) |
| Ferramenta | OWASP ZAP <!-- TODO: versão --> |
| Tipo de varredura | <!-- TODO: Automated Scan / Spider + Passive Scan --> |
| Data e hora da execução | <!-- TODO --> |
| Executado por | Murillo Dias Nunes |

### Como montar o ambiente

Rodar o Juice Shop com Docker é o caminho mais curto:

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

Depois abra `http://localhost:3000` no navegador para confirmar que subiu. O ZAP pode ser baixado
em [zaproxy.org/download](https://www.zaproxy.org/download/). Na tela inicial do ZAP, use
*Automated Scan*, informe `http://localhost:3000` e execute.

> Se o Docker não estiver disponível, o Juice Shop também roda via `npm start` a partir do
> repositório oficial. O professor mencionou que poderá disponibilizar um tutorial —
> vale conferir antes de sofrer com instalação.

---

## 2. Configuração básica do teste

<!-- TODO(Murillo): descrever em 3 a 5 linhas o que você configurou — URL alvo, se rodou spider,
     se usou o scan ativo ou só o passivo, qual o escopo (só localhost:3000), e se autenticou na
     aplicação ou testou sem login. Isso é o que permite alguém reproduzir a sua execução. -->

---

## 3. Evidência da execução

<!-- TODO(Murillo): salvar as capturas de tela em evidencias/etapa-5/capturas-de-tela/ e o
     relatório exportado pelo ZAP em evidencias/etapa-5/relatorio-da-verificacao.md
     (ou o HTML exportado, se preferir). Depois referencie os arquivos aqui.
     Capturas úteis: a tela do ZAP com a lista de alertas, e o detalhe de cada alerta analisado. -->

> **Pendente.**

Sugestão de capturas:

| Arquivo | O que mostra |
|---|---|
| `evidencias/etapa-5/capturas-de-tela/zap-resumo-alertas.png` | Lista geral dos alertas encontrados |
| `evidencias/etapa-5/capturas-de-tela/zap-alerta-a01.png` | Detalhe do primeiro achado analisado |
| `evidencias/etapa-5/capturas-de-tela/zap-alerta-a02.png` | Detalhe do segundo achado |
| `evidencias/etapa-5/capturas-de-tela/zap-alerta-a03.png` | Detalhe do terceiro achado |

---

## 4. Análise dos achados

<!-- TODO(Murillo): analisar TRÊS alertas. Escolha os mais relevantes, não necessariamente os
     três primeiros da lista. A linha A01 é um exemplo de FORMATO — substitua pelo que o ZAP
     realmente encontrar na sua execução, com o texto do alerta de verdade. -->

| ID | Alerta ou achado | Evidência | Possível impacto | Relação com OWASP ou CWE | Correção proposta |
|---|---|---|---|---|---|
| A01 | <!-- ex.: "Content Security Policy (CSP) Header Not Set" --> | <!-- captura ou trecho do relatório --> | <!-- consequência concreta --> | <!-- ex.: CWE-693, A05:2021 --> | <!-- medida específica --> |
| A02 | | | | | |
| A03 | | | | | |

### 4.1 Comentário sobre cada achado

<!-- TODO(Murillo): 1 parágrafo por achado, explicando o que o alerta significa em linguagem
     própria. O enunciado avalia a "interpretação dos alertas" — copiar a descrição do ZAP não
     demonstra compreensão. Diga o que a ferramenta detectou, por que aquilo é um problema e o
     que aconteceria se fosse explorado. -->

**A01 —**

**A02 —**

**A03 —**

### 4.2 Alertas descartados e falsos positivos

<!-- TODO(Murillo): o enunciado dá crédito explícito por "reconhecer limitações e possíveis
     falsos positivos". Liste aqui os alertas que você decidiu não analisar e por quê:
     informativos, duplicados, ou prováveis falsos positivos. Mesmo que o ZAP traga mais de três
     achados relevantes, vale registrar esse raciocínio. -->

---

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
