# 🔄 Pipeline CI/CD

## Vue d'ensemble

Ce projet implémente une pipeline CI/CD complète avec GitHub Actions.

## Architecture

```mermaid
flowchart TD
    A[Git Push] -->|Trigger| B{Branch?}
    B -->|develop/main| C[CI Workflow]
    C --> D[Lint & Format]
    C --> E[Type Check]
    C --> F[Security Scan]
    C --> G[Tests + Coverage]
    D --> H{All Pass?}
    E --> H
    F --> H
    G --> H
    H -->|Yes| I[Build Docker]
    H -->|No| J[❌ Failed]
    I --> K[Push to GHCR]
    B -->|main only| L[Semantic Release]
    L --> M[Create Tag]
    M --> N[Generate CHANGELOG]
    N --> O[GitHub Release]
    O --> P[Sync develop]
```

## Workflows

| Workflow | Trigger | Description |
|----------|---------|-------------|
| **CI** | Push/PR sur main/develop | Tests, linting, type checking, security |
| **Build** | Push sur main/develop | Build et push image Docker vers GHCR |
| **Release** | Push sur main | Semantic release automatique |
| **Sync** | Release | Synchronise develop avec main |
| **Docs** | Push sur main | Déploie la documentation sur GitHub Pages |

## Jobs CI en Détail

### 1. Lint & Format Check
- **Outil** : Ruff
- **Durée** : ~5s
- **Vérifie** : Style de code, imports, formatage

### 2. Type Checking
- **Outil** : Mypy
- **Durée** : ~10s
- **Vérifie** : Annotations de types

### 3. Security Scan
- **Outils** : Bandit, Safety
- **Durée** : ~15s
- **Vérifie** : Vulnérabilités code & dépendances

### 4. Tests & Coverage
- **Outil** : pytest
- **Durée** : ~20s
- **Vérifie** : Tests unitaires, couverture de code

## Badges

```markdown
![CI](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/CI/badge.svg)
![Build](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/Build%20&%20Push%20Docker%20Image/badge.svg)
![Release](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/Semantic%20Release/badge.svg)
```
