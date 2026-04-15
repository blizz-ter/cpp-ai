# Seguridad Fullstack

## Teoría

- **XSS**: Cross-site scripting
- **CSRF**: Cross-site request forgery
- **SQL Injection**: Desde frontend
- **Auth JWT**: Tokens seguros
- **HTTPS**: Cifrado completo
- **Rate limiting**: Prevención ataques

## Código MuServer

```cpp
// Autenticación actual:
// - Login packet -> verificar credenciales
// - Sesión en memoria (gObj)
// Mejoras necesarias:
// - Tokens con expiración
// - Hash de passwords (bcrypt)
// - CSRF tokens
```

## Ejercicio

1. Implementar JWT authentication
2. Agregar rate limiting