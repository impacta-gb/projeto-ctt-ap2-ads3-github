[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/Du716tKn)

# Documentação Go — CTT AP2

Site de documentação da linguagem **Go** construído com **Zensical** e publicado automaticamente no **GitHub Pages** via **GitHub Actions**.

## Integrantes

| Nome | RA |
|---|---|
| Giovanni Moreira | 2500078 |
| João Pedro Telmo Ferroni | 2404404 |
| Lucas Soares Branco | 2404395 |
| Vitor Gabriel | 2501307 |

---

## Fluxo de Trabalho da Equipe

Adotamos um fluxo colaborativo baseado em **Feature Branches** com revisão obrigatória via Pull Request antes de qualquer merge na `main`.

### Regras adotadas

- A branch `main` tem **proteção contra push direto** (Branch Protection Rules no GitHub)
- Todo trabalho é feito em uma branch separada com nomenclatura descritiva
- Ao finalizar, o autor abre um **Pull Request** para a `main`
- Pelo menos **um outro membro** revisa o PR, deixa comentários (quando necessário) e aprova antes do merge

### Convenção de nomes de branch

| Tipo | Padrão | Exemplo |
|---|---|---|
| Nova página de documentação | `feat/doc-<topico>` | `feat/doc-goroutines` |
| Correção de workflow/CI | `fix/ci-<descricao>` | `fix/ci-cache` |
| Correção de conteúdo | `fix/<descricao>` | `fix/nav-order` |
| Documentação do projeto | `docs/<descricao>` | `docs/readme` |

### Como as revisões foram feitas

1. O autor da branch abre o PR com descrição do que foi feito
2. O revisor lê o diff, testa localmente se necessário e deixa comentários
3. O autor responde/corrige e solicita nova revisão
4. Após aprovação, o merge é feito via interface do GitHub (nunca por linha de comando direto na main)

---

## Conteúdo do Site

As páginas de documentação cobrem os seguintes tópicos de Go, nesta ordem:

1. Introdução e Instalação
2. Sintaxe Básica e Variáveis
3. Estruturas de Controle (If, For, Switch)
4. Arrays, Slices e Maps
5. Structs e Métodos
6. Tratamento de Erros (Error Handling)
7. Concorrência I: Goroutines
8. Concorrência II: Channels
9. Gerenciamento de Pacotes (Go Modules)
10. Testes Automatizados em Go

---

## Arquitetura do CI/CD (GitHub Actions)

O pipeline é dividido em dois workflows com responsabilidades distintas.

### `ci.yml` — Validação (roda em Pull Requests)

Garante que nenhum PR quebre o site antes de ser aprovado.

```
Trigger: pull_request → main
│
└── job: validate
    strategy:
      matrix:
        python-version: [3.10, 3.11]   ← roda em paralelo nas duas versões
    │
    ├── Cache de dependências pip (actions/cache)
    ├── pip install -r requirements.txt
    └── zensical build --clean          ← falha o PR se o build quebrar
```

### `docs.yml` — Build e Deploy (roda em push e schedule)

Publica o site no GitHub Pages após cada merge e automaticamente toda semana.

```
Triggers:
  - push → main          (após merge de PR)
  - schedule: 0 0 * * 0  (toda domingo à meia-noite)
│
├── job: build_site
│   ├── Cache de dependências pip (actions/cache)
│   ├── pip install -r requirements.txt
│   ├── zensical build --clean
│   └── upload-pages-artifact → empacota o site/ gerado
│
└── job: deploy_site
    needs: build_site                               ← aguarda o build terminar
    if: push ou schedule (NUNCA em pull_request)    ← condicional de segurança
    └── deploy-pages → publica no GitHub Pages
```

### Por que dois jobs separados?

O desacoplamento entre `build_site` e `deploy_site` garante:
- **Segurança**: o deploy nunca é executado durante um PR, apenas quando há push real na main ou via schedule
- **Rastreabilidade**: o artefato gerado pelo build fica registrado na aba Actions antes do deploy
- **Resiliência**: se o deploy falhar, o artefato continua disponível para reenvio sem necessidade de rebuild

---

## Como Rodar Localmente

### Pré-requisitos

- Python 3.10 ou superior
- pip

### Configuração

```powershell
# 1. Criar e ativar o ambiente virtual
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# 2. Instalar dependências
pip install -r requirements.txt

# 3. Servidor local com hot reload
zensical serve
```

Acesse em `http://localhost:8000`

### Build estático

```powershell
zensical build --clean
```

Os arquivos gerados ficam em `site/`.

---

## Estrutura do Projeto

```
projeto-ctt-ap2-ads3-github/
├── README.md                        # este arquivo
├── requirements.txt                 # dependências Python
├── zensical.toml                    # configuração do site e navegação
├── docs/                            # fontes Markdown
│   ├── index.md
│   ├── introducao-instalacao.md
│   ├── sintaxe-basica.md
│   ├── estruturas-controle.md
│   ├── arrays-slices-maps.md
│   ├── structs-metodos.md
│   ├── tratamento-erros.md
│   ├── concorrencia-i-goroutines.md
│   ├── concorrencia-ii-channels.md
│   ├── go-modules.md
│   └── testes-automatizados.md
├── site/                            # gerado automaticamente (não editar)
└── .github/
    └── workflows/
        ├── ci.yml                   # validação em PRs (matrix + cache)
        └── docs.yml                 # build + deploy (jobs separados)
```
