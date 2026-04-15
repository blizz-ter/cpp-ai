# Capa de Transporte - TCP/UDP

## Teoría

### Introducción a la Capa de Transporte

La capa de transporte es la cuarta capa del modelo OSI. Proporciona comunicación lógica entre procesos de diferentes sistemas. Los protocolos principales son **TCP** y **UDP**.

### UDP (User Datagram Protocol)

- **Sin conexión**: No establece conexión antes de enviar
- **No confiable**: No hay garantía de entrega
- **Datagramas**: Mensajes independientes
- **Bajo overhead**: Cabecera pequeña (8 bytes)
- **Uso**: Streaming, VoIP, juegos en tiempo real, DNS

### TCP (Transmission Control Protocol)

- **Orientado a conexión**: Establece conexión antes de transferir
- **Confiable**: Garantiza entrega orden de paquetes
- **Flujo**: Datos tratados como stream
- **Control de congestion**: Algoritmos sofisticados
- **Uso**: HTTP, HTTPS, FTP, SMTP, bases de datos

### Puertos

- **Puerto**: Identificador de proceso (16 bits, 0-65535)
- **Puertos bien conocidos**: 0-1023 (HTTP 80, HTTPS 443)
- **Puertos registrados**: 1024-49151
- **Puertos dinámicos**: 49152-65535

### Sockets

Un socket es la combinación de:
- Dirección IP + Puerto origen
- Dirección IP + Puerto destino
- Protocolo (TCP/UDP)

---

## Ejemplos en MuServer

### Sockets en MuServer

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\User.h`

```cpp
struct OBJECTSTRUCT
{
    SOCKET Socket;           // Socket del cliente
    char IpAddr[16];        // Dirección IP del cliente
    
    // Estados de conexión
    int Connected;          // Estado de conexión
    DWORD ConnectTickCount; // Tiempo de conexión
    DWORD ClientTickCount;  // Último tick del cliente
    DWORD ServerTickCount;  // Último tick del servidor
    
    // ...
};
```

### Incluir Headers de Red

```cpp
// En stdafx.h
#include <WinSock2.h>
#include <windows.h>
```

### Inicializar Winsock

```cpp
// En el main del servidor
WSADATA wsaData;
if(WSAStartup(MAKEWORD(2, 2), &wsaData) != 0)
{
    // Error al inicializar
    return false;
}
```

### Crear Servidor TCP

```cpp
// Crear socket
SOCKET serverSocket = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);
if(serverSocket == INVALID_SOCKET)
{
    // Error
}

// Bind a puerto
SOCKADDR_IN serverAddr;
serverAddr.sin_family = AF_INET;
serverAddr.sin_port = htons(44405);  // Puerto del GameServer
serverAddr.sin_addr.s_addr = INADDR_ANY;

if(bind(serverSocket, (SOCKADDR*)&serverAddr, sizeof(serverAddr)) == SOCKET_ERROR)
{
    // Error en bind
}

// Escuchar conexiones
if(listen(serverSocket, SOMAXCONN) == SOCKET_ERROR)
{
    // Error en listen
}
```

### Aceptar Conexiones

```cpp
SOCKADDR_IN clientAddr;
int clientAddrLen = sizeof(clientAddr);

SOCKET clientSocket = accept(
    serverSocket, 
    (SOCKADDR*)&clientAddr, 
    &clientAddrLen
);

if(clientSocket != INVALID_SOCKET)
{
    // Nueva conexión aceptada
    char* clientIP = inet_ntoa(clientAddr.sin_addr);
    int clientPort = ntohs(clientAddr.sin_port);
    
    // Agregar al sistema de objetos
    short aIndex = gObjAddSearch(clientSocket, clientIP);
}
```

### Enviar Datos TCP

```cpp
int sendPacket(SOCKET socket, unsigned char* packet, int length)
{
    int result = send(socket, (const char*)packet, length, 0);
    
    if(result == SOCKET_ERROR)
    {
        int error = WSAGetLastError();
        // Manejar error
    }
    
    return result;
}
```

### Recibir Datos TCP

```cpp
int recvPacket(SOCKET socket, unsigned char* buffer, int bufferSize)
{
    int result = recv(socket, (char*)buffer, bufferSize, 0);
    
    if(result == 0)
    {
        // Conexión cerrada por el cliente
    }
    else if(result == SOCKET_ERROR)
    {
        int error = WSAGetLastError();
        // Manejar error
    }
    
    return result;
}
```

### UDP en MuServer (ejemplo conceptual)

```cpp
// Crear socket UDP
SOCKET udpSocket = socket(AF_INET, SOCK_DGRAM, IPPROTO_UDP);

// Configurar dirección
SOCKADDR_IN destAddr;
destAddr.sin_family = AF_INET;
destAddr.sin_port = htons(44405);
destAddr.sin_addr.s_addr = inet_addr("192.168.1.100");

// Enviar datagrama
int sent = sendto(
    udpSocket, 
    data, 
    dataLen, 
    0, 
    (SOCKADDR*)&destAddr, 
    sizeof(destAddr)
);
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Puertos

Responde:
1. ¿Qué puerto usa típicamente el GameServer de Mu Online?
2. ¿Por qué el puerto 44405 en lugar de uno menor como 80?
3. ¿Qué pasaría si dos servidores intentaran usar el mismo puerto?

### Ejercicio 2: TCP vs UDP

Para cada escenario, indica si usarías TCP o UDP:

| Escenario | Protocolo | Razón |
|-----------|----------|-------|
| Login de usuario | | |
| Movimiento de personaje | | |
| Chat global | | |
| Datos de tienda | | |
| Paquetes de ataque | | |

### Ejercicio 3: Código de Conexión

Analiza el siguiente pseudocódigo de aceptación de conexiones:

```cpp
while(running)
{
    // Esperar conexiones nuevas
    SOCKET client = accept(serverSocket, nullptr, nullptr);
    
    if(client != INVALID_SOCKET)
    {
        // Obtener información del cliente
        SOCKADDR_IN addr;
        int len = sizeof(addr);
        getpeername(client, (SOCKADDR*)&addr, &len);
        
        printf("Nueva conexión desde %s:%d\n", 
               inet_ntoa(addr.sin_addr), 
               ntohs(addr.sin_port));
        
        // Agregar objeto
        short aIndex = gObjAdd(client, inet_ntoa(addr.sin_addr), -1);
    }
}
```

Responde:
1. ¿Qué hace `accept()`?
2. ¿Por qué se usa `getpeername()` después de accept?
3. ¿Qué retornaría `gObjAdd()`?

### Ejercicio 4: Protocolo de Mensajes

Diseña un sistema simple de mensajes TCP que:
1. Envíe un header de 4 bytes indicando el tamaño del mensaje
2. Envíe el cuerpo del mensaje
3. El receptorlea el header primero, luego los datos restantes

```cpp
// Enviar
void sendMessage(SOCKET sock, const std::string& msg)
{
    DWORD size = msg.length();
    send(sock, (char*)&size, 4, 0);
    send(sock, msg.c_str(), size, 0);
}

// Recibir
std::string recvMessage(SOCKET sock)
{
    DWORD size;
    recv(sock, (char*)&size, 4, 0);
    
    std::string msg(size, '\0');
    recv(sock, &msg[0], size, 0);
    
    return msg;
}
```

### Ejercicio 5: Estados de Conexión

En MuServer, el estado de conexión está en `OBJECTSTRUCT.Connected`. Investiga qué valores puede tomar:
- `OBJECT_OFFLINE`
- `OBJECT_CONNECTED`
- `OBJECT_LOGGED`
- `OBJECT_ONLINE`

Luego, diseña un diagrama de estados de conexión para un jugador.
