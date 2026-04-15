# Sistemas Numéricos

## Teoría

### Sistema Binario

El sistema binario (base 2) usa solo dígitos 0 y 1. Cada posición representa una potencia de 2:
- Bit más significativo (MSB): 2^n-1
- Bit menos significativo (LSB): 2^0 = 1

**Conversión decimal a binario**: Dividir sucesivamente por 2, tomar residuos.
**Conversión binario a decimal**: Sumar 2^n para cada bit activo.

### Sistema Hexadecimal

Hexadecimal (base 16) usa 0-9 y A-F. Cada dígito representa 4 bits:
- 0x0 = 0000, 0xF = 1111
- Eficiente para representar bytes y direcciones de memoria

### Tipos en stdafx.h

**Archivo**: `Source Main 5.2\source\stdafx.h`

```cpp
// Tipos de Windows (definidos en windows.h)
#include <windows.h>

// Estos son los tipos fundamentales usados en MuServer:
// BYTE  - unsigned char (0-255)
// WORD  - unsigned short (0-65535)
// DWORD - unsigned long (0-4294967295)
// QWORD - unsigned __int64 (64 bits)

// Ejemplos de valores hexadecimales en código
#define MAX_MAP 64          // 0x40
#define MAX_ITEM_INDEX 5055 // 0x13C7
#define MAX_USER 1000       // 0x3E8
```

### Uso de Hex en MuServer

```cpp
// Flags y protocolos en hex
#define PROTO_LOGIN        0x01
#define PROTO_CHARLIST     0x02
#define PROTO_CREATECHAR   0x03
#define PROTO_MOVE       0x10
#define PROTO_ATTACK     0x11

// Direcciones de memoria (debug)
#define DEBUG_ADDR       0x00401000

// Más ejemplos
BYTE buffer[1024];
WORD packetHeader = 0xC1;  // paquete de encabezado
DWORD address = 0x00403000; // dirección de función
```

---

## Código del MuServer

### stdafx.h - Definiciones de Tipos

```cpp
// stdafx.h incluye los tipos Windows
#include <windows.h>

// Tipos STL
#include <string>
#include <vector>
#include <map>
#include <list>
```

### Item.h - Campos Numéricos

```cpp
// Archivo: Item.h
class CItem
{
public:
    short m_Index;      // índice del item
    BYTE m_Level;      // nivel (0-15)
    WORD m_DamageMin;   // daño mínimo
    WORD m_DamageMax;   // daño máximo
    WORD m_Defense;    // defensa
    BYTE m_Skill;     // skill del item
    BYTE m_Slot;      // slot en inventario
};
```

### Enums con Valores Hex

```cpp
// Estados de conexión
enum eConnectState 
{
    eConnectState_Disconnected = 0,  // 0x00
    eConnectState_Connected = 1,      // 0x01
    eConnectState_Logged = 2,         // 0x02
    eConnectState_Playing = 3,         // 0x03
};

// Protocolos de mensaje
enum eMessageProtocol 
{
    MSGPROTOCOL_GS_CONN = 0x01,
    MSGPROTOCOL_GS_CHAR = 0x02,
    MSGPROTOCOL_GS_CREATE = 0x03,
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Conversión de Sistemas

Resuelve:
1. Convierte 255 a binario y hexadecimal
2. Convierte 0xFF a decimal y binario
3. Convierte 11110000b a decimal y hex
4. ¿Cuántos bits necesito para 1000?

### Ejercicio 2: Analizar Tipos

Dado:
```cpp
BYTE m_Level;      // nivel del item (max 15)
WORD m_DamageMin;  // daño mínimo
DWORD m_Serial;   // serial único
```

**Preguntas**:
1. ¿Cuál es el valor máximo de m_Level?
2. ¿Cuántos bytes ocupa m_DamageMin?
3. ¿Cuántos bytes ocupa m_Serial?

### Ejercicio 3: Protocolos Hex

Crea un enum de protocolos para un juego con:
- Login: 0x01
- Logout: 0x02
- Chat: 0x10
- Trade: 0x20
- Move: 0x30
- Attack: 0x40

Usa valores hexadecimales.