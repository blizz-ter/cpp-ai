# Análisis Profundo: Sistema de Objetos (OBJECTSTRUCT)

## Estructura Principal del Jugador

En `User.h:468` se define la estructura principal:

```cpp
struct OBJECTSTRUCT {
    // === Identificación ===
    int Index;              // Índice en el array gObj
    int Connected;         // Estado: 0=OFFLINE, 1=CONNECTED, 2=LOGGED, 3=ONLINE
    SOCKET Socket;         // Socket de red
    char IpAddr[16];       // IP del jugador
    char Account[11];      // Nombre de cuenta
    char Name[11];         // Nombre del personaje
    char PersonalCode[14]; // Código personal

    // === Clase ===
    WORD Class;           // Clase del personaje
    BYTE DBClass;        // Clase de BD
    BYTE ChangeUp;        // Evolución: 0=Normal, 1=DK, 2=ME, 3=LE

    // === Nivel y Experiencia ===
    short Level;          // Nivel (1-400)
    WORD LevelUpPoint;    // Puntos disponibles
    DWORD Experience;    // Experiencia actual
    DWORD NextExperience; // XP para siguiente nivel
    WORD FruitAddPoint;  // Puntos de fruta acumulados
    WORD FruitSubPoint;  // Puntos de fruta usados

    // === Stats ===
    int Strength;        // Fuerza
    int Dexterity;       // Destreza
    int Vitality;        // Vitalidad
    int Energy;          // Energía
    int Leadership;     // Liderazgo (solo DL)

    // === Stats Extra ===
    int AddStrength;
    int AddDexterity;
    int AddVitality;
    int AddEnergy;
    int AddLeadership;

    // === Vida y Mana ===
    float Life, MaxLife;
    float ScriptMaxLife;
    float Mana, MaxMana;
    float VitalityToLife;   // Vitality → Life ratio
    float EnergyToMana;      // Energy → Mana ratio

    // === Shield (S5+) ===
    int Shield;
    int MaxShield;
    int AddShield;

    // === BP (Stamina) ===
    int BP, MaxBP, AddBP;

    // === Posición ===
    short X, Y;            // Coordenadas actuales
    BYTE Dir;              // Dirección (0-7)
    BYTE Map;              // ID del mapa
    short OldX, OldY;      // Coordenadas anteriores
    short TX, TY;         // Target posición
    BYTE StartX, StartY;  // Spawn posición

    // === PK ===
    char PKCount;
    char PKLevel;          // PK Level (0=Purple, 1-5=Rojo)
    int PKTime;           // Tiempo PK

    // === Chat ===
    WORD ChatLimitTime;
    BYTE ChatLimitTimeSec;

    // === Inventario ===
    // Punteros a arrays de CItem
};
```

---

## Límites del Sistema

En `User.h`:

```cpp
#define MAX_OBJECT           10000  // Total objetos en memoria
#define MAX_OBJECT_USER     1000   // Jugadores máximos
#define MAX_OBJECT_MONSTER 8000   // Monstruos
#define MAX_OBJECT_BOTS      200    // Bots

#define MAX_CHARACTER_LEVEL  400    // Nivel máximo
#define MAX_MONEY          2000000000  // 2B Zen máximo
#define MAX_ACCOUNT_LEVEL  4       // GM levels

#define MAX_VIEWPORT       75     // Objetos visibles
#define MAX_GUILD_USER    80     // Miembros por guild
```

---

## Rangos de Índices

```cpp
#define OBJECT_START_BOTS       0
#define OBJECT_START_MONSTER   0
#define OBJECT_START_USER     (MAX_OBJECT - MAX_OBJECT_USER)
```

---

## Macros de Verificación

```cpp
#define OBJECT_RANGE(x)         // Verifica índice válido
#define OBJECT_MONSTER_RANGE(x) // Verifica es monstruo
#define OBJECT_USER_RANGE(x)     // Verifica es jugador
#define CHECK_RANGE(x,y)        // Verifica rango
```

---

## Estados de Conexión

```cpp
enum eObjectConnectState {
    OBJECT_OFFLINE = 0,
    OBJECT_CONNECTED = 1,
    OBJECT_LOGGED = 2,
    OBJECT_ONLINE = 3,
};
```

---

## Tipos de Objeto

```cpp
enum eObjectType {
    OBJECT_USER = 1,
    OBJECT_MONSTER = 2,
    OBJECT_NPC = 3,
    // ...
};
```

---

## CItem (Items)

La clase `CItem` gestionaitems:

```cpp
class CItem {
public:
    int m_Index;
    int m_Type;
    int m_Level;
    int m_Part;
    int m_Class;
    BYTE m_X;
    BYTE m_Y;
    BYTE m_Z;
    BYTE m_Enc;
    BYTE m_NewOption;
    BYTE m_Option1;
    BYTE m_Option2;
    BYTE m_Option3;
    int m_Durability;
    BYTE m_Number;
    int m_Slot;

    // Métodos principales
    bool IsItem();
    void Clear();
};
```

---

## CSkill (Skills)

```cpp
class CSkill {
public:
    int m_Skill;
    int m_Level;
    int m_SP;
    int m_Caster;
    BYTE m_Durability;
    BYTE m_Number;
    // ...
};
```

---

## Problemas Identificados

### 1. Arrays Fijos en Estructura

```cpp
// En OBJECTSTRUCT, inventario es un puntero, no array fijo
// Debe allocarse con new en tiempo de ejecución
CItem* Inventory;      // new CItem[INVENTORY_SIZE]
```

### 2. Memory Leaks Potenciales

```cpp
// Muchos new sin delete匹配的 en:
// - ObjectManager.cpp
// - User.cpp
// - Items
```

### 3. Sin Smart Pointers

Todo el código usa raw pointers, sin smart pointers.

---

*Análisis de Objetos - MuServer S5U15*