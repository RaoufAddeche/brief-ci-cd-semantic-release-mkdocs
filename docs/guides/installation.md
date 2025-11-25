# 📥 Guide d'Installation Détaillé

Guide complet pour installer et configurer le projet.

## Prérequis Système

### Obligatoires
- **Python** : 3.13 ou supérieur
- **Git** : Pour cloner le repo
- **PostgreSQL** : 15+ (ou Docker)

### Optionnels
- **Docker** : Pour PostgreSQL et déploiement
- **Docker Compose** : Orchestration simplifiée
- **Make** : Commandes simplifiées

## Installation de Python 3.13

=== "Linux (Ubuntu/Debian)"

    ```bash
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt update
    sudo apt install python3.13 python3.13-venv
    ```

=== "macOS"

    ```bash
    brew install python@3.13
    ```

=== "Windows"

    Téléchargez depuis [python.org](https://www.python.org/downloads/)

## Installation de uv

**uv** est le gestionnaire de paquets ultra-rapide utilisé par ce projet.

=== "Linux/macOS"

    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```

=== "Windows (PowerShell)"

    ```powershell
    powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
    ```

Vérifiez l'installation :

```bash
uv --version
# uv 0.4.x
```

## Cloner le Projet

```bash
git clone https://github.com/RaoufAddeche/brief-ci-cd-semantic-release-mkdocs.git
cd brief-ci-cd-semantic-release-mkdocs
```

## Configuration PostgreSQL

### Option 1 : Docker Compose (Recommandé)

```bash
# Démarrer PostgreSQL
docker-compose up -d postgres

# Vérifier que PostgreSQL est prêt
docker-compose logs postgres
```

La base de données sera accessible sur `localhost:5432`.

### Option 2 : PostgreSQL Local

```bash
# Installer PostgreSQL
sudo apt install postgresql postgresql-contrib  # Linux
brew install postgresql@15                      # macOS

# Créer la base de données
sudo -u postgres createdb items_db

# Créer un utilisateur
sudo -u postgres psql
postgres=# CREATE USER items_user WITH PASSWORD 'secure_password';
postgres=# GRANT ALL PRIVILEGES ON DATABASE items_db TO items_user;
```

## Installation des Dépendances

```bash
# Dépendances de production
uv sync

# Avec outils de développement
uv sync --group dev

# Avec documentation
uv sync --group docs

# Tout installer
uv sync --all-groups
```

## Configuration Environnement

Créez un fichier `.env` :

```bash
cp .env.example .env
```

Éditez `.env` :

```bash
# Database
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/items_db

# Application
DEBUG=true
LOG_LEVEL=info

# Optional: pour production
# SECRET_KEY=your-super-secret-key-here
# API_KEY=your-api-key
```

## Initialisation Base de Données

Les tables sont créées automatiquement au démarrage de l'application.

## Lancement de l'Application

### Mode Développement

```bash
uv run fastapi dev app/main.py
```

Fonctionnalités :
- Rechargement automatique
- Debug mode activé
- Accessible sur http://localhost:8000

### Mode Production

```bash
uv run fastapi run app/main.py --port 8000 --host 0.0.0.0
```

## Vérification de l'Installation

### 1. Health Check

```bash
curl http://localhost:8000/health
```

Réponse attendue :
```json
{"status": "healthy"}
```

### 2. Documentation Interactive

Ouvrez : http://localhost:8000/docs

### 3. Créer un Item de Test

```bash
curl -X POST http://localhost:8000/items \
  -H "Content-Type: application/json" \
  -d '{"nom": "Test Item", "prix": 99.99}'
```

### 4. Lister les Items

```bash
curl http://localhost:8000/items
```

## Configuration Développeur

### Pre-commit Hooks

```bash
# Installer les hooks
uv run pre-commit install

# Tester sur tous les fichiers
uv run pre-commit run --all-files
```

### IDE Configuration (VS Code)

Installez les extensions :
- Python
- Pylance
- Ruff
- autoDocstring

Créez `.vscode/settings.json` :

```json
{
  "python.defaultInterpreterPath": ".venv/bin/python",
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.codeActionsOnSave": {
      "source.organizeImports": true,
      "source.fixAll": true
    },
    "editor.formatOnSave": true
  },
  "python.testing.pytestEnabled": true
}
```

## Problèmes Courants

### uv non trouvé

Rechargez votre shell :

```bash
source ~/.bashrc  # ou ~/.zshrc
```

### PostgreSQL refuse la connexion

Vérifiez que PostgreSQL tourne :

```bash
docker-compose ps  # Si Docker
sudo systemctl status postgresql  # Si local
```

### Port 8000 déjà utilisé

Changez le port :

```bash
uv run fastapi dev app/main.py --port 8001
```

## Prochaines Étapes

- ✅ [Lancer les tests](tests.md)
- ✅ [Comprendre la CI/CD](../cicd/overview.md)
- ✅ [Build Docker](docker.md)
