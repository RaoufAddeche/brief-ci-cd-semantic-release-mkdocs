# 🛣️ Routes API

Documentation des endpoints REST de l'API Items.

## Endpoints disponibles

### Health Check

`GET /health` - Vérifier l'état de l'API

### Items CRUD

- `GET /items` - Liste les articles
- `POST /items` - Crée un nouvel article
- `GET /items/{item_id}` - Récupère un article par ID
- `PUT /items/{item_id}` - Met à jour un article
- `DELETE /items/{item_id}` - Supprime un article

## Détails des Routes

::: app.routes.items
    options:
      show_source: true
      heading_level: 3
