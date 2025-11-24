# 🧪 Guide des Tests

Ce projet utilise **pytest** pour les tests automatisés.

## Lancer les Tests

### Commandes de Base

```bash
# Tous les tests
uv run pytest

# Avec output verbeux
uv run pytest -v

# Tests spécifiques
uv run pytest tests/test_main.py
uv run pytest tests/test_main.py::test_root
```

### Avec Coverage

```bash
# Coverage en terminal
uv run pytest --cov=app --cov-report=term-missing

# Rapport HTML
uv run pytest --cov=app --cov-report=html
open htmlcov/index.html
```

## Structure des Tests

```
tests/
├── __init__.py
├── conftest.py           # Fixtures partagées
├── test_main.py          # Tests endpoints principaux
├── test_items.py         # Tests CRUD items
└── test_services.py      # Tests services
```

## Écrire des Tests

### Exemple Simple

```python
def test_health():
    """Test health check endpoint."""
    response = client.get("/health")
    assert response.status_code == 200
    assert response.json() == {"status": "healthy"}
```

### Avec Fixtures

```python
@pytest.fixture
def sample_item():
    """Create a sample item for testing."""
    return {"nom": "Test Item", "prix": 99.99}

def test_create_item(sample_item):
    """Test creating an item."""
    response = client.post("/items", json=sample_item)
    assert response.status_code == 201
    data = response.json()
    assert data["nom"] == sample_item["nom"]
    assert data["prix"] == sample_item["prix"]
```

## Fixtures Pytest

### Client de Test

```python
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)
```

### Base de Données de Test

```python
import pytest
from sqlmodel import Session, create_engine, SQLModel

@pytest.fixture
def db_session():
    """Create test database session."""
    engine = create_engine("sqlite:///:memory:")
    SQLModel.metadata.create_all(engine)
    with Session(engine) as session:
        yield session
```

## Bonnes Pratiques

### 1. Nomenclature

- Fichiers : `test_*.py`
- Fonctions : `test_*`
- Classes : `Test*`

### 2. Structure AAA

```python
def test_example():
    # Arrange - Préparer les données
    data = {"nom": "Item", "prix": 10.0}

    # Act - Exécuter l'action
    response = client.post("/items", json=data)

    # Assert - Vérifier le résultat
    assert response.status_code == 201
```

### 3. Markers

```python
import pytest

@pytest.mark.slow
def test_long_operation():
    """Test qui prend du temps."""
    pass

@pytest.mark.integration
def test_database_integration():
    """Test d'intégration avec DB."""
    pass
```

Lancer des groupes :

```bash
pytest -m slow         # Seulement tests lents
pytest -m "not slow"   # Exclure tests lents
```

## Coverage Goals

- **Minimum** : 80%
- **Objectif** : 90%+
- **Critique** : 100% pour services

```bash
# Échouer si coverage < 80%
uv run pytest --cov=app --cov-fail-under=80
```

## Tests dans la CI

La CI exécute :

```yaml
- name: Run tests with coverage
  run: uv run pytest --cov=app --cov-report=xml
```

Le rapport est envoyé à Codecov pour suivi.

## Debugging Tests

### Mode Interactif

```bash
uv run pytest --pdb
```

### Print Debugging

```bash
uv run pytest -s  # Affiche les prints
```

### Voir les logs

```python
import logging

def test_with_logs(caplog):
    with caplog.at_level(logging.INFO):
        # Code qui log
        pass
    assert "message" in caplog.text
```

## Prochaines Étapes

- ✅ Ajouter tests pour routes
- ✅ Ajouter tests pour services
- ✅ Améliorer coverage à 90%+
