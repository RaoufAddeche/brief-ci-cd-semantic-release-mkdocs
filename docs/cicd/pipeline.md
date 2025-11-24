# 🔧 Pipeline CI Détaillé

Détails techniques du pipeline CI avec GitHub Actions.

## Fichier: .github/workflows/ci.yml

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  lint:
    name: Lint & Format Check
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v4
        with:
          enable-cache: true
      - run: uv sync --group dev
      - run: uv run ruff check .
      - run: uv run ruff format --check .

  typecheck:
    name: Type Checking
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v4
      - run: uv sync --group dev
      - run: uv run mypy app/ --ignore-missing-imports

  security:
    name: Security Scan
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v4
      - run: uv sync --group dev
      - run: uv run bandit -r app/
      - run: uv run safety check

  tests:
    name: Tests & Coverage
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
        ports:
          - 5432:5432
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v4
      - run: uv sync --group dev
      - run: uv run pytest --cov=app --cov-report=xml
```

## Jobs en Parallèle

Les 4 jobs s'exécutent en **parallèle** pour gagner du temps :

| Job | Durée | Description |
|-----|-------|-------------|
| Lint | ~5-10s | Ruff check + format |
| TypeCheck | ~10-15s | Mypy verification |
| Security | ~15-20s | Bandit + Safety |
| Tests | ~20-30s | pytest + coverage |

**Total** : ~30 secondes (vs 1-2 minutes séquentiel)

## Cache uv

Le cache accélère considérablement les builds :

```yaml
- uses: astral-sh/setup-uv@v4
  with:
    enable-cache: true
    cache-dependency-glob: "uv.lock"
```

**Gain** : 2-3 minutes → 10-20 secondes

## Services PostgreSQL

Pour les tests, un container PostgreSQL temporaire est lancé :

```yaml
services:
  postgres:
    image: postgres:15
    env:
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: test_db
    options: >-
      --health-cmd pg_isready
      --health-interval 10s
    ports:
      - 5432:5432
```

## Triggers

La CI se déclenche sur :

- **Push** sur `main` ou `develop`
- **Pull Request** vers `main` ou `develop`

```yaml
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]
```

## Protection de Branches

Sur GitHub, configurer :

### Main
- ✅ Require PR before merging
- ✅ Require status checks (CI, Lint, Tests, Security)
- ✅ Require conversation resolution
- ✅ 1 approval requis

### Develop
- ✅ Require PR before merging
- ✅ Require status checks
- ⚠️ Pas d'approval requis (plus rapide)

## Badges de Statut

```markdown
![CI](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/CI/badge.svg)
```

Affiche le statut de la CI dans le README.

## Debugging

### Voir les logs

1. Aller sur Actions tab
2. Cliquer sur le workflow run
3. Cliquer sur le job qui a échoué
4. Lire les logs ligne par ligne

### Re-run

Cliquer sur "Re-run jobs" pour relancer.

### Secrets

Pour debug les secrets :

```yaml
- name: Debug (safe)
  run: echo "Workflow triggered by ${{ github.actor }}"
```

⚠️ **Jamais** `echo ${{ secrets.TOKEN }}` !

## Optimisations

### Matrix Strategy (optionnel)

Tester sur plusieurs versions Python :

```yaml
strategy:
  matrix:
    python-version: ["3.12", "3.13"]
steps:
  - uses: actions/setup-python@v5
    with:
      python-version: ${{ matrix.python-version }}
```

### Condition Skip

Skip CI sur certains commits :

```yaml
if: "!contains(github.event.head_commit.message, '[skip ci]')"
```

## Métriques

Objectifs de performance :

- ⚡ **Durée totale** : < 1 minute
- 📊 **Taux de succès** : > 95%
- 🔄 **Builds par jour** : Variable selon activité

## Prochaines Étapes

- ✅ Ajouter coverage upload vers Codecov
- ✅ Ajouter notifications Slack/Discord
- ✅ Benchmark performance
