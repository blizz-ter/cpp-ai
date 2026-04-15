# Proyecto Integrador: Fullstack

## Descripción

Construir aplicación fullstack completa que integre frontend con backend del servidor Mu Online.

## Arquitectura

```
[Frontend Web]
      |
      v
[REST/GraphQL API]
      |
      v
[GameServer] <-> [DataServer]
```

## Stack Tecnológico

- **Frontend**: React + TypeScript
- **Backend**: Python/Go o C++ con HTTP
- **Base de datos**: PostgreSQL
- **Despliegue**: Docker

## Componentes

### 1. API Layer
- REST API completa
- O GraphQL
- WebSocket para real-time

### 2. Frontend
- Login/Register
- Dashboard de personaje
- Inventario visual
- Chat

### 3. Integración
- Conexión con operaciones existentes
- Reutilización de lógica de negocio
- Sincronización de estado

## Fases

### Fase 1: API
- Diseñar e implementar REST API
- Autenticación JWT
- CRUD de personajes

### Fase 2: Frontend Base
- Setup React
- Routing
- Login form
- API client

### Fase 3: Dashboard
- Mostrar personaje
- Mostrar inventario
- Stats

### Fase 4: Integración
- Conectar con GameServer
- Sincronización en tiempo real
- WebSocket para updates

## Ejercicios

1. Implementar blog simple (proyecto 1)
2. Implementar red social básica (proyecto 2)
3. Integrar con MuServer via API

## Verificación

- [ ] API responde correctamente
- [ ] Frontend conecta con API
- [ ] Operaciones se reflejan en juego
- [ ] Stack completo funciona