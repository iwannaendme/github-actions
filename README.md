# GitHub Actions - Projeto de Estudos

Repositório de estudos sobre GitHub Actions, baseado no curso "Introdução ao GitHub Actions" (Microsoft/GitHub). Reúne exemplos práticos de workflows em YAML que cobrem os conceitos fundamentais da ferramenta.

## O que é o GitHub Actions

Mecanismo de automação de fluxos de trabalho integrado ao GitHub, usado principalmente para CI (integração contínua) e CD (implantação contínua), mas também aplicável a testes automatizados, resposta a issues/menções, revisão de código, gerenciamento de pull requests e branches.

Os workflows são escritos em YAML e vivem no diretório `.github/workflows` do repositório. São executados por "runners", hospedados pelo GitHub ou auto-hospedados.

## Conceitos principais

O fluxo de funcionamento segue esta cadeia:

```
Evento (trigger) -> Workflow -> Jobs -> Steps -> Actions
```

- **Evento**: algo que o GitHub rastreia e que dispara um workflow (push, pull request, schedule, webhook, disparo manual ou externo).
- **Workflow**: unidade de automação, definida em um arquivo `.yaml`. Contém um ou mais jobs.
- **Job**: conjunto de steps executados em sequência, no mesmo runner, compartilhando o mesmo sistema de arquivos. Por padrão, jobs de um workflow rodam em paralelo, a menos que uma dependência seja declarada com `needs`.
- **Step**: uma etapa dentro de um job. Pode usar uma action pronta (`uses`) ou rodar um comando no runner (`run`).
- **Action**: unidade reutilizável de código (própria ou do GitHub Marketplace) que executa uma tarefa específica dentro de um step.
- **Runner**: máquina que executa o job. Hospedado pelo GitHub (Ubuntu, Windows, macOS, sem gerenciamento de infraestrutura) ou auto-hospedado (controle total, sem os limites de tempo dos runners do GitHub, porém com responsabilidade de manutenção e mais risco de segurança em repositórios públicos).

## Sintaxe básica de um workflow

| Cláusula | Função |
|---|---|
| `name` | Nome do workflow (opcional, aparece na interface do GitHub) |
| `on` | Evento ou lista de eventos que disparam o workflow |
| `jobs` | Lista de jobs a serem executados |
| `runs-on` | Define o runner utilizado pelo job |
| `steps` | Lista de etapas do job, executadas em ordem |
| `uses` | Referencia uma action predefinida |
| `run` | Executa um comando no runner |
| `needs` | Declara dependência entre jobs (execução sequencial) |

## Estrutura do repositório

```
.
├── .github/workflows/
│   └── ola-mundo.yaml       # Aula 6: workflow funcional, disparado em cada push
├── exemplo_aula_3.yaml      # Aula 3: sintaxe básica de um workflow (matrix, checkout, build)
├── exemplo_aula_4.yaml      # Aula 4: exemplos de eventos (schedule, pull_request, push, branch, webhook)
└── exemplo_aula_5.yaml      # Aula 5: jobs com dependência (test -> deploy via needs)
```

> Os arquivos `exemplo_aula_*.yaml` na raiz são material de estudo/consulta e não são executados pelo GitHub, pois só workflows dentro de `.github/workflows` são reconhecidos. O único workflow ativo é `ola-mundo.yaml`.

### ola-mundo.yaml (workflow ativo)

Dispara a cada `push`, faz checkout do repositório e imprime "Ola, Mundo!" no log. Serve como primeiro exemplo funcional de ponta a ponta: evento -> job -> steps -> saída visível na aba **Actions** do GitHub.

### exemplo_aula_3.yaml

Demonstra as cláusulas padrão (`name`, `on`, `jobs`, `runs-on`, `steps`, `uses`, `run`) e o uso de `strategy.matrix` para rodar um build de Node.js em uma combinação de sistema operacional e versão.

### exemplo_aula_4.yaml

Reúne exemplos isolados de eventos que podem disparar um workflow:
- `schedule` com cron (`0 8-17 * * 1-5` = a cada hora, das 8h às 17h, de segunda a sexta)
- `pull_request`
- `[push, pull_request]`
- `pull_request` restrito a uma branch (`develop`)
- `gollum` (evento de webhook disparado por alterações em páginas da Wiki)

> Nota: um arquivo YAML real só pode ter uma cláusula `on`; os exemplos aqui são ilustrativos e não formam um workflow válido único.

### exemplo_aula_5.yaml

Mostra dependência entre jobs: o job `deploy` só executa se o job `test` for concluído com sucesso, usando `needs: test`.

## Gerenciamento de versão de actions

Ao referenciar uma action externa (`uses:`), é possível fixar a versão de três formas:
- **Tag** (ex.: `actions/install-timer@v2.0.1`) - versão específica e estável.
- **Hash SHA** (ex.: `actions/install-timer@327239...`) - garante imutabilidade total, mas não recebe atualizações automáticas.
- **Branch** (ex.: `actions/install-timer@develop`) - sempre a versão mais recente da branch, com risco maior de quebra.

## Depuração

A saída de cada step fica disponível na aba **Actions** do repositório, sem necessidade de acesso direto ao runner. Para investigar falhas com mais detalhe, é possível habilitar o log de depuração (debug logging) do GitHub Actions.

## Referências

- [Sintaxe de fluxo de trabalho para GitHub Actions](https://docs.github.com/pt/actions/using-workflows/workflow-syntax-for-github-actions)
- [Eventos que disparam fluxos de trabalho](https://docs.github.com/pt/actions/using-workflows/events-that-trigger-workflows)
- [Início rápido para GitHub Actions](https://aka.ms/githubactionsquickstart)
