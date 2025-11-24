# 📚 Veille Technologique - CI/CD

## 1. Comprendre CI/CD

### Qu'est-ce que la CI (Continuous Integration) ?

**Définition** : La CI est une pratique de développement où les développeurs intègrent fréquemment leur code dans un dépôt partagé, idéalement plusieurs fois par jour. Chaque intégration est vérifiée automatiquement par des builds et tests automatisés.

**Problèmes résolus par la CI** :
- **Détection précoce des bugs** : Les erreurs sont identifiées rapidement après leur introduction
- **Conflits de merge réduits** : Les intégrations fréquentes limitent les divergences de code
- **Code cassé moins longtemps** : Les problèmes sont corrigés immédiatement
- **Feedback rapide** : Les développeurs savent instantanément si leur code fonctionne

**Principes clés** :
1. **Commit fréquent** : Au moins une fois par jour sur la branche principale
2. **Build automatique** : Chaque commit déclenche un build
3. **Tests automatisés** : Suite complète de tests à chaque build
4. **Fix rapide** : Corriger les builds cassés devient la priorité #1
5. **Environnement de test identique** : Reproduit la production

**Exemples d'outils de CI** :
- **GitHub Actions** : Intégré à GitHub, workflows YAML, gratuit pour projets publics
- **GitLab CI/CD** : Intégré à GitLab, très puissant avec pipelines complexes
- **Jenkins** : Open-source, très flexible mais nécessite infrastructure propre
- **CircleCI** : SaaS, rapide avec cache intelligent
- **Travis CI** : Historiquement populaire pour projets open-source

### Qu'est-ce que le CD (Continuous Deployment/Delivery) ?

**Continuous Delivery vs Continuous Deployment** :

| Aspect | Continuous Delivery | Continuous Deployment |
|--------|-------------------|---------------------|
| **Définition** | Code toujours prêt à être déployé | Déploiement automatique en production |
| **Validation finale** | Manuelle (bouton de déploiement) | Automatique après CI |
| **Intervention humaine** | Nécessaire pour production | Aucune (sauf rollback) |
| **Risque** | Plus faible, contrôle humain | Plus élevé, mais détection rapide |
| **Usage** | Entreprises avec compliance stricte | Startups, SaaS agiles |

**Risques du CD** :
- ⚠️ **Bugs en production** : Si les tests ne couvrent pas tout
- ⚠️ **Downtime** : Déploiement mal configuré
- ⚠️ **Rollback nécessaire** : Coût de revenir en arrière
- ⚠️ **Dépendance à l'automatisation** : Si ça casse, tout s'arrête

**Bénéfices du CD** :
- ✅ **Time-to-market réduit** : Features en production en heures, pas en semaines
- ✅ **Feedback utilisateur rapide** : Validation réelle immédiate
- ✅ **Moins de stress** : Déploiements fréquents = déploiements plus petits et moins risqués
- ✅ **Qualité améliorée** : Tests rigoureux obligatoires

### Pourquoi CI/CD est important ?

**Impact sur la qualité du code** :
- 🔍 **Détection automatique** : Linting, type checking, security scans
- 🧪 **Tests systématiques** : Impossible de merger sans tests qui passent
- 📊 **Métriques de qualité** : Coverage, complexité cyclomatique
- 🛡️ **Standards uniformes** : Formatage, conventions respectées

**Impact sur la vitesse de développement** :
- ⚡ **Moins de temps debug** : Problèmes détectés tôt = moins coûteux
- 🚀 **Déploiements plus fréquents** : De 1/mois à 10+/jour
- 🔄 **Feedback loops courts** : Minutes vs jours
- 💻 **Développeurs focus sur features** : Pas sur déploiements manuels

**Impact sur la collaboration en équipe** :
- 🤝 **Code review facilité** : CI valide avant review humaine
- 📝 **Standards partagés** : Tout le monde suit les mêmes règles
- 🔔 **Transparence** : Tout le monde voit l'état du build
- 🎯 **Responsabilité collective** : Équipe responsable de la qualité

---

## 2. Maîtriser uv

### Qu'est-ce que uv ?

**uv** est un **gestionnaire de paquets et d'environnements Python ultra-rapide**, écrit en Rust par Astral (créateurs de Ruff).

**Différences avec pip/poetry/pipenv** :

| Fonctionnalité | pip | poetry | pipenv | **uv** |
|----------------|-----|--------|--------|--------|
| **Vitesse** | Lent | Moyen | Lent | **🚀 10-100x plus rapide** |
| **Résolution deps** | Basique | Excellente | Bonne | **Excellente + rapide** |
| **Lock file** | ❌ | ✅ | ✅ | **✅ (uv.lock)** |
| **Workspaces** | ❌ | ✅ | ❌ | **✅** |
| **Langage** | Python | Python | Python | **Rust (rapide)** |
| **pyproject.toml** | Partiel | ✅ | ❌ | **✅ Standard PEP** |
| **Install Python** | ❌ | ❌ | ❌ | **✅ (uv python install)** |

**Avantages de uv** :
- ⚡ **Performances exceptionnelles** : Résolution de dépendances en secondes vs minutes
- 🎯 **Standard moderne** : Suit PEP 621, 660, 735 (dependency groups)
- 🔐 **Lock file robuste** : uv.lock garantit reproductibilité
- 🛠️ **Tout-en-un** : Remplace pip, venv, pyenv, poetry
- 📦 **Cache intelligent** : Partage des paquets entre projets

### Comment uv fonctionne avec pyproject.toml ?

**Structure du fichier pyproject.toml** :

```toml
[project]
name = "mon-projet"
version = "0.1.0"
description = "Description du projet"
requires-python = ">=3.12"

# Dépendances de production (obligatoires)
dependencies = [
    "fastapi>=0.100.0",
    "sqlmodel>=0.0.14",
    "psycopg2-binary>=2.9",
]

# Dépendances de développement (optionnelles)
# PEP 735 - Dependency Groups
[dependency-groups]
dev = [
    "pytest>=8.0",
    "ruff>=0.6.0",
    "mypy>=1.11",
]

test = [
    "pytest-cov>=4.0",
    "httpx>=0.24",  # Pour tester FastAPI
]

docs = [
    "mkdocs>=1.5",
    "mkdocs-material>=9.0",
]

# Configuration du build backend
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

# Métadonnées optionnelles
[project.optional-dependencies]
security = ["bandit>=1.7", "safety>=3.0"]
```

**Gestion des dépendances par sections** :

```bash
# Installer seulement les deps de production
uv sync --no-dev

# Installer avec groupe dev
uv sync --group dev

# Installer plusieurs groupes
uv sync --group dev --group test

# Tout installer
uv sync --all-groups
```

**Build backend** :
uv utilise `hatchling` par défaut (moderne et rapide), mais supporte aussi :
- `setuptools` (traditionnel)
- `pdm-backend`
- `flit`

### Comment utiliser uv dans GitHub Actions ?

**Installation** :

```yaml
- name: Set up uv
  uses: astral-sh/setup-uv@v4
  with:
    enable-cache: true
    cache-dependency-glob: "uv.lock"
```

**Cache des dépendances** :

Le cache est automatique avec `enable-cache: true`. uv utilise un cache global qui :
- Partage les paquets entre runs
- Invalide automatiquement si `uv.lock` change
- Réduit le temps d'installation de 2-3 min à 10-20 secondes

**Exécution de commandes** :

```yaml
# Synchroniser les dépendances
- name: Install dependencies
  run: uv sync

# Exécuter des commandes
- name: Run tests
  run: uv run pytest

- name: Run linter
  run: uv run ruff check .

# Exécuter avec groupe spécifique
- name: Build docs
  run: uv sync --group docs && uv run mkdocs build
```

**Workflow complet optimisé** :

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: astral-sh/setup-uv@v4
        with:
          enable-cache: true
          cache-dependency-glob: "uv.lock"

      - run: uv sync --group test
      - run: uv run pytest --cov
```

---

## 3. Comprendre Semantic Release

### Qu'est-ce que le versionnage sémantique (SemVer) ?

**Format** : `MAJOR.MINOR.PATCH` (ex: `2.4.7`)

**Quand bumper chaque niveau** :

| Version | Quand ? | Exemple | Rétrocompatibilité |
|---------|---------|---------|-------------------|
| **MAJOR** | Breaking changes | `1.5.2` → `2.0.0` | ❌ Non compatible |
| **MINOR** | Nouvelles features | `1.5.2` → `1.6.0` | ✅ Compatible |
| **PATCH** | Bug fixes | `1.5.2` → `1.5.3` | ✅ Compatible |

**Exemples concrets** :

```
0.1.0 → 0.1.1  : fix: correction d'un bug (PATCH)
0.1.1 → 0.2.0  : feat: nouvelle fonctionnalité (MINOR)
0.2.0 → 1.0.0  : feat!: refonte de l'API (MAJOR)
1.0.0 → 1.0.1  : fix: patch de sécurité (PATCH)
1.0.1 → 1.1.0  : feat: pagination ajoutée (MINOR)
```

**Pré-releases** :
- `1.0.0-alpha.1` : Version alpha (instable)
- `1.0.0-beta.2` : Version beta (features complètes, tests en cours)
- `1.0.0-rc.1` : Release candidate (prêt sauf bugs critiques)

### Qu'est-ce que Conventional Commits ?

**Format** :

```
<type>(<scope>): <description>

[corps optionnel]

[footer optionnel]
```

**Types de commits** :

| Type | Description | Bump version ? | Exemple |
|------|-------------|----------------|---------|
| `feat` | Nouvelle fonctionnalité | MINOR | `feat(auth): add OAuth2 support` |
| `fix` | Correction de bug | PATCH | `fix(api): handle null values` |
| `docs` | Documentation | ❌ | `docs: update README` |
| `style` | Formatage (pas de logique) | ❌ | `style: format with ruff` |
| `refactor` | Refactoring | ❌ | `refactor: extract service layer` |
| `perf` | Performance | PATCH | `perf: optimize DB queries` |
| `test` | Tests | ❌ | `test: add integration tests` |
| `chore` | Maintenance | ❌ | `chore: update dependencies` |
| `ci` | CI/CD | ❌ | `ci: add GitHub Actions` |

**Breaking changes** (MAJOR bump) :

```bash
# Option 1 : ! après le type
git commit -m "feat!: redesign API endpoints"

# Option 2 : footer BREAKING CHANGE
git commit -m "feat: redesign API

BREAKING CHANGE: all endpoints now use /api/v2 prefix"
```

**Impact sur le versionnage** :

```
feat(items): add pagination        → 1.0.0 → 1.1.0 (MINOR)
fix(api): null check               → 1.1.0 → 1.1.1 (PATCH)
feat!: remove deprecated endpoints → 1.1.1 → 2.0.0 (MAJOR)
docs: update README                → 2.0.0 → 2.0.0 (pas de bump)
```

### Comment python-semantic-release fonctionne ?

**Configuration dans pyproject.toml** :

```toml
[tool.semantic_release]
version_toml = ["pyproject.toml:project.version"]
branch = "main"
build_command = "uv build"
changelog_file = "CHANGELOG.md"

[tool.semantic_release.branches.main]
match = "main"
prerelease = false

[tool.semantic_release.branches.develop]
match = "develop"
prerelease = true
prerelease_token = "dev"

[tool.semantic_release.changelog]
template_dir = "templates"
exclude_commit_patterns = [
    "^chore",
    "^ci",
    "^docs",
]

[tool.semantic_release.commit_parser_options]
allowed_tags = ["feat", "fix", "perf", "refactor"]
minor_tags = ["feat"]
patch_tags = ["fix", "perf"]
```

**Génération du CHANGELOG** :

Le CHANGELOG est généré automatiquement à partir des commits :

```markdown
# CHANGELOG

## v1.2.0 (2024-01-15)

### Features
- **items**: add pagination support (#45)
- **api**: add filtering by date (#47)

### Bug Fixes
- **database**: fix connection pool leak (#46)
- **auth**: correct JWT expiration (#48)

## v1.1.0 (2024-01-10)
...
```

**Création des releases GitHub** :

python-semantic-release :
1. Analyse les commits depuis le dernier tag
2. Calcule la nouvelle version (MAJOR/MINOR/PATCH)
3. Met à jour `pyproject.toml`
4. Crée un commit de bump : `chore(release): 1.2.0`
5. Crée un tag Git : `v1.2.0`
6. Génère le CHANGELOG.md
7. Crée une GitHub Release avec notes de version
8. Peut publier sur PyPI (optionnel)

---

## 4. MkDocs & GitHub Pages

### Comment MkDocs génère de la documentation ?

**MkDocs** est un générateur de sites statiques spécialisé pour la documentation de projets.

**Fonctionnement** :
1. **Fichiers Markdown** : Documentation écrite en `.md`
2. **Configuration** : `mkdocs.yml` définit la structure
3. **Build** : Génère HTML/CSS/JS statiques
4. **Serve** : Serveur local pour prévisualisation

**Structure typique** :

```
project/
├── mkdocs.yml           # Configuration
├── docs/
│   ├── index.md        # Page d'accueil
│   ├── getting-started.md
│   ├── api/
│   │   ├── endpoints.md
│   │   └── models.md
│   └── changelog.md
└── site/               # Généré par mkdocs build
```

**Thème Material** :
- Design moderne et responsive
- Search intégré
- Dark mode
- Navigation automatique
- Code syntax highlighting

### Comment déployer sur GitHub Pages ?

**GitHub Pages** héberge gratuitement des sites statiques depuis un repo GitHub.

**Méthodes de déploiement** :

**Méthode 1 : mkdocs gh-deploy (recommandée)** :

```bash
# Build + deploy automatique
mkdocs gh-deploy

# Crée automatiquement la branche gh-pages
# Push le site compilé
# Configure GitHub Pages
```

**Méthode 2 : GitHub Actions** :

```yaml
name: Deploy Docs
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v4
      - run: uv sync --group docs
      - run: uv run mkdocs gh-deploy --force
```

**Configuration GitHub** :
- Settings → Pages
- Source : `gh-pages` branch
- URL : `https://username.github.io/repo-name/`

### Qu'est-ce que mkdocstrings ?

**mkdocstrings** est un plugin MkDocs qui génère automatiquement de la documentation à partir des **docstrings Python**.

**Installation** :

```toml
[dependency-groups]
docs = [
    "mkdocs-material>=9.0",
    "mkdocstrings[python]>=0.24",
]
```

**Configuration** :

```yaml
plugins:
  - mkdocstrings:
      handlers:
        python:
          options:
            show_source: true
            show_root_heading: true
            docstring_style: google  # ou numpy, sphinx
```

**Usage** :

Dans un fichier markdown :

```markdown
# API Reference

## ItemService

::: app.services.item_service.ItemService
    options:
      members:
        - get_all
        - create
      show_source: true
```

**Docstring Google style** :

```python
def get_item(item_id: int, db: Session) -> Item | None:
    """Récupère un item par son ID.

    Args:
        item_id: L'identifiant unique de l'item
        db: La session de base de données active

    Returns:
        L'objet Item si trouvé, None sinon

    Raises:
        DatabaseError: Si la connexion échoue

    Examples:
        >>> item = get_item(1, db)
        >>> item.nom
        'Laptop'
    """
    return db.get(Item, item_id)
```

mkdocstrings extrait automatiquement :
- Description
- Paramètres avec types
- Valeur de retour
- Exceptions
- Exemples
- Code source (si activé)

---

## 📊 Résumé

| Technologie | Utilité | Avantage principal |
|-------------|---------|-------------------|
| **CI/CD** | Automatiser tests/déploiements | Qualité + rapidité |
| **uv** | Gestion Python moderne | 10-100x plus rapide |
| **Semantic Release** | Versionnage automatique | Pas d'erreur humaine |
| **Conventional Commits** | Commits structurés | Changelog automatique |
| **MkDocs** | Documentation élégante | GitHub Pages gratuit |
| **mkdocstrings** | Doc depuis docstrings | Toujours à jour |
