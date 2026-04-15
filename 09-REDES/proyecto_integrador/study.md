# Proyecto Integrador: Redes

## Descripcion del Proyecto

Rastrear el flujo de paquetes desde el cliente hasta el servidor, analizando `ProtocolCore` en Protocol.cpp. Este proyecto te permite entender como funcionan las comunicaciones cliente-servidor en un servidor de juegos.

## Archivos a Estudiar

- `Source MuServer Update 15/GameServer/GameServer/Protocol.cpp` - Funcion ProtocolCore (lineas 75-5000+)
- `Source MuServer Update 15/GameServer/GameServer/Protocol.h` - Definiciones de paquetes
- `Source MuServer Update 15/DataServer/DataServer/DataServerProtocol.cpp` - Protocolos del DataServer

## Pasos para Completar

### Paso 1: Analizar ProtocolCore

1. Lee Protocol.cpp lineas 75-97 (entrada de ProtocolCore)
2. Observa el switch-case principal que distribuye los paquetes por `head`
3. Identifica los paquetes principales:
   - `0x00` - Chat
   - `0x02` - Whisper (chat privado)
   - `0x03` - MainCheck (keep-alive)
   - `0x0E` - LiveClient
   - `PROTOCOL_CODE2` (0x1A) - Attack
   - `PROTOCOL_CODE3` (0x1D) - Position
   - `0x18` - Action
   - `0x19` - Skill Attack

### Paso 2: Tracear un Paquete Completo

Elige un flujo para tracear (ejemplo: ataque):

```
Cliente -> 0x1A (Attack) -> ProtocolCore 
         -> gAttack.CGAttackRecv 
         -> Logica de dano 
         -> Respuesta al cliente
```

1. Busca donde se maneja el paquete 0x1A:
   - Linea 112 en Protocol.cpp
   - `gAttack.CGAttackRecv` en Attack.cpp

### Paso 3: Documentar el Flow

Crea un diagrama de flujo para el paquete de ataque (0x1A):

```
[Cliente] --(packet 0x1A)--> [ProtocolCore]
                               |
                               v
                         [ProtocolCore switch]
                               |
                               v
                       [gAttack.CGAttackRecv]
                               |
                               v
                      [Calculo de dano]
                               |
                               v
                      [Enviar a viewport]
```

### Paso 4: Analizar DataServer Protocol

1. Lee DataServerProtocol.cpp lineas 36-50
2. Observa como el DataServer recibe paquetes del GameServer
3. Identifica los codigos de protocolo:
   - `0x00` - Server Info
   - `0x01` - Character List
   - `0x02` - Character Create
   - `0x04` - Character Info
   - `0x07` - Create Item

### Paso 5: Comparar ambos protocolos

Compara la estructura de protocolos:

| Aspecto | GameServer Protocol | DataServer Protocol |
|--------|----------------|-----------------|
| Entrada | ProtocolCore | DataServerProtocolCore |
| Encabezado | head (1 byte) | head (1 byte) |
| Sub-codigo | lpMsg[3] en algunos | lpMsg[3] o lpMsg[4] |

## Archivos a Modificar

- Crea un documento `packet_flow.txt` documentando tu analisis

## Como Verificar

1. Puedes dibujar el flujo completo de al menos 3 paquetes diferentes
2. Entiendes la diferencia entre GameServer y DataServer protocols
3. Identificas donde se procesa cada tipo de paquete

## Recursos Adicionales

- Busca `LogAdd(LOG_RED` en Protocol.cpp para ver logging de paquetes
- Observa los logs en el directorio del servidor para ver traffic en vivo