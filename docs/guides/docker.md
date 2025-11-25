# 🐳 Guide Docker

Containerisation et déploiement de l'application.

## Images Disponibles

L'image Docker est disponible sur GitHub Container Registry :

```
ghcr.io/raoufaddeche/brief-ci-cd-semantic-release-mkdocs:latest
```

## Build Local

```bash
# Build l'image
docker build -t items-api:local .

# Build avec cache
docker build --cache-from items-api:latest -t items-api:local .
```

## Run avec Docker

### Simple

```bash
docker run -p 8000:8000 items-api:local
```

### Avec Variables d'Environnement

```bash
docker run -p 8000:8000 \
  -e DATABASE_URL=postgresql://user:pass@host:5432/db \
  items-api:local
```

## Docker Compose

### Lancer tout

```bash
# Démarrer tous les services
docker-compose up -d

# Voir les logs
docker-compose logs -f

# Arrêter
docker-compose down
```

### Services Disponibles

- **app** : Application FastAPI (port 8000)
- **postgres** : Base de données (port 5432)

### docker-compose.yml

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@postgres:5432/items_db
    depends_on:
      postgres:
        condition: service_healthy

  postgres:
    image: postgres:15
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: items_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

## Dockerfile Expliqué

```dockerfile
# Stage 1: Base Python avec uv
FROM python:3.13-slim

WORKDIR /app

# Installer dépendances système
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Installer uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# Activer Python système pour uv
ENV UV_SYSTEM_PYTHON=1

# Copier dépendances et installer
COPY pyproject.toml .
RUN uv pip install -e .

# Copier code application
COPY . .

EXPOSE 8000

# Lancer l'application
CMD ["fastapi", "run", "app/main.py", "--port", "8000"]
```

### Optimisations

- ✅ **Multi-stage** : Sépare build et runtime
- ✅ **Cache layers** : pyproject.toml copié avant code
- ✅ **Petite image** : Base `slim` au lieu de `full`
- ✅ **uv rapide** : Installation 10x plus rapide

## Pull depuis GHCR

```bash
# Pull l'image
docker pull ghcr.io/raoufaddeche/brief-ci-cd-semantic-release-mkdocs:latest

# Run
docker run -p 8000:8000 \
  ghcr.io/raoufaddeche/brief-ci-cd-semantic-release-mkdocs:latest
```

## Tags Disponibles

| Tag | Description |
|-----|-------------|
| `latest` | Dernière version de main |
| `develop` | Version develop |
| `v1.2.3` | Version spécifique (semantic release) |
| `main-abc123` | Commit SHA de main |

## GitHub Container Registry

### Authentification

```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

### Push Manuel

```bash
docker tag items-api:local ghcr.io/raoufaddeche/brief-ci-cd-semantic-release-mkdocs:test
docker push ghcr.io/raoufaddeche/brief-ci-cd-semantic-release-mkdocs:test
```

## Workflow CI/CD

Le workflow `.github/workflows/build.yml` :

1. Build l'image Docker
2. Tag automatiquement (branch, version, SHA)
3. Push vers GHCR
4. Utilise cache GitHub Actions

```yaml
- name: Build and push
  uses: docker/build-push-action@v5
  with:
    push: true
    tags: ${{ steps.meta.outputs.tags }}
    cache-from: type=gha
    cache-to: type=gha,mode=max
```

## Debugging

### Shell dans le container

```bash
docker run -it items-api:local /bin/bash
```

### Logs d'un container

```bash
docker logs <container-id>
docker logs -f <container-id>  # Follow
```

### Inspect

```bash
docker inspect items-api:local
```

## Production

### Bonnes Pratiques

- ✅ Utiliser tags de version, pas `latest`
- ✅ Scanner les vulnérabilités (Trivy)
- ✅ Limiter ressources (CPU, mémoire)
- ✅ Health checks configurés
- ✅ Logs structurés (JSON)

### Health Check

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:8000/health || exit 1
```

## Prochaines Étapes

- ✅ Déployer sur Azure Container Apps (Phase 8)
- ✅ Configurer load balancing
- ✅ Monitoring avec Application Insights
