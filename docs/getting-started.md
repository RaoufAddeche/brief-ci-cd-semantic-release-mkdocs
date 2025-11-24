# 🚀 Démarrage Rapide

Ce guide vous permettra de lancer l'application en quelques minutes.

## Prérequis

- **Python 3.13+**
- **PostgreSQL 15+** (ou Docker)
- **uv** - Gestionnaire de paquets Python

## Installation

### 1. Installer uv

=== "Linux/macOS"

    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```

=== "Windows (PowerShell)"

    ```powershell
    powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```

### 2. Cloner le projet

```bash
git clone https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs.git
cd brief-ci-cd-semantic-release-mkdocs
```

### 3. Installer les dépendances

```bash
# Dépendances de production
uv sync

# Avec outils de développement
uv sync --group dev

# Avec documentation
uv sync --group docs
```

### 4. Configurer la base de données

#### Option A : PostgreSQL local

```bash
# Créer la base de données
createdb items_db

# Configurer l'URL de connexion
export DATABASE_URL="postgresql://user:password@localhost:5432/items_db"
```

#### Option B : Docker Compose (recommandé)

```bash
docker-compose up -d postgres
```

Le `docker-compose.yml` configure automatiquement PostgreSQL.

### 5. Lancer l'application

```bash
# Mode développement avec rechargement automatique
uv run fastapi dev app/main.py

# Mode production
uv run fastapi run app/main.py
```

L'API est accessible sur **http://localhost:8000**

## Vérification

### Health Check

```bash
curl http://localhost:8000/health
```

Réponse attendue :
```json
{"status": "healthy"}
```

### Documentation Interactive

Ouvrez votre navigateur : [http://localhost:8000/docs](http://localhost:8000/docs)

Vous aurez accès à :
- 📖 Documentation Swagger interactive
- 🧪 Tests des endpoints directement dans le navigateur
- 📝 Schémas de données

### Premier appel API

#### Créer un item

```bash
curl -X POST http://localhost:8000/items \
  -H "Content-Type: application/json" \
  -d '{
    "nom": "Laptop",
    "prix": 999.99
  }'
```

#### Lister les items

```bash
curl http://localhost:8000/items
```

## Configuration Avancée

### Variables d'environnement

Créez un fichier `.env` :

```bash
# Base de données
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/items_db

# FastAPI
DEBUG=true
```

### Pre-commit hooks (développeurs)

```bash
# Installer les hooks
uv run pre-commit install

# Exécuter sur tous les fichiers
uv run pre-commit run --all-files
```

## Développement

### Lancer les tests

```bash
# Tous les tests
uv run pytest

# Avec coverage
uv run pytest --cov=app --cov-report=html

# Ouvrir le rapport de coverage
open htmlcov/index.html
```

### Quality checks

```bash
# Linting
uv run ruff check .

# Auto-fix
uv run ruff check --fix .

# Formatage
uv run ruff format .

# Type checking
uv run mypy app/
```

## Prochaines Étapes

- 📖 [API Reference](api/routes.md) - Explorer les endpoints
- 🔧 [Configuration CI/CD](cicd/overview.md) - Comprendre le pipeline
- 🐳 [Guide Docker](guides/docker.md) - Containerisation
- 🧪 [Guide Tests](guides/tests.md) - Écrire des tests
