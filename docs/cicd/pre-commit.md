# 🛡️ Pre-commit Hooks

Les pre-commit hooks vérifient le code **avant** chaque commit, économisant du temps en détectant les problèmes localement.

## Installation

```bash
# Installer pre-commit
uv sync --group dev

# Activer les hooks
uv run pre-commit install
```

## Hooks Configurés

### 1. Hooks de Base
- `trailing-whitespace` - Supprime espaces en fin de ligne
- `end-of-file-fixer` - Assure une ligne vide en fin de fichier
- `check-yaml` - Valide les fichiers YAML
- `check-json` - Valide les fichiers JSON
- `detect-private-key` - Détecte les clés privées

### 2. Ruff
- Linting avec auto-fix
- Formatage automatique

### 3. Mypy
- Vérification des types

### 4. Detect Secrets
- Scan des secrets hardcodés

## Utilisation

### Commit Normal

```bash
git add .
git commit -m "feat: add new feature"
```

Les hooks s'exécutent automatiquement :
```
Trim Trailing Whitespace.........................Passed
Fix End of Files.................................Passed
Check Yaml.......................................Passed
ruff.............................................Passed
ruff-format......................................Passed
mypy.............................................Passed
detect-secrets...................................Passed
```

### Forcer le Commit (non recommandé)

```bash
git commit --no-verify -m "bypass hooks"
```

⚠️ **Attention** : La CI va probablement échouer !

## Avantages

✅ **Gain de temps** : Détection immédiate (5s vs 3min CI)
✅ **Moins de cycles CI** : Économie de ressources
✅ **Feedback instantané** : Corrections avant push
