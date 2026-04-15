# Análisis Profundo: Sistema de Items

## Estructura CItem

En `Item.h` defines the item class:

```cpp
class CItem {
public:
    int m_Index;          // Index global
    int m_Type;          // Type (ItemType * 512 + ItemIndex)
    int m_Level;         // Level (0-13)
    int m_Part;          // Slot de equipo
    int m_Class;         // Clase válida
    BYTE m_X;            // Slot X en inventario
    BYTE m_Y;            // Slot Y en inventario
    BYTE m_Z;            (deprecated)
    BYTE m_Enc;           // Excelent options
    BYTE m_NewOption;     // Options adicionales
    BYTE m_Option1;
    BYTE m_Option2;
    BYTE m_Option3;
    int m_Durability;    // Durabilidad
    BYTE m_Number;       // Número series
    int m_Slot;          // Slot actual

    // Métodos
    bool IsItem();       // Verifica es item válido
    void Clear();        // Limpia el item
    // ...
};
```

---

## Tipos de Items

| Category | Type Range | Descripción |
|----------|------------|-------------|
| Weapons | 0-49 | Espadas, arcos, etc |
| Shields | 50-55 | Escudos |
| Armors | 64-138 | Armaduras |
| Rings | 106-111 | Anillos |
| Pendants | 0-20 | Collares (tipo 12) |
| Pets | 70-90 | Pets |
| Wings | 0-63 | Alas |
| Custom | Varios | Items custom |

---

## Inventory

Tamaños de inventario definidos en stdafx.h:

```cpp
#define INVENTORY_WEAR_SIZE 12    // Slots de equipo
#define INVENTORY_MAIN_SIZE 76  // Inventario base
#define INVENTORY_FULL_SIZE 236  // Fullinventory (S5+)
#define WAREHOUSE_SIZE 240       // Almacén
#define CHAOS_BOX_SIZE 32        // Caja del Caos

// Extended
#define MAX_EXTENDED_INV 2        // # de extensiones
#define EXTENDED_INV_SIZE 32     // Slots por extensión
```

---

## Opciones de Items

###Excellent Options (+4, +5, +6):

```cpp
// m_Enc - Excelent options bitfield
#define EXCELLENT_DAMAGE        0x01
#define EXCELLENT_ATTACK_SPEED  0x02
#define EXCELLENT_DAMAGE2      0x04
#define EXCELLENT_CRITICAL   0x08
#define EXCELLENT_MANA         0x10
#define EXCELLENT_LIFE         0x20
#define EXCELLENT_BP           0x40
```

###380 Items (Socket):

```cpp
// Cuando m_Type tiene tipo 5+12*ItemIndex + 512*ItemType = item380
// m_NewOptions = optionsocket
#define OPTION_380_1 0x01
#define OPTION_380_2 0x02
#define OPTION_380_3 0x04
#define OPTION_380_4 0x08
#define OPTION_380_5 0x10
```

###Set Bonus:

Itemsset dan bonificación completocuando todos los items tienen el mismo Index:

| Set | Bonus |
|-----|-------|
| Complete set | +N skills |

---

## Sistema de Mezclas

###CustomMix (CustomMix.cpp):

Mezclas personalizadas configuradas en Archivo:

- **Angel/Rare/Secret items**
- **Wing Mix**
- **Jewel Mix**
- **Potion Mix**

### ItemPrice calculation:

```cpp
// ItemManager.cpp
Price = BasePrice * (Level + 9) / 10;
// + opciones excellent
// + luck
// + skill
```

---

## Problemas

### 1. No hay validación de nivel

```cpp
// El clienteenvía nivel, servidor no valida
// Posible exploited!
```

### 2. No hay validación de opciones

```cpp
// Cliente puede enviar opciones quals
// Debería validarserver-side
```

### 3. Inventario puede explotarse

```cpp
// Sin check de overlap en move
// Sin validación de durablidad
```

---

## Faltante (según README)

| Sistema | Estado |
|---------|--------|
| Extended Inventory | ❌ No implementado |
| Extended Chest | ❌ No implementado |

---

*Análisis de Items - MuServer S5U15*