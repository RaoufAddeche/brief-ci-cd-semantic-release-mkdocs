# 📦 Services

Les services encapsulent la logique métier de l'application. Ils sont séparés des routes pour faciliter les tests et la réutilisation.

## ItemService

Le service principal pour gérer les opérations CRUD sur les articles.

::: app.services.item_service.ItemService
    options:
      show_source: true
      heading_level: 3
      members:
        - get_all
        - get_by_id
        - create
        - update
        - delete
