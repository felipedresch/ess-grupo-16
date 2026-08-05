# SaborExpress — Análise de Segurança

Trabalho final da disciplina de **Engenharia de Software Seguro** — Grupo 16.

Este repositório reúne toda a análise de segurança de um sistema de **delivery de comida**,
construída ao longo das etapas da disciplina. O software **não será implementado**: o objetivo
é compreender o funcionamento do sistema e analisar seus problemas de segurança antes da
implementação.

---

## 1. Identificação

| Item | Valor |
|---|---|
| **Nome do sistema** | SaborExpress |
| **Domínio** | Plataforma de delivery de comida (marketplace de restaurantes + entrega) |
| **Disciplina** | Engenharia de Software Seguro |
| **Grupo** | 16 |
| **Repositório** | https://github.com/<ORG-OU-USUARIO>/<REPO> <!-- TODO: preencher com a URL real --> |
| **Prazo final de entrega** | 14/08/2026, 23:59 |

### Integrantes

| Integrante | Usuário GitHub | Responsabilidade principal |
|---|---|---|
| Felipe Nestor Dresch | `<TODO>` | Organização do repositório, descrição do sistema, revisão e fechamento |
| Deivid Alfonso Beise | `deividbeise-blip` | Usuários, ativos e diagramas |
| Gabriel Rodrigues da Rocha | `onhoudini` | STRIDE — Spoofing, Tampering, Repudiation |
| Luis Fillipe Dias Alves | `luisfillipealuno-design` | STRIDE — Information Disclosure, DoS, Elevation of Privilege |
| Murillo Dias Nunes | `mdngtr09` | Casos de abuso e mapeamento NIST CSF |
| Fernando Nicola Correa | `<TODO>` | Critérios, registro e priorização de riscos |

> A divisão detalhada de tarefas, com prazos e status, está em
> **[docs/backlog.md](docs/backlog.md)**.

### Justificativa da escolha do sistema

Um aplicativo de delivery foi escolhido porque concentra, em um único produto, quase todos os
elementos que tornam a análise de segurança interessante:

- **Múltiplos perfis de usuário com interesses conflitantes** — cliente, restaurante,
  entregador e administrador. Cada perfil pode abusar do sistema em prejuízo dos outros.
- **Transações financeiras reais** — pagamentos, repasses a restaurantes, cupons, gorjetas e
  reembolsos, o que cria incentivo econômico direto para fraude.
- **Dados pessoais sensíveis** — endereço residencial, telefone, geolocalização em tempo real e
  histórico de consumo, protegidos pela LGPD.
- **Superfície de ataque ampla** — aplicativos móveis, painel web, APIs públicas e integração
  com serviços externos (gateway de pagamento, mapas, notificações).
- **Requisito forte de disponibilidade** — a indisponibilidade nos horários de pico (almoço e
  jantar) causa prejuízo imediato e mensurável.

Essa combinação permite identificar ameaças concretas em **todas** as seis categorias do STRIDE,
sem forçar cenários artificiais.

---

## 2. Documentos do trabalho

| Etapa | Documento | Status |
|---|---|---|
| — | [README.md](README.md) — identificação e índice | 🟡 Em andamento |
| **Etapa 1** | [docs/modelagem-de-ameacas.md](docs/modelagem-de-ameacas.md) — Casos de abuso e modelagem STRIDE | 🔴 Não iniciado |
| **Etapa 2** | [docs/analise-de-riscos.md](docs/analise-de-riscos.md) — Análise, priorização e tratamento com NIST CSF 2.0 | 🔴 Não iniciado |
| — | [docs/backlog.md](docs/backlog.md) — divisão de tarefas e acompanhamento | 🟢 Ativo |
| — | [CONTRIBUTING.md](CONTRIBUTING.md) — como contribuir, padrão de commits | 🟢 Ativo |

Legenda de status: 🔴 não iniciado · 🟡 em andamento · 🟢 concluído/ativo

> **Nota sobre a organização em dois arquivos:** o enunciado da Etapa 2 exige que o conteúdo da
> Etapa 1 **não seja substituído nem apagado**. Mantemos as etapas em arquivos separados e
> claramente identificados (permitido pelo item 1 do enunciado: *"README.md ou outro arquivo
> claramente identificado"*), o que garante a preservação da Etapa 1 e reduz conflitos de
> versionamento entre os seis integrantes. A Etapa 2 referencia explicitamente os identificadores
> de ameaça (`T##`) e de caso de abuso (`CA##`) definidos na Etapa 1.

---

## 3. Estrutura do repositório

```
ess-grupo-16/
├── README.md                        # Identificação, índice e visão geral (este arquivo)
├── CONTRIBUTING.md                  # Fluxo de trabalho, padrão de commits e revisão
├── .gitignore
├── docs/
│   ├── modelagem-de-ameacas.md      # ETAPA 1 — descrição, ativos, STRIDE, casos de abuso
│   ├── analise-de-riscos.md         # ETAPA 2 — riscos, priorização, NIST CSF, tratamento
│   └── backlog.md                   # Divisão de tarefas, responsáveis, prazos e status
├── diagramas/
│   ├── README.md                    # Convenções de nomenclatura e exportação
│   └── fonte/                       # Arquivos editáveis (.drawio) dos diagramas
└── imagens/                         # Imagens exportadas (.png) usadas nos documentos
```

Todos os arquivos produzidos — documentos, imagens, diagramas e **arquivos-fonte dos diagramas** —
são versionados neste repositório. Nenhum diagrama é referenciado apenas por link externo.

---

## 4. Como contribuir

Leia **[CONTRIBUTING.md](CONTRIBUTING.md)** antes do primeiro commit. Em resumo:

```bash
git clone https://github.com/<ORG-OU-USUARIO>/<REPO>.git
cd <REPO>
git checkout -b etapa1/nome-da-secao
# edite os arquivos
git add .
git commit -m "Adiciona ameacas de falsificacao de identidade"
git push -u origin etapa1/nome-da-secao
# abra um Pull Request no GitHub
```

⚠️ **A avaliação é individual e baseada nos seus próprios commits.** Confira que seu e-mail do
Git corresponde ao da sua conta do GitHub, senão seus commits não serão atribuídos a você:

```bash
git config user.email
```

---

## 5. Referências

- Shostack, A. *Threat Modeling: Designing for Security* — metodologia STRIDE.
- NIST. *Cybersecurity Framework (CSF) 2.0* — https://www.nist.gov/cyberframework
- Brasil. *Lei nº 13.709/2018* — Lei Geral de Proteção de Dados Pessoais (LGPD).
- OWASP. *Top 10* e *API Security Top 10*.
