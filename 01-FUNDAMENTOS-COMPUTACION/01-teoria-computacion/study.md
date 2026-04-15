# Teoría de la Computación

## Teoría

### Fundamentos de la Computación

La teoría de la computación estudia qué problemas pueden resolverse algorítmicamente y cómo. Los conceptos fundamentales incluyen:

- **Algoritmo**: Secuencia finita de instrucciones que resuelve un problema
- **Máquina de Turing**: Modelo teórico de computación universal
- **Computabilidad**: Qué problemas son resolubles vs. irresolubles
- **Complejidad**: Cuántos recursos (tiempo, memoria) necesita un algoritmo

### Teoría de Autómatas

Los autómatas son modelos matemáticos de computación:
- **Autómatas Finitos**: Estados limitados, usados en parsers
- **Expresiones Regulares**: Patrones de texto
- **Gramáticas**: Reglas para generar lenguajes

### Relevancia en Servidores de Juegos

Los servidores de juegos implementan:
- Protocolos de comunicación (parsers de protocolos)
- Máquinas de estados para personajes y NPCs
- Lógica de juego basada en reglas

---

## Código del MuServer

### stdafx.h - Headers del Sistema

**Archivo**: `Source Main 5.2\source\stdafx.h`

```cpp
// Include de Windows y sistema
#include <windows.h>

// Windows Sockets
#include <WinSock2.h>

// Standard C runtime
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <vector>
#include <map>
#include <list>
```

### USER__POST - Máquina de Estados

```cpp
enum eCONNECT_DB_STATE
{
    CONNECT_DB_STATE_DISCONNECTED = 0,
    CONNECT_DB_STATE_CONNECTED = 1,
    CONNECT_DB_STATE_LOGGED = 2,
};

// Máquina de estados de conexión
enum eUserState 
{
    eUserState_Create = 0,
    eUserState_Playing = 1,
    eUserState_Logged = 2,
    eUserState_Quit = 3,
};
```

### Protocol.cpp - Parser de Protocolos

```cpp
// Protocol handler ejemplo
void DataServerProtocol(short protocolId, BYTE* buffer, int size)
{
    switch(protocolId)
    {
        case 0x01:
            // Login request
            break;
        case 0x02:
            // Character list
            break;
        case 0x03:
            // Create character
            break;
    }
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Estados de Conexión

Dado el siguiente código:

```cpp
enum eCONNECT_DB_STATE
{
    CONNECT_DB_STATE_DISCONNECTED = 0,
    CONNECT_DB_STATE_CONNECTED = 1,
    CONNECT_DB_STATE_LOGGED = 2,
};
```

**Preguntas**:
1. ¿Cuántos estados diferentes tiene este autómata?
2. ¿Es un autómata determinista? ¿Por qué?
3. Dibuja el diagrama de estados

### Ejercicio 2: Diseñar Máquina de Estados

Diseña una máquina de estados para el proceso de login:
1. DISCONNECTED → CONNECTING (iniciar conexión)
2. CONNECTING → AUTH_WAIT (esperar autenticación)
3. AUTH_WAIT → LOGGED (autenticación exitosa)
4. Cualquiera → DISCONNECTED (error)

Usa un enum y una función de transición.

### Ejercicio 3: Protocol Parser

Implementa un parser simple que maneje estos protocolos:
- 0x01: PlayerMove(X, Y)
- 0x02: PlayerAttack(targetId)
- 0x03: UseSkill(skillId)

Usa switch-case como en Protocol.cpp.