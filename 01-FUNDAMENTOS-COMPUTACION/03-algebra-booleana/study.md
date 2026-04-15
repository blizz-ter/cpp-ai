# Álgebra Booleana

## Teoría

### Operaciones Fundamentales

El álgebra booleana opera con valores verdadero (1) y falso (0):

- **AND (&&)**: 1 si ambos son 1
- **OR (||)**: 1 si al menos uno es 1
- **NOT (!)**: Invierte el valor

### Leyes Booleanas

- **Identidad**: A AND 1 = A, A OR 0 = A
- **Nulidad**: A AND 0 = 0, A OR 1 = 1
- **Idempotencia**: A AND A = A, A OR A = A
- **Complemento**: A AND !A = 0, A OR !A = 1
- **Conmutativa**: A AND B = B AND A
- **Asociativa**: (A AND B) AND C = A AND (B AND C)
- **Distributiva**: A AND (B OR C) = (A AND B) OR (A AND C)

### Puertas Lógicas

Las puertas lógicas son circuitos que implementan operaciones booleanas:
- AND, OR, NAND, NOR, XOR, NOT

---

## Código del MuServer

### Protocol.cpp - Switch Cases

```cpp
// Archivo: Protocol.cpp
// Uso de lógica booleana para protocolos

void ProtocolHandler(BYTE protocolId, BYTE* buffer)
{
    // AND booleano para verificación
    if(buffer != NULL && size > 0)
    {
        // Procesar protocolo
    }
    
    // Uso de OR para múltiples condiciones
    if(protocolId == 0x01 || protocolId == 0x02)
    {
        // Manejar login/char list
    }
    
    // NOT para negación
    if(!gObjIsConnected(index))
    {
        // El jugador no está conectado
        return;
    }
}
```

### Lógica Booleana en Código

```cpp
// Flags y máscaras de bits
#define FLAG_VISIBLE     0x01
#define FLAG_ATTACKABLE 0x02
#define FLAG_TRADEABLE  0x04
#define FLAG_STORAGE    0x08

bool CanTrade(CItem* item)
{
    // AND de bits para verificar propiedades
    return (item->m_Flag & FLAG_TRADEABLE) != 0;
}

bool CanAttack(OBJECTSTRUCT* lpObj)
{
    // Verificar múltiples condiciones con AND
    return lpObj != NULL && 
           lpObj->Live > 0 && 
           lpObj->Connected == OBJECT_ONLINE;
}
```

### Operadores Bitwise

```cpp
// Operaciones bitwise en protocolos
BYTE CalculatePacketSize(BYTE header)
{
    // Uso de shift y mask
    return (header & 0x0F) + 2;
}

void SetFlag(BYTE* flags, BYTE flag)
{
    *flags |= flag;  // OR bitwise
}

void ClearFlag(BYTE* flags, BYTE flag)
{
    *flags &= ~flag; // AND con NOT
}

bool HasFlag(BYTE flags, BYTE flag)
{
    return (flags & flag) != 0;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Leyes Booleanas

Aplica las leyes booleanas para simplificar:
1. A AND (A OR B)
2. (A AND B) OR (A AND NOT B)
3. NOT(NOT A)

### Ejercicio 2: Flags de Items

Dado:
```cpp
#define FLAG_VISIBLE     0x01
#define FLAG_ATTACKABLE 0x02
#define FLAG_TRADEABLE  0x04
#define FLAG_STORAGE    0x08

BYTE m_Flag;  // flags del item
```

**Preguntas**:
1. Si m_Flag = 0x03, ¿qué flags están activos?
2. ¿Cómo harías para activar solo TRADEABLE?
3. ¿Cómo verificarías si tiene FLAG_ATTACKABLE?

### Ejercicio 3: Implementar Funciones

Implementa las funciones:
- `SetFlag(BYTE* flags, BYTE flag)`
- `ClearFlag(BYTE* flags, BYTE flag)`
- `ToggleFlag(BYTE* flags, BYTE flag)`