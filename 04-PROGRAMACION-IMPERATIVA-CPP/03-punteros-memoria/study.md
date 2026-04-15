# Punteros y Gestión de Memoria en C++

## Teoría

### ¿Qué es un Puntero?

Un puntero es una variable que almacena la dirección de memoria de otra variable. Los punteros son fundamentales en C++ para:
- Asignación dinámica de memoria
- Paso de parámetros por referencia
- Manipulación de arrays
- Implementación de estructuras de datos complejas
- Interfaz con APIs de sistemas

### Operadores de Punteros

- `&` (address-of): Obtiene la dirección de memoria de una variable
- `*` (dereferencia): Accede al valor almacenado en la dirección que apunta el puntero

### Tipos de Punteros

- **Punteros simples**: `tipo* ptr`
- **Punteros const**: `const tipo* ptr` (no puedes modificar el valor apontado)
- **Punteros a const**: `tipo* const ptr` (no puedes modificar el puntero)
- **Punteros inteligentes**: `std::unique_ptr`, `std::shared_ptr` (manejo automático de memoria)

### Gestión de Memoria

- `new`: Asigna memoria dinámica
- `delete`: Libera memoria asignada con new
- `new[]` / `delete[]`: Para arrays

---

## Ejemplos en MuServer

### Punteros en OBJECTSTRUCT

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\User.h`

```cpp
struct OBJECTSTRUCT
{
    // Punteros a otras estructuras
    struct PER_SOCKET_CONTEXT* PerSocketContext;
    SOCKET Socket;
    
    // Punteros a arrays de viewport
    VIEWPORT_STRUCT* VpPlayer;
    VIEWPORT_STRUCT* VpPlayer2;
    VIEWPORT_STRUCT* VpPlayerItem;
    
    // Punteros a HitDamage
    HIT_DAMAGE_STRUCT* HitDamage;
    
    // Punteros a Inventory (gestión de items)
    CItem* Inventory;
    CItem* Inventory1;
    CItem* Inventory2;
    BYTE* InventoryMap;
    
    // Punteros a Trade, Warehouse, ChaosBox
    CItem* Trade;
    BYTE* TradeMap;
    CItem* Warehouse;
    BYTE* WarehouseMap;
    CItem* ChaosBox;
    BYTE* ChaosBoxMap;
    
    // Punteros a Skills
    CSkill* SkillBackup;
    CSkill* Skill;
    CSkill* MasterSkill;
    
    // Punteros a Effects
    CEffect* Effect;
    
    // Puntero al objeto que ataca
    OBJECTSTRUCT* AttackObj;
    
    // Punteros a Guild
    struct GUILD_INFO_STRUCT* Guild;
    // ...
};
```

### Uso de Punteros con Operador []

El sistema de objetos de MuServer usa un patrón de array de punteros:

```cpp
// Definición del array global de objetos
struct OBJECTSTRUCT_HEADER
{
    OBJECTSTRUCT_HEADER()
    {
        this->CommonStruct = new OBJECTSTRUCT;
        
        for(int n=0; n < MAX_OBJECT; n++)
        {
            this->ObjectStruct[n] = this->CommonStruct;
        }
    }
    
    OBJECTSTRUCT& operator[](int index)
    {
        return (*this->ObjectStruct[index]);
    }
    
    OBJECTSTRUCT* CommonStruct;
    OBJECTSTRUCT* ObjectStruct[MAX_OBJECT];
};

extern OBJECTSTRUCT_HEADER gObj;
```

### Acceso a Objetos

```cpp
// gObj es un array global de punteros a OBJECTSTRUCT
// Para acceder a un jugador específico:
LPOBJ lpObj = &gObj[aIndex];  // donde aIndex es el índice del jugador

// Acceso a campos del objeto
lpObj->Life = 100.0f;
lpObj->Map = 0;
lpObj->X = 100;
lpObj->Y = 100;

// Verificar si el objeto está conectado
if(lpObj->Connected == OBJECT_ONLINE)
{
    // El jugador está online
}
```

### Punteros a Inventory

```cpp
// En User.h
CItem* Inventory;      // inventario principal
CItem* Inventory1;      // inventario extendido 1
CItem* Inventory2;      // inventario extendido 2
BYTE* InventoryMap;     // mapa de slots ocupados

// Asignación de memoria para inventory
void gObjSetInventory1Pointer(LPOBJ lpObj)
{
    lpObj->Inventory = new CItem[INVENTORY_MAIN_SIZE];
    lpObj->InventoryMap = new BYTE[INVENTORY_MAIN_SIZE];
}

// Liberación de memoria
void gObjClearInventory(LPOBJ lpObj)
{
    if(lpObj->Inventory)
    {
        delete[] lpObj->Inventory;
        lpObj->Inventory = nullptr;
    }
    if(lpObj->InventoryMap)
    {
        delete[] lpObj->InventoryMap;
        lpObj->InventoryMap = nullptr;
    }
}
```

### MESSAGE_STATE_MACHINE con Punteros

```cpp
struct MESSAGE_STATE_MACHINE
{
    MESSAGE_STATE_MACHINE()
    {
        this->Clear();
    }
    
    void Clear()
    {
        this->MsgCode = -1;
        this->SendUser = -1;
        this->MsgTime = 0;
        this->SubCode = 0;
    }
    
    int MsgCode;
    int SendUser;
    int MsgTime;
    int SubCode;
};

struct MESSAGE_STATE_MACHINE_COMMON
{
    MESSAGE_STATE_MACHINE_COMMON()
    {
        this->CommonStruct = new MESSAGE_STATE_MACHINE;
        
        for(int n=0; n < MAX_MONSTER_SEND_MSG; n++)
        {
            this->ObjectStruct[n] = this->CommonStruct;
        }
    }
    
    MESSAGE_STATE_MACHINE& operator[](int index)
    {
        return (*this->ObjectStruct[index]);
    }
    
    MESSAGE_STATE_MACHINE* CommonStruct;
    MESSAGE_STATE_MACHINE* ObjectStruct[MAX_MONSTER_SEND_MSG];
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Uso de Punteros

Dado el siguiente código de `User.h`:

```cpp
CItem* Inventory;
BYTE* InventoryMap;
OBJECTSTRUCT* AttackObj;
```

**Preguntas**:
1. ¿Qué representa `Inventory` en el contexto del servidor?
2. ¿Por qué se usa un puntero en lugar de un array estático para `Inventory`?
3. ¿Qué función cumple `InventoryMap`?
4. ¿Qué información contiene `AttackObj`?

### Ejercicio 2: Implementar una Inventory Simple

Crea una clase `SimpleInventory` que:
1. Use un puntero `CItem* m_Items` para almacenar items
2. Tenga un método `Allocate(int size)` que asigne memoria
3. Tenga un método `Deallocate()` que libere memoria
4. Tenga un método `GetItem(int index)` que retorne referencia al item

```cpp
class SimpleInventory
{
private:
    CItem* m_Items;
    int m_Size;
    
public:
    SimpleInventory();
    ~SimpleInventory();
    
    void Allocate(int size);
    void Deallocate();
    CItem& GetItem(int index);
    bool IsAllocated();
};
```

### Ejercicio 3: Punteros y Referencias

Explica la diferencia entre estos tres códigos y cuándo usarías cada uno:

```cpp
// Versión 1: Puntero
void SetLife(LPOBJ lpObj, float life)
{
    lpObj->Life = life;
}

// Versión 2: Referencia
void SetLife(OBJECTSTRUCT& obj, float life)
{
    obj.Life = life;
}

// Versión 3: Puntero a puntero
void CreateObject(OBJECTSTRUCT** ppObj)
{
    *ppObj = new OBJECTSTRUCT;
}
```

### Ejercicio 4: Memory Leak Detection

Si olvidáramos hacer `delete` en el inventory, ¿qué pasaría? Identifica en el código de MuServer dónde se asigna y dónde se debería liberar la memoria del inventory.
