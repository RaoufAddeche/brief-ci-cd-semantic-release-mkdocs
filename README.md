# 🚀 Items API - CI/CD Pipeline Complet

![CI](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/CI/badge.svg)
![Build](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/Build%20%26%20Push%20Docker%20Image/badge.svg)
![Release](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/workflows/Semantic%20Release/badge.svg)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)
![Python](https://img.shields.io/badge/Python-3.13-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.121+-009688?logo=fastapi&logoColor=white)

> 🎓 **Projet Brief CI/CD** - Démonstration complète d'une pipeline CI/CD professionnelle avec GitHub Actions, Semantic Release, et bonnes pratiques DevOps modernes.

## 📖 Documentation

**📚 [Documentation complète](https://raoufaddeche.github.io/brief-ci-cd-semantic-release-mkdocs/)**

## 🎯 Objectifs du Projet

Ce projet démontre la mise en place d'une **pipeline CI/CD de bout en bout** avec :

- ✅ **CI complète** : Lint, Type Check, Security Scan, Tests
- ✅ **Pre-commit hooks** : Validation avant chaque commit
- ✅ **Semantic Release** : Versionnage automatique
- ✅ **Docker** : Build et push automatique vers GHCR
- ✅ **Documentation** : Auto-générée avec MkDocs
- ✅ **GitFlow** : Stratégie de branches professionnelle

## 🛠️ Stack Technique

### Backend
- **FastAPI** - Framework web performant
- **SQLModel** - ORM type-safe
- **PostgreSQL** - Base de données
- **uv** - Gestionnaire de paquets ultra-rapide

### DevOps
- **GitHub Actions** - CI/CD
- **Ruff** - Linter/Formatter (10-100x plus rapide)
- **Mypy** - Type checker
- **pytest** - Tests
- **Bandit** - Security scanner
- **python-semantic-release** - Versionnage automatique
- **MkDocs Material** - Documentation

## 🚀 Quick Start

### Prérequis

- Python 3.13+
- PostgreSQL 15+ (ou Docker)
- [uv](https://docs.astral.sh/uv/) installé

### Installation

```bash
# Cloner le projet
git clone https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs.git
cd brief-ci-cd-semantic-release-mkdocs

# Installer uv (si pas déjà fait)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Installer les dépendances
uv sync

# Lancer la base de données (Docker)
docker-compose up -d postgres

# Lancer l'application
uv run fastapi dev app/main.py
```

Ouvrez **http://localhost:8000/docs** pour la documentation interactive Swagger.

## 📁 Structure du Projet

```
.
├── .github/workflows/      # GitHub Actions workflows
├── app/                   # Code source
│   ├── main.py
│   ├── models/
│   ├── routes/
│   ├── schemas/
│   └── services/
├── tests/                 # Tests pytest
├── docs/                  # Documentation MkDocs
├── pyproject.toml         # Configuration
├── Dockerfile
├── docker-compose.yml
└── mkdocs.yml
```

## 📚 Livrables du Brief

### Phase 0 : Veille Technologique ✅
- [`VEILLE_CICD.md`](./VEILLE_CICD.md) - Recherche sur CI/CD, uv, semantic release
- [`COMPARATIF_OUTILS.md`](./COMPARATIF_OUTILS.md) - Comparaison linters, formatters, etc.

### Phase 1 : Découverte du Projet ✅
- [`PROBLEMES_DETECTES.md`](./PROBLEMES_DETECTES.md) - 35 problèmes identifiés

### Phase 2 : Stratégie Git & Branches ✅
- Branche `develop` créée
- Conventional Commits utilisés

### Phase 3 : CI Pipeline ✅
- [`.github/workflows/ci.yml`](./.github/workflows/ci.yml) - 4 jobs parallèles

### Phase 4 : Pre-commit Hooks ✅
- [`.pre-commit-config.yaml`](./.pre-commit-config.yaml)

### Phase 5 : Docker & GHCR ✅
- [`.github/workflows/build.yml`](./.github/workflows/build.yml)
- Image : `ghcr.io/raoufaddeche/brief-ci-cd-semantic-release-mkdocs`

### Phase 6 : Semantic Release ✅
- [`.github/workflows/release.yml`](./.github/workflows/release.yml)

### Phase 7 : Documentation MkDocs ✅
- [`mkdocs.yml`](./mkdocs.yml) + [`docs/`](./docs/)
- [`.github/workflows/docs.yml`](./.github/workflows/docs.yml)

## 🔧 Commandes Utiles

```bash
# Développement
uv run fastapi dev app/main.py

# Tests
uv run pytest --cov=app

# Linting & formatage
uv run ruff check .
uv run ruff format .

# Type checking
uv run mypy app/

# Pre-commit
uv run pre-commit run --all-files
```

## 📦 Versionnage Sémantique

Exemples de commits conventionnels :

```bash
feat: add pagination     # MINOR bump
fix: handle null values  # PATCH bump
feat!: redesign API     # MAJOR bump
```

## 🔗 Liens

- 📚 [Documentation](https://raoufaddeche.github.io/brief-ci-cd-semantic-release-mkdocs/)
- 🐙 [GitHub](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs)
- 🐳 [Docker Image](https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs/pkgs/container/brief-ci-cd-semantic-release-mkdocs)

---

**Built with uv and FastAPI** 🚀