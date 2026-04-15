# Introducción a C++ - Tipos y Sintaxis Básica

## Teoría

### Tipos de Datos Fundamentales

En C++, los tipos de datos fundamentales son la base de toda la programación. Los tipos enteros como `int`, `short`, `long` y `long long` representan números enteros de diferentes rangos. Los tipos flotantes `float` y `double` manejan números con decimales. El tipo `char` almacena caracteres individuales, mientras que `bool` representa valores de verdadero o falso.

### Tipos Definidos en Windows (Win32)

El desarrollo de servidores de juegos en Windows utiliza tipos derivados de la API de Windows para garantizar compatibilidad y consistencia:

- **BYTE**: Unsigned char (0 a 255) - idéal para datos binarios y flags
- **WORD**: Unsigned short (0 a 65,535) - usado para puertos, identificadores pequeños
- **DWORD**: Unsigned long (0 a 4,294,967,295) - para timestamps, handles, flags extensos
- **QWORD**: Unsigned quad word (64 bits) - para datos grandes como experiencias de master

### Sintaxis Básica de C++

La sintaxis de C++ incluye:
- Variables: `tipo nombre;` o `tipo nombre = valor;`
- Funciones: `tipo_retorno nombre(params) { cuerpo }`
- Classes: `class Nombre { miembros };`
- Punteros: `tipo* variable;`
- Referencias: `tipo& variable;`

---

## Ejemplos en MuServer

### stdafx.h - Definición de Tipos

**Archivo**: `Source Main 5.2\source\stdafx.h`

```cpp
// Include de STL
#include <string>
#include <list>
#include <map>
#include <deque>
#include <algorithm>
#include <vector>
#include <queue>
```

### Definición de Tipos en MuServer

Los tipos BYTE, WORD, DWORD se definen en `_types.h`:

```cpp
// Tipos comunes en MuServer
BYTE m_Level;        // nivel del item (0-15)
WORD m_DamageMin;    // daño mínimo
DWORD m_Serial;      // serial único del item
QWORD MasterExperience; // experiencia de master level (64 bits)
```

### Uso de Tipos en OBJECTSTRUCT

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\User.h`

```cpp
struct OBJECTSTRUCT
{
    int Index;              // índice del objeto
    int Connected;          // estado de conexión
    BYTE ClassCode;         // código de clase
    WORD Class;             // clase del personaje
    short Level;           // nivel
    DWORD Experience;       // experiencia actual
    QWORD MasterExperience;// experiencia master
    float Life;             // vida actual
    float MaxLife;          // vida máxima
    DWORD AutoSaveTime;     // tiempo de auto-guardado
    BYTE Dir;               // dirección (0-7)
    BYTE Map;               // mapa actual
    char Account[11];       // nombre de cuenta
    char Name[11];          // nombre del personaje
    // ... muchos más campos
};
```

### Uso en CItem (Item.h)

```cpp
class CItem
{
public:
    DWORD m_Serial;         // serial único
    short m_Index;          // índice del item
    short m_Level;          // nivel del item
    BYTE m_Slot;            // slot del inventario
    BYTE m_Class;           // clase del item
    WORD m_DamageMin;       // daño mínimo
    WORD m_DamageMax;      // daño máximo
    WORD m_Defense;        // defensa
    float m_Durability;    // durabilidad actual
    // ...
};
```

### Constantes y Enumeraciones

```cpp
enum eObjectConnectState
{
    OBJECT_OFFLINE = 0,
    OBJECT_CONNECTED = 1,
    OBJECT_LOGGED = 2,
    OBJECT_ONLINE = 3,
};

enum eObjectType
{
    OBJECT_NONE = 0,
    OBJECT_USER = 1,
    OBJECT_MONSTER = 2,
    OBJECT_NPC = 3,
    OBJECT_ITEM = 5,
    OBJECT_BOTS = 6,
};

#define MAX_OBJECT 10000
#define MAX_OBJECT_USER 1000
#define MAX_CHARACTER_LEVEL 400
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar OBJECTSTRUCT

Dado el siguiente fragmento de código de `User.h`, responde:

```cpp
struct OBJECTSTRUCT
{
    int Index;
    BYTE ClassCode;
    WORD Class;
    short Level;
    DWORD Experience;
    float Life;
    float MaxLife;
    BYTE Map;
    short X;
    short Y;
};
```

**Preguntas**:
1. ¿Qué tipo usarías para almacenar el dinero del jugador (hasta 2,000,000,000)?
2. ¿Por qué `Map` es `BYTE` y no `int`?
3. ¿Qué tipo es más apropiado para coordenadas X, Y: `int` o `short`? ¿Por qué?

### Ejercicio 2: Crear una Estructura de Jugador

Crea una estructura `PlayerData` que contenga:
- Nombre del jugador (string, máximo 11 caracteres)
- Nivel (1-400)
- Experiencia (hasta 4,000,000,000)
- Vida actual y máxima
- Posición (mapa, X, Y)
- Fecha de creación (puedes usar DWORD para timestamp)

Usa los tipos apropiados (BYTE, WORD, DWORD, int, float).

### Ejercicio 3: Enum para Estados de Conexión

Crea un enumerador `ConnectionState` con los estados:
- `DISCONNECTED`
- `CONNECTING`
- `AUTHENTICATING`
- `CONNECTED`
- `PLAYING`

Luego, escribe una función que convierta el estado a string.
