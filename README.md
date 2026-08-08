# SaborExpress — Análise de Segurança

Trabalho final da disciplina de **Engenharia de Software Seguro** — Grupo 16.

Este repositório reúne a análise de segurança completa de um sistema de **delivery de comida**, das ameaças ao pipeline DevSecOps, construída ao longo das sete etapas da disciplina. O software **não será implementado**: o foco é compreender o funcionamento do sistema e analisar seus problemas de segurança.

---

## 1. Identificação

| Item | Valor |
|---|---|
| **Nome do sistema** | SaborExpress |
| **Domínio** | Plataforma de delivery de comida (marketplace de restaurantes + entrega) |
| **Disciplina** | Engenharia de Software Seguro |
| **Grupo** | 16 |
| **Repositório** | https://github.com/felipedresch/ess-grupo-16 |
| **Prazo final de entrega** | 14/08/2026, 23:59 |

### Integrantes

| Integrante | Usuário GitHub | Responsabilidade principal |
|---|---|---|
| Felipe Nestor Dresch | `felipedresch` | Organização do repositório, descrição do sistema, detecção de intrusões, pipeline e fechamento |
| Deivid Alfonso Beise | `deividbeise-blip` | Usuários, ativos, diagramas e plano de tratamento |
| Gabriel Rodrigues da Rocha | `onhoudini` | STRIDE — Spoofing, Tampering, Repudiation; vulnerabilidades catalogadas; código seguro |
| Luis Fillipe Dias Alves | `luisfillipealuno-design` | STRIDE — Information Disclosure, DoS, Elevation of Privilege; código seguro |
| Murillo Dias Nunes | `mdngtr09` | Casos de abuso; verificação com ZAP |
| Fernando Nicola Correa | `fernandounipampa26` | Critérios e priorização de riscos; NIST CSF; requisitos e decisões de arquitetura |

> A divisão detalhada de tarefas, com prazos e status, está em **[docs/backlog.md](docs/backlog.md)**.

### Justificativa da escolha do sistema

Um aplicativo de delivery foi escolhido porque concentra, em um único produto, quase todos os elementos que tornam a análise de segurança interessante:

- **Múltiplos perfis de usuário com interesses conflitantes** — cliente, restaurante, entregador e administrador. Cada perfil pode abusar do sistema em prejuízo dos outros.
- **Transações financeiras reais** — pagamentos, repasses a restaurantes, cupons, gorjetas e reembolsos, o que cria incentivo econômico direto para fraude.
- **Dados pessoais sensíveis** — endereço residencial, telefone, geolocalização em tempo real e histórico de consumo, protegidos pela LGPD.
- **Superfície de ataque ampla** — aplicativos móveis, painel web, APIs públicas e integração com serviços externos (gateway de pagamento, mapas, notificações).
- **Requisito forte de disponibilidade** — a indisponibilidade nos horários de pico (almoço e jantar) causa prejuízo imediato e mensurável.

Essa combinação permite identificar ameaças concretas em **todas** as seis categorias do STRIDE, sem forçar cenários artificiais.

---

## 2. Os sete entregáveis

| Etapa | Documento | Conteúdo | Status |
|---|---|---|---|
| **1** | [docs/etapa-1-ameacas-stride.md](docs/etapa-1-ameacas-stride.md) | Descrição do sistema, ativos, modelagem STRIDE e casos de abuso | 🟡 Em andamento |
| **2** | [docs/etapa-2-riscos-nist.md](docs/etapa-2-riscos-nist.md) | Análise, priorização e tratamento de riscos com o NIST CSF 2.0 | 🔴 Não iniciado |
| **3** | [docs/etapa-3-arquitetura-segura.md](docs/etapa-3-arquitetura-segura.md) | Requisitos de segurança, vulnerabilidades catalogadas, diagrama e decisões | 🔴 Não iniciado |
| **4** | [docs/etapa-4-codigo-seguro.md](docs/etapa-4-codigo-seguro.md) | Práticas de código seguro com testes definidos antes da implementação | 🔴 Não iniciado |
| **5** | [docs/etapa-5-verificacao-vulnerabilidades.md](docs/etapa-5-verificacao-vulnerabilidades.md) | Verificação com OWASP ZAP e análise dos achados | 🔴 Não iniciado |
| **6** | [roteiros/etapa-6-deteccao-de-intrusoes.md](roteiros/etapa-6-deteccao-de-intrusoes.md) | Roteiro de monitoramento e regras de detecção de intrusões | 🔴 Não iniciado |
| **7** | [roteiros/etapa-7-devsecops-e-video-final.md](roteiros/etapa-7-devsecops-e-video-final.md) | Pipeline DevSecOps, roteiro e vídeo final | 🔴 Não iniciado |

**Apoio:** [docs/backlog.md](docs/backlog.md) — divisão de tarefas; [CONTRIBUTING.md](CONTRIBUTING.md) — fluxo de trabalho e padrão de commits; [diagramas/README.md](diagramas/README.md) — convenções dos diagramas.

Legenda: 🔴 não iniciado, 🟡 em andamento, 🟢 concluído

> **Nota sobre a organização em arquivos separados:** o enunciado da Etapa 2 exige que o conteúdo da Etapa 1 **não seja substituído nem apagado**, e o item 3 pede que os sete entregáveis estejam claramente identificados. Um arquivo por etapa atende às duas exigências, preserva integralmente o conteúdo anterior e reduz conflitos de versionamento entre os seis integrantes. Os documentos são encadeados: cada etapa referencia os identificadores das anteriores — ameaças (`T##`), casos de abuso (`CA##`), riscos (`R##`), requisitos (`RS##`) e testes (`TS##`).

---

## 3. Estrutura do repositório

```
ess-grupo-16/
├── README.md                                    # Este arquivo — identificação e índice
├── CONTRIBUTING.md                              # Fluxo de trabalho e padrão de commits
│
├── docs/
│   ├── etapa-1-ameacas-stride.md                # ETAPA 1
│   ├── etapa-2-riscos-nist.md                   # ETAPA 2
│   ├── etapa-3-arquitetura-segura.md            # ETAPA 3
│   ├── etapa-4-codigo-seguro.md                 # ETAPA 4
│   ├── etapa-5-verificacao-vulnerabilidades.md  # ETAPA 5
│   └── backlog.md                               # Divisão de tarefas e acompanhamento
│
├── diagramas/
│   ├── README.md                                # Convenções e checklist de legibilidade
│   ├── etapa-1/                                 # Contexto e fluxo de dados (.drawio + .png)
│   └── etapa-3/                                 # Arquitetura segura
│
├── codigo/
│   └── etapa-4/
│       ├── implementacao/                       # Código ou pseudocódigo
│       └── testes/                              # Testes de segurança
│
├── evidencias/
│   └── etapa-5/
│       ├── capturas-de-tela/                    # Prints da execução do ZAP
│       └── relatorio-da-verificacao.md          # Relatório bruto da ferramenta
│
└── roteiros/
    ├── etapa-6-deteccao-de-intrusoes.md         # ETAPA 6
    └── etapa-7-devsecops-e-video-final.md       # ETAPA 7
```

Todos os arquivos produzidos — documentos, imagens, **arquivos-fonte dos diagramas**, código e evidências — são versionados neste repositório. Nenhum material é referenciado apenas por link externo.

---

## 4. Como contribuir

Leia **[CONTRIBUTING.md](CONTRIBUTING.md)** antes do primeiro commit. Em resumo:

```bash
git clone https://github.com/felipedresch/ess-grupo-16.git
cd ess-grupo-16
git checkout -b etapa1/nome-da-secao
# edite os arquivos
git add .
git commit -m "Adiciona ameacas de falsificacao de identidade"
git push -u origin etapa1/nome-da-secao
# abra um Pull Request no GitHub
```

⚠️ **A avaliação é individual e baseada nos seus próprios commits.** Confira que o e-mail do seu Git corresponde ao da sua conta do GitHub — senão seus commits não serão atribuídos a você e não contarão:

```bash
git config user.email
```

---

## 5. Checklist final da disciplina

- [ ] **Etapa 1** — ameaças STRIDE e casos de abuso
- [ ] **Etapa 2** — análise, priorização e tratamento dos riscos
- [ ] **Etapa 3** — três requisitos, três vulnerabilidades, um diagrama e três decisões
- [ ] **Etapa 4** — duas práticas de código seguro com testes
- [ ] **Etapa 5** — uma verificação com até três achados analisados
- [ ] **Etapa 6** — roteiro com três regras de detecção
- [ ] **Etapa 7** — pipeline, roteiro e vídeo final
- [ ] Commits próprios de todos os integrantes
- [ ] Arquivos, diagramas e evidências versionados no GitHub

---

## 6. Referências

- Shostack, A. *Threat Modeling: Designing for Security* — metodologia STRIDE.
- NIST. *Cybersecurity Framework (CSF) 2.0* — https://www.nist.gov/cyberframework
- OWASP. [Top 10](https://owasp.org/Top10/), [API Security Top 10](https://owasp.org/API-Security/), [ASVS](https://owasp.org/www-project-application-security-verification-standard/) e [Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- MITRE. [CWE — Common Weakness Enumeration](https://cwe.mitre.org/)
- OWASP. [ZAP](https://www.zaproxy.org/) e [Juice Shop](https://owasp.org/www-project-juice-shop/)
- Brasil. *Lei nº 13.709/2018* — Lei Geral de Proteção de Dados Pessoais (LGPD).
