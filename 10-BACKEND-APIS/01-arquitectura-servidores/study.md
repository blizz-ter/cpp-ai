# Arquitectura de Servidores

## Teoría

### Tipos de Servidores

- **Web Server**: HTTP, serving páginas
- **Game Server**: Juegos online
- **Data Server**: Base de datos
- **API Server**: REST/GraphQL

### Patrones

- **Thread per client**
- **Event-driven (epoll)**
- **Actor model**

---

## MuServer Components

```cpp
//GameServer
HANDLE hServerThread;
SOCKET serverSocket;

//DataServer  
SOCKET dataSocket;
```