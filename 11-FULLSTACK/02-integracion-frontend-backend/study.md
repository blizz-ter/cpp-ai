# Integración Frontend-Backend

## Teoría

- **CORS**: Cross-Origin Resource Sharing
- **JWT**: JSON Web Tokens para auth
- **WebSockets**: Comunicación bidireccional
- **REST + GraphQL**: APIs híbridas
- **State management**: Sincronización estado

## Código MuServer

```cpp
// MuServer original no tiene HTTP
// Integración requiere capa API
// O agregar wrapper REST al servidor
/*
GET /api/character/:name
POST /api/login
*/
```

## Ejercicio

1. Diseñar REST API para operaciones existentes
2. Crear cliente simple que consuma API