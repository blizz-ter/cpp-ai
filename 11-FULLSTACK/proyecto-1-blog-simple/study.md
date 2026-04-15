# Proyecto 1: Blog Simple

## Descripción

Crear API REST para blog con posts y comentarios.

## Archivos a Estudiar

- Protocol.cpp - para entender estructura de paquetes
- DBCmd.cpp - para acceso a datos

## Pasos

### Paso 1: Diseñar API

```
GET    /api/posts        - Listar posts
GET    /api/posts/:id    - Ver post
POST   /api/posts       - Crear post
PUT    /api/posts/:id   - Editar post
DELETE /api/posts/:id  - Eliminar post

GET    /api/posts/:id/comments
POST   /api/posts/:id/comments
```

### Paso 2: Implementar en MuServer

1. Agregar handling HTTP o usar componente externo
2. Mapear endpoints a operaciones existentes

### Paso 3: Frontend

Crear página simple que consuma la API.

## Recursos

- Express.js o FastAPI para API
- React o vanilla JS para frontend