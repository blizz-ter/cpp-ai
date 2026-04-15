# Programación Dinámica

## Teoría

### Conceptos Fundamentales

Programación Dinámica (DP) resuelve problemas dividiéndolos en subproblemas:
- **Subestructura óptima**: Solución óptima contiene sub-soluciones óptimas
- **Subproblemas repetidos**: Mismos subproblemas se calculan múltiples veces

### Aproximaciones

- **Top-down con memoización**: recursion + cache
- **Bottom-up**: Construir tabla de subproblemas

### Aplicaciones típicas

- Fibonacci
- Knapsack
- Caminos más cortos
- **Daño de ataque en juegos**

---

## Código del MuServer

### Cálculo de Daño con DP

```cpp
//Archivo: DamageCalc.h
//Cálculo de daño usando programación dinámica

int CalculateDamage(OBJECTSTRUCT* lpObj, OBJECTSTRUCT* lpTarget)
{
    // daño base + bonus de stats + bonus de items
    int baseDamage = lpObj->DamageMin + 
                    (lpObj->Level * 2);
    
    // Bonus por equipment (memoization)
    static std::map<int, int> damageCache;
    
    int key = lpObj->Index;
    if(damageCache.count(key))
    {
        return baseDamage + damageCache[key];
    }
    
    // Calcular bonus de items
    int itemBonus = 0;
    for(int i = 0; i < INVENTORY_MAIN_SIZE; i++)
    {
        if(lpObj->Inventory[i].m_Index > 0)
        {
            itemBonus += lpObj->Inventory[i].m_DamageMin;
        }
    }
    
    damageCache[key] = itemBonus;
    
    return baseDamage + itemBonus;
}
```

### Memoización de Stats

```cpp
//Cache de estadísticas del jugador
class CStatsCache
{
private:
    std::map<int, int> m_DamageCache;
    std::map<int, int> m_DefenseCache;
    
public:
    int GetAttackDamage(OBJECTSTRUCT* lpObj)
    {
        int idx = lpObj->Index;
        if(m_DamageCache.count(idx))
            return m_DamageCache[idx];
            
        int damage = CalculateBaseDamage(lpObj);
        m_DamageCache[idx] = damage;
        return damage;
    }
    
    void Invalidate(int playerIndex)
    {
        m_DamageCache.erase(playerIndex);
        m_DefenseCache.erase(playerIndex);
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: DP vs Recursive

Implementa Fibonacci de dos formas:
1. Recursiva pura
2. Con memoización

Compara tiempos para n=30.

### Ejercicio 2: Daño con Cache

IMPLEMENTA CalculateDamage con cache que:
- Se invalide cuando cambia equipment
- Retorne daño base + bonus

### Ejercicio 3: Knapsack

Problema: 76 items en inventory, cada uno con weight y value.
Maximiza value con weight limit = 100.