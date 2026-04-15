# Proyecto Integrador: Backend y APIs

## Descripción

Integrar todos los conceptos de backend para crear un sistema de API completo para el servidor Mu Online.

## Objetivos

1. Diseñar REST API para operaciones del juego
2. Implementar conexión a base de datos
3. Agregar sistema de autenticación
4. Implementar testing

## Arquitectura Propuesta

```
[Cliente] -> [API Gateway] -> [GameServer]
                         -> [DataServer]
                         -> [Auth Service]
```

## Endpoints a Implementar

### Autenticación
- `POST /api/auth/login` - Login de usuario
- `POST /api/auth/logout` - Logout
- `GET /api/auth/verify` - Verificar sesión

### Personajes
- `GET /api/characters` - Listar personajes
- `GET /api/characters/:name` - Obtener personaje
- `POST /api/characters` - Crear personaje
- `DELETE /api/characters/:name` - Eliminar personaje

### Inventario
- `GET /api/inventory/:charId` - Ver inventario
- `POST /api/inventory/:charId` - Agregar item

## Pasos

### Paso 1: Diseñar API
1. Definir recursos
2. Diseñar URLs
3. Definir formatos de request/response

### Paso 2: Implementar Capa HTTP
1. Agregar parser HTTP a GameServer
2. O crear servicio separado en Python/Go

### Paso 3: Integrar con Base de Datos
1. Conectar endpoints a consultas SQL
2. Manejar transacciones

### Paso 4: Autenticación
1. Implementar JWT
2. Proteger endpoints

### Paso 5: Testing
1. Unit tests
2. Integration tests
3. Load tests

## Recursos

- Express.js, FastAPI, o Go para API
- PostgreSQL o SQLite
- JWT para auth
- k6 o JMeter para load testing