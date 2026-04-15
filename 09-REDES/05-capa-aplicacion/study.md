# Capa de Aplicación - Protocolos de Juego

## Teoría

### Capa de Aplicación

La capa de aplicación es la séptima capa del modelo OSI. Proporciona interfaces para que las aplicaciones accedan a los servicios de red. En servidores de juegos, esta capa implementa el protocolo específico del juego.

### Protocolos de Juego

Los protocolos de juego definen:
- **Formato de paquetes**: Estructura de datos enviados/recibidos
- **Codificación**: Cómo se representan los datos (binario, JSON, etc.)
- **Estados**: Mensajes para login, juego, logout
- **Eventos**: Acciones del jugador y respuestas del servidor

### Tipos de Paquetes

1. **Paquetes de login**: Autenticación, creación de personaje
2. **Paquetes de movimiento**: Posición, dirección, animación
3. **Paquetes de combate**: Daño, skills, efectos
4. **Paquetes de inventario**: Items, equipamiento, transacciones
5. **Paquetes de chat**: Mensajes, canales
6. **Paquetes del sistema**: Actualizaciones de estado, heartbeats

### Packet Header

Generalmente los paquetes tienen:
- **Header**: Identificador del tipo de paquete
- **Length**: Tamaño del paquete
- **Body**: Datos del paquete
- **Checksum**: Verificación de integridad

---

## Ejemplos en MuServer

### Packet Header en MuServer

```cpp
// Estructura típica de header de packet
struct PHEAD
{
    BYTE header;      // 0xC1 o 0xC3
    BYTE size;        // Tamaño del paquete
};

// Para paquetes más grandes
struct PHEAD2
{
    BYTE header;      // 0xC2 o 0xC4
    BYTE size[2];     // Tamaño en 2 bytes (little-endian)
    BYTE checkSum;    // Checksum
};
```

### Protocol.cpp - Sistema de Paquetes

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\Protocol.cpp`

```cpp
// El servidor tiene múltiples handlers de protocolo
// Ejemplo conceptual de estructura de packet

void ProtocolCore(BYTE protoNum, BYTE* aRecv, int aLen, int aIndex)
{
    switch(protoNum)
    {
        case 0x00:  // Login Request
            SubJoinLogin(aRecv, aLen, aIndex);
            break;
        case 0x01:  // Character List
            SubCharacterList(aRecv, aLen, aIndex);
            break;
        case 0x02:  // Character Create
            SubCharacterCreate(aRecv, aLen, aIndex);
            break;
        case 0x03:  // Character Delete
            SubCharacterDelete(aRecv, aLen, aIndex);
            break;
        // ... muchos más
    }
}
```

### Enviar Paquete a Cliente

```cpp
void SendPacket(int aIndex, unsigned char* packet, int length)
{
    LPOBJ lpObj = &gObj[aIndex];
    
    if(lpObj->Socket == INVALID_SOCKET)
        return;
    
    int result = send(lpObj->Socket, (const char*)packet, length, 0);
    
    if(result == SOCKET_ERROR)
    {
        gObjClose(aIndex);
    }
}
```

### Macro para Crear Paquetes

```cpp
// Macro típico para crear paquetes
#define CREATE_PAKCET(p, type, size) \
    BYTE p[size]; \
    p[0] = type; \
    p[1] = size;

// Ejemplo: packet de información de vida
void SendLifePacket(int aIndex)
{
    LPOBL lpObj = &gObj[aIndex];
    
    BYTE p[9];
    p[0] = 0x16;          // Header: HP Update
    p[1] = 9;             // Tamaño
    p[2] = 0;             // Tipo (HP)
    
    // Copiar valores (little-endian)
    memcpy(&p[3], &lpObj->Life, 4);
    memcpy(&p[7], &lpObj->MaxLife, 4);
    
    SendPacket(aIndex, p, 9);
}
```

### Packet de Movimiento

```cpp
// Paquete de posición del jugador
struct MOVE_PACKET
{
    BYTE header;        // 0xD1
    BYTE size;          // 0x0F
    BYTE type;          // 0x01
    BYTE x;             // Posición X
    BYTE y;             // Posición Y
    BYTE dir;           // Dirección
    // ... datos adicionales
};

// Procesar paquete de movimiento recibido
void CGMoveRecv(BYTE* aRecv, int aIndex)
{
    MOVE_PACKET* p = (MOVE_PACKET*)aRecv;
    
    LPOBJ lpObj = &gObj[aIndex];
    
    lpObj->X = p->x;
    lpObj->Y = p->y;
    lpObj->Dir = p->dir;
    
    // Actualizar posición en el servidor
    gObjSetPosition(aIndex, p->x, p->y);
}
```

### Packet de Inventory

```cpp
// Enviar inventory completo
void SendInventory(int aIndex)
{
    LPOBJ lpObj = &gObj[aIndex];
    
    BYTE packet[INVENTORY_SIZE + 5];
    int offset = 0;
    
    packet[offset++] = 0x24;  // Header: Inventory
    packet[offset++] = 0x00;   // SubHeader
    
    // Copiar inventory
    for(int i = 0; i < INVENTORY_SIZE; i++)
    {
        if(lpObj->Inventory[i].IsItem())
        {
            // Serial del item
            memcpy(&packet[offset], &lpObj->Inventory[i].m_Serial, 4);
            offset += 4;
            
            packet[offset++] = lpObj->Inventory[i].m_Level;
            packet[offset++] = lpObj->Inventory[i].m_Durability;
            // ... más campos
        }
        else
        {
            // Slot vacío
            packet[offset++] = 0xFF;
            packet[offset++] = 0xFF;
        }
    }
    
    // Actualizar tamaño
    packet[1] = offset;
    
    SendPacket(aIndex, packet, offset);
}
```

### Checksum

```cpp
// Verificar integridad del packet
DWORD CalculatePacketChecksum(BYTE* packet, int length)
{
    DWORD checksum = 0;
    
    for(int i = 0; i < length; i++)
    {
        checksum += packet[i];
        checksum = (checksum >> 21) | (checksum << 11);
    }
    
    return checksum;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Estructura de Paquete

Observa este paquete de login:

```
C1 0F 00 [01] [01] [username] [password]
```

Responde:
1. ¿Qué significa `0xC1` al inicio?
2. ¿Qué tamaño tiene el paquete?
3. ¿Cómo separarías el header del body?

### Ejercicio 2: Diseñar Paquete de Chat

Diseña un paquete para enviar mensajes de chat:

```cpp
struct CHAT_PACKET
{
    BYTE header;      // ?
    BYTE size;        // ?
    BYTE type;        // 0=Normal, 1=Party, 2=Guild, 3=Global
    BYTE fromLen;     // Longitud del nombre del remitente
    char fromName[11];// Nombre del remitente
    BYTE messageLen;  // Longitud del mensaje
    char message[60]; // Contenido del mensaje
};
```

### Ejercicio 3: Implementar Envío de Paquete

Implementa una función que envíe un paquete de información de personaje:

```cpp
void SendCharacterInfo(int aIndex)
{
    LPOBJ lpObj = &gObj[aIndex];
    
    // Buffer para el paquete
    BYTE packet[256];
    int offset = 0;
    
    // Header
    packet[offset++] = 0x30;  // Character Info
    
    // Completar: agregar level, experience, class, stats
}
```

### Ejercicio 4: Enum de Protocolos

Crea un enum con los principales protocolos de MuServer:

```cpp
enum Protocols
{
    PROTOCOL_LOGIN = 0x00,
    PROTOCOL_CHAR_LIST = 0x01,
    PROTOCOL_CHAR_CREATE = 0x02,
    // ... agregar al menos 10 más
};
```

### Ejercicio 5: Serialización

Explica cómo se serializaría el siguiente struct para enviar por red:

```cpp
struct ItemData
{
    int Index;
    int Level;
    int Durability;
    bool IsExcellent;
};
```

Considera:
1. Orden de bytes (endianness)
2. Tamaño de cada campo
3. Padding entre campos
