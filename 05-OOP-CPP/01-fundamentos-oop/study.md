# Fundamentos de Programación Orientada a Objetos (OOP)

## Teoría

### ¿Qué es la Programación Orientada a Objetos?

La Programación Orientada a Objetos (OOP) es un paradigma de programación que organiza el código alrededor de "objetos" que representan entidades del mundo real. Cada objeto combina datos (atributos/propiedades) y comportamiento (métodos/funciones).

### Pilares de la OOP

1. **Encapsulamiento**: Agrupar datos y métodos que operan sobre esos datos en una sola unidad (clase), ocultando la implementación interna.
2. **Abstracción**: Ocultar los detalles complejos de implementación y mostrar solo las funcionalidades esenciales.
3. **Herencia**: Permite que una clase herede atributos y métodos de otra clase.
4. **Polimorfismo**: La capacidad de objetos de diferentes clases de ser tratados como objetos de una clase común.

### Beneficios de la OOP

- **Modularidad**: Código más organizado y mantenible
- **Reusabilidad**: A través de la herencia y composición
- **Flexibilidad**: Polimorfismo permite escribir código genérico
- **Ocultamiento de información**: Protege los datos internos
- **Facilidad de mantenimiento**: Cambios locales no afectan todo el sistema

---

## Ejemplos en MuServer

### Clase CItem - Encapsulamiento

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\Item.h`

```cpp
class CItem
{
public:
    CItem();
    void Clear();
    bool IsItem();
    bool IsExcItem();
    bool IsSetItem();
    bool Is380Item();
    bool IsSocketItem();
    bool IsLuckyItem();
    bool IsPentagramItem();
    bool IsPentagramJewel();
    bool IsPentagramMithril();
    bool IsEventItem();
    bool IsMuunItem();
    bool IsMuunUtil();
    bool IsClass(int Class, int ChangeUp);
    void Convert(int index, BYTE Option1, BYTE Option2, BYTE Option3, 
                 BYTE NewOption, BYTE SetOption, BYTE JewelOfHarmonyOption,
                 BYTE ItemOptionEx, BYTE SocketOption[MAX_SOCKET_OPTION],
                 BYTE SocketOptionBonus);
    void Value();
    void OldValue();
    void PetValue();
    void SetPetItemInfo(int PetLevel, int PetExp);
    int GetDamageMin();
    int GetDamageMax();
    int GetDefense();
    int GetDefenseSuccessRate();
    int GetBookSuccessRate();
    bool WeaponDurabilityDown(int aIndex, int defense, int type);
    bool ArmorDurabilityDown(int aIndex, int damage);
    bool WingDurabilityDown(int aIndex, int decrease);
    bool PendantDurabilityDown(int aIndex, int decrease);
    bool RingDurabilityDown(int aIndex, int decrease);
    bool LuckyDurabilityDown(int aIndex, int decrease);
    bool CheckDurabilityState();
    bool AddPetItemExp(int amount);
    bool DecPetItemExp(int amount);

public:
    DWORD m_Serial;
    short m_Index;
    short m_Level;
    BYTE m_Slot;
    BYTE m_Class;
    BYTE m_TwoHand;
    BYTE m_AttackSpeed;
    BYTE m_WalkSpeed;
    WORD m_DamageMin;
    WORD m_DamageMax;
    WORD m_DefenseSuccessRate;
    WORD m_Defense;
    WORD m_MagicDefense;
    BYTE m_Speed;
    float m_Durability;
    float m_BaseDurability;
    int m_Value;
    // ... más atributos
};
```

### Análisis del Ejemplo

- **Encapsulamiento**: Los datos como `m_Serial`, `m_Index`, `m_Level` son miembros de la clase
- **Métodos públicos**: `IsItem()`, `IsExcItem()`, `Value()` exponen funcionalidad
- **Abstracción**: El usuario no necesita saber cómo se calcula el daño, solo llama `GetDamageMin()`
- **Miembros públicos vs privados**: Datos públicos (sin encapsulamiento real en este caso histórico)

### Abstracción - Métodos de Verificación

```cpp
// El usuario simplemente llama:
CItem item;
if(item.IsItem()) {
    int damage = item.GetDamageMin();
}

// No necesita saber cómo se determina si es un item válido
// ni cómo se calcula el daño mínimo
```

### OBJECTSTRUCT - Estructura de Datos

```cpp
struct OBJECTSTRUCT
{
    // Datos del personaje
    int Index;
    int Connected;
    char Account[11];
    char Name[11];
    WORD Class;
    short Level;
    DWORD Experience;
    float Life;
    float MaxLife;
    float Mana;
    float MaxMana;
    
    // Comportamiento (a través de funciones que reciben el struct)
    // gObjSetPosition(aIndex, x, y);
    // gObjTeleport(aIndex, map, x, y);
    // gObjAddMsgSend(lpObj, MsgCode, SendUser, SubCode);
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Identificar los Pilares

Para cada ejemplo, identifica qué pilar de OOP se demuestra:

```cpp
// Ejemplo 1
class CPlayer
{
private:
    int m_Health;
    
public:
    void TakeDamage(int damage)
    {
        m_Health -= damage;
        if(m_Health < 0) m_Health = 0;
    }
    
    int GetHealth() { return m_Health; }
};
```

```cpp
// Ejemplo 2
class CCharacter { };

class CWizard : public CCharacter { };
class CKnight : public CCharacter { };
class CElf : public CCharacter { };
```

```cpp
// Ejemplo 3
class CItem
{
public:
    bool IsValid() { return m_Index > 0; }
};
```

### Ejercicio 2: Diseñar una Clase

Diseña una clase `CCurrency` que gestione las monedas de un jugador:
- Tipos: Zen, Coin1, Coin2, Coin3
- Métodos: Add, Spend, GetBalance
- Restricciones: No se puede gastar más de lo que se tiene

Identifica qué partes son:
- Atributos
- Métodos públicos
- Encapsulamiento

### Ejercicio 3: Analizar Código Real

Analiza la clase `CItem` de MuServer:
1. ¿Qué atributos tiene?
2. ¿Qué métodos de verificación existen?
3. ¿Qué información está encapsulada vs expuesta?
4. ¿Cómo se relacionaría con un `OBJECTSTRUCT`?

### Ejercicio 4: Reflexión

En MuServer, `OBJECTSTRUCT` es una estructura, no una clase. ¿Qué diferencias prácticas hay? ¿Debería convertirse en clase? ¿Por qué sí o por qué no?
