# Lógica Matemática

## Teoría

### Lógica Proposicional

La lógica proposicional trabaja con proposiciones que pueden ser verdaderas o falsas:

- **Variables proposicionales**: P, Q, R...
- **Conectivos lógicos**: AND (∧), OR (∨), NOT (¬), IMPLICA (→)
- **Tablas de verdad**: Para cada conectivo

### Leyes de la Lógica

- **Identidad**: P ∧ true = P, P ∨ false = P
- **Idempotencia**: P ∧ P = P, P ∨ P = P
- **Conmutativa**: P ∧ Q = Q ∧ P
- **Asociativa**: (P ∧ Q) ∧ R = P ∧ (Q ∧ R)
- **Distributiva**: P ∧ (Q ∨ R) = (P ∧ Q) ∨ (P ∧ R)
- **Morgan**: ¬(P ∧ Q) = ¬P ∨ ¬Q, ¬(P ∨ Q) = ¬P ∧ ¬Q

### Teoría de Conjuntos

- **Unión** (∪): Elementos en A o B
- **Intersección** (∩): Elementos en ambos
- **Complemento** (¬A): Elementos no en A
- **Diferencia** (A\B): Elementos en A pero no en B

---

## Código del MuServer

### Inventory - Tamaño de Conjuntos

```cpp
// Archivo: Item.h
// El inventario es un conjunto de items

#define INVENTORY_MAIN_SIZE 76     // 12 filas x 6 cols + 4
#define INVENTORY_WAREHOUSE_SIZE 120 // almacén
#define INVENTORY_TRADE_SIZE 16    // trade

class CInventory
{
private:
    CItem* m_Items;        // conjunto de items
    int m_Size;            // tamaño del conjunto
    
public:
    bool AddItem(CItem* item);
    bool RemoveItem(int slot);
    CItem* GetItem(int slot);
    
    int GetEmptySlotCount();   //intersección con slots vacíos
    std::vector<int> GetEmptySlots(); //conjunto de slots libres
};
```

### Conjuntos en Inventory

```cpp
// Operaciones de conjuntos
std::vector<int> GetEmptySlots()
{
    std::vector<int> empty;
    
    for(int i = 0; i < m_Size; i++)
    {
        if(m_Items[i].m_Index == 0)  // slot vacío
        {
            empty.push_back(i);
        }
    }
    return empty;
}

bool HasItem(int itemIndex)
{
    // Buscar en el conjunto de items
    for(int i = 0; i < m_Size; i++)
    {
        if(m_Items[i].m_Index == itemIndex)
        {
            return true;
        }
    }
    return false;
}
```

### Sets de Habilidades

```cpp
//Archivo: Skill.h
//Conjunto de skills activos

class CSkillSet
{
private:
    std::map<short, short> m_Skills; // skillId -> level
    
public:
    bool HasSkill(short skillId)
    {
        return m_Skills.count(skillId) > 0;
    }
    
    void AddSkill(short skillId, short level)
    {
        m_Skills[skillId] = level;
    }
    
    void RemoveSkill(short skillId)
    {
        m_Skills.erase(skillId);
    }
    
    int SkillCount()
    {
        return m_Skills.size();
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Lógica de Inventory

Dado un inventario con 76 slots:
- Slot 0-5 tienen items
- Slot 6-75 están vacíos

**Preguntas**:
1. ¿Cuál es la cardinalidad del conjunto de slots ocupados?
2. ¿Y del conjunto de slots vacíos?
3. Si A = slots ocupados, B = slots 0-15, ¿cuál es A ∩ B?

### Ejercicio 2: Implementar GetEmptySlots

Implementa la función GetEmptySlots() que retorne un vector con los índices de slots vacíos.

### Ejercicio 3: Verificación de Skills

Crea un conjunto de skills donde:
- Puedas agregar un skill
- Puedas remover un skill
- Puedas verificar si existe
- Puedas contar cuántos skills tienes