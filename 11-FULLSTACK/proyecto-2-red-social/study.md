# Proyecto 2: Red Social

## Descripción

Crear API para red social básica con usuarios, posts, follows, likes.

## Características

- **Usuarios**: Registro, perfil, sesión
- **Posts**: Crear, editar, eliminar, timeline
- **Interacción**: Follow/unfollow, likes, comentarios
- **Feed**: Posts de usuarios seguidos

## Diagrama API

```
POST   /api/auth/register  - Registro
POST   /api/auth/login     - Login
GET    /api/users/:id      - Perfil
PUT    /api/users/:id      - Editar perfil

POST   /api/posts          - Crear post
GET    /api/posts/:id     - Ver post
DELETE /api/posts/:id    - Eliminar post

POST   /api/posts/:id/like
DELETE /api/posts/:id/like

POST   /api/users/:id/follow
DELETE /api/users/:id/follow

GET    /api/feed          - Timeline
```

## Modelo de Datos

```sql
Users(id, username, email, password_hash, bio, avatar)
Posts(id, user_id, content, created_at)
Likes(id, user_id, post_id)
Follows(follower_id, following_id)
```

## Ejercicio

1. Implementar API completa
2. Crear frontend con React
3. Integrar con sistema de auth