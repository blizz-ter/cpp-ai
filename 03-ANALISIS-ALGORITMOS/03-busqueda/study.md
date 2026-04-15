# Algoritmos de Búsqueda

## Teoría

### Tipos de Búsqueda

- **Linear**: O(n) - revisar uno por uno
- **Binary**: O(log n) - requiere array ordenado
- **Hash**: O(1) promedio - usar tabla hash
- **Interpolation**: O(log log n) promedio - datos uniformes

### Binary Search

Para datos ordenados:
1. Calcular mid = (low + high) / 2
2. Si target < mid, buscar en izquierda
3. Si target > mid, buscar en derecha
4. Repetir hasta encontrar o low > high

---

## Código del MuServer

### gObjFind - Linear Search

```cpp
//Archivo: User.cpp
//Búsqueda lineal O(n)
int gObjFind(int aIndex)
{
    if(aIndex < 0 || aIndex >= MAX_OBJECT)
        return -1;
    
    if(gObj[aIndex].Connected == OBJECT_ONLINE)
        return aIndex;
    
    return -1;
}
```

### gObjFindName - Búsqueda por Nombre

```cpp
//Búsqueda por nombre - O(n)
int gObjFindName(char* name)
{
    for(int n = 0; n < MAX_OBJECT; n++)
    {
        if(gObj[n].Connected == OBJECT_ONLINE)
        {
            if(strcmp(gObj[n].Name, name) == 0)
            {
                return n;
            }
        }
    }
    return -1;
}
```

### Búsqueda de Item en Inventory

```cpp
//Búsqueda item en inventory - O(n)
int FindItemInInventory(CItem* inventory, int size, short itemId)
{
    for(int i = 0; i < size; i++)
    {
        if(inventory[i].m_Index == itemId)
        {
            return i;
        }
    }
    return -1;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Búsqueda

Para gObj con 10000 objetos:
- 5000 players online
- Buscar por nombre "Admin"

**Preguntas**:
1. ¿Cuántas iteraciones en worst case?
2. ¿Cuántas en average case?

### Ejercicio 2: Implementar Binary Search

Implementa búsqueda binaria para un array ordenado de ints.

### Ejercicio 3: Hash Table Lookup

Cómo implementarías una búsqueda O(1) de jugadores por nombre?