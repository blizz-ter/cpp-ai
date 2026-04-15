# REST APIs

## Teoría

- **Recursos y URIs**:Naming de endpoints
- **HTTP Methods**: GET, POST, PUT, DELETE
- **Códigos de estado**: 200, 201, 400, 404, 500
- **RESTful maturity model**: Niveles de conformance
- **HATEOAS**: Hypermedia as Engine of Application State

## Código MuServer

```cpp
// MuServer no usa REST,
// usa protocolo binario proprietário
// ProtocolCore recibe head code y procesa
switch (head) {
    case 0x00: // Chat
    case 0x02: // Whisper
    case 0x1A: // Attack
}
```

## Ejercicio

1. Comparar protocolo MuServer con REST
2. Diseñar API REST para operaciones de juego