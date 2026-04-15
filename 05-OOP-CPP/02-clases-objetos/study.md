# Clases y Objetos en C++

## Teoría

### Clase vs Objeto

- **Clase**: Plantilla o molde que define la estructura y comportamiento de un tipo de objeto
- **Objeto**: Instancia concreta de una clase, con su propia copia de datos

### Componentes de una Clase

```cpp
class NombreClase
{
private:           // Oculto al exterior
    // atributos
    
public:            // Accesible desde afuera
    // métodos constructores
    // métodos getters/setters
    // otros métodos
    
protected:         // Accesible por clases derivadas
    // miembros protegidos
    
public:
    // también puede haber secciones public
};
```

### Constructor y Destructor

- **Constructor**: Método especial llamado al crear un objeto
- **Destructor**: Método especial llamado al destruir un objeto
- **Constructor de copia**: Copia otro objeto
- **Operador de asignación**: Asigna valores

### Miembros de Clase

- **Atributos/Miembros**: Variables que almacenan el estado
- **Métodos**: Funciones que definen el comportamiento
- **Const member functions**: No modifican el objeto

---

## Ejemplos en MuServer

### CItem - Definición de Clase

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\Item.h`

```cpp
class CItem
{
public:
    CItem();  // Constructor
    void Clear();
    
    // Métodos de verificación
    bool IsItem();
    bool IsExcItem();
    bool IsSetItem();
    bool Is380Item();
    bool IsSocketItem();
    bool IsLuckyItem();
    bool IsPentagramItem();
    
    // Métodos de cálculo
    void Convert(int index, BYTE Option1, BYTE Option2, BYTE Option3,
                 BYTE NewOption, BYTE SetOption, BYTE JewelOfHarmonyOption,
                 BYTE ItemOptionEx, BYTE SocketOption[MAX_SOCKET_OPTION],
                 BYTE SocketOptionBonus);
    void Value();
    void OldValue();
    void PetValue();
    
    // Métodos de acceso
    int GetDamageMin();
    int GetDamageMax();
    int GetDefense();
    
    // Métodos de modificación
    bool WeaponDurabilityDown(int aIndex, int defense, int type);
    bool ArmorDurabilityDown(int aIndex, int damage);
    bool CheckDurabilityState();
    bool AddPetItemExp(int amount);
    
public:
    // Atributos
    DWORD m_Serial;
    short m_Index;
    short m_Level;
    BYTE m_Slot;
    BYTE m_Class;
    WORD m_DamageMin;
    WORD m_DamageMax;
    WORD m_Defense;
    float m_Durability;
    float m_BaseDurability;
    // ... muchos más
};
```

### Implementación del Constructor

En el archivo .cpp correspondiente:

```cpp
CItem::CItem()
{
    this->Clear();
}

void CItem::Clear()
{
    m_Serial = 0;
    m_Index = 0;
    m_Level = 0;
    m_Slot = 0xFF;
    m_Class = 0;
    m_DamageMin = 0;
    m_DamageMax = 0;
    m_Defense = 0;
    m_Durability = 0.0f;
    m_BaseDurability = 0.0f;
    m_IsValidItem = false;
    // ... inicializar todos los campos
}
```

### CPetItemExp - Clase con Tabla de Experiencia

```cpp
class CPetItemExp
{
public:
    CPetItemExp()  // Constructor
    {
        this->m_DarkSpiritExpTable[0] = 0;
        this->m_DarkSpiritExpTable[1] = 0;
        
        for(int n = 2; n < MAX_PET_LEVEL + 2; n++)
        {
            this->m_DarkSpiritExpTable[n] = ((((n + 10) * n) * n) * n) * 100;
        }
        
        this->m_DarkHorseExpTable[0] = 0;
        this->m_DarkHorseExpTable[1] = 0;
        
        for(int n = 2; n < MAX_PET_LEVEL + 2; n++)
        {
            this->m_DarkHorseExpTable[n] = ((((n + 10) * n) * n) * n) * 100;
        }
    }
    
public:
    int m_DarkSpiritExpTable[MAX_PET_LEVEL + 2];
    int m_DarkHorseExpTable[MAX_PET_LEVEL + 2];
};
```

### Instanciación y Uso

```cpp
// Crear un objeto CItem
CItem newItem;
newItem.Clear();

newItem.m_Index = 0x1B0E;  // Índice de item (Blade Knight Armor)
newItem.m_Level = 13;       // Nivel +13
newItem.m_Durability = 255.0f;

// Usar métodos
int minDmg = newItem.GetDamageMin();
int maxDmg = newItem.GetDamageMax();

if(newItem.IsItem())
{
    // El item es válido
    newItem.Value();  // Calcular valor
}
```

### Objetos Globales en MuServer

```cpp
// gObj es un array global de punteros a OBJECTSTRUCT
extern OBJECTSTRUCT_HEADER gObj;

// Acceso: gObj[aIndex] retorna referencia a OBJECTSTRUCT
LPOBJ lpObj = &gObj[aIndex];
lpObj->Life = 100.0f;
lpObj->Map = 0;

// Inventory es un puntero a array de CItem
CItem* Inventory = lpObj->Inventory;

// Acceso al item en slot 0
if(Inventory[0].IsItem())
{
    int damage = Inventory[0].GetDamageMin();
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Crear una Clase de Moneda

Crea una clase `CCoin` que gestione un tipo de moneda:

```cpp
class CCoin
{
private:
    std::string m_Name;
    int m_Amount;
    
public:
    CCoin();  // Constructor por defecto
    CCoin(const std::string& name, int initialAmount);
    
    void Add(int amount);
    bool Spend(int amount);  // Retorna false si no hay suficiente
    int GetBalance() const;
    std::string GetName() const;
};
```

### Ejercicio 2: Constructor de Copia

Explica qué hace este constructor de copia:

```cpp
class CPlayer
{
public:
    CPlayer(const CPlayer& other)
    {
        this->m_Name = other.m_Name;
        this->m_Level = other.m_Level;
        this->m_Experience = other.m_Experience;
    }
    
private:
    std::string m_Name;
    int m_Level;
    DWORD m_Experience;
};
```

¿Necesitarías un constructor de copia para `CItem`? ¿Por qué?

### Ejercicio 3: Implementar Métodos

Implementa los métodos de la clase `CCoin`:

```cpp
CCoin::CCoin()
{
    m_Name = "Zen";
    m_Amount = 0;
}

CCoin::CCoin(const std::string& name, int initialAmount)
{
    m_Name = name;
    m_Amount = initialAmount;
}

void CCoin::Add(int amount)
{
    // Completar: agregar amount, con límite máximo de 2000000000
}

bool CCoin::Spend(int amount)
{
    // Completar: verificar si hay suficiente, restar y retornar true/false
}

int CCoin::GetBalance() const
{
    // Completar: retornar balance
}
```

### Ejercicio 4: Análisis de OBJECTSTRUCT

`OBJECTSTRUCT` en MuServer es una estructura (struct), no una clase. Responde:
1. ¿Qué diferencia hay entre struct y class en C++?
2. ¿Por qué crees que usaron struct en lugar de class?
3. Si fuera una clase, ¿qué miembros deberían ser privados?
4. ¿Qué métodos añadirías a `OBJECTSTRUCT` si lo convirtieras en clase?

### Ejercicio 5: Crear Inventory con Clase

Diseña una clase `CInventory` que gestione el inventario de un jugador:

```cpp
class CInventory
{
private:
    CItem* m_Items;
    int m_Size;
    
public:
    CInventory(int size);
    ~CInventory();
    
    bool AddItem(CItem& item, int slot);
    CItem* GetItem(int slot);
    bool RemoveItem(int slot);
    int GetEmptySlot();
    int GetItemCount();
};
```
