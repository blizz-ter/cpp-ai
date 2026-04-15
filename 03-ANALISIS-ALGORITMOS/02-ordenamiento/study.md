# Algoritmos de Ordenamiento

## Teoría

### Algoritmos Comunes

- **Bubble Sort**: O(n²) - simple pero lento
- **Insertion Sort**: O(n²), bueno para datos casi ordenados
- **Selection Sort**: O(n²)
- **Quick Sort**: O(n log n) promedio
- **Merge Sort**: O(n log n) - estable
- **std::sort**: O(n log n) - STL

### Estabilidad

Un algoritmo estable mantiene el orden relativo de elementos iguales:
- Estables: Bubble, Insertion, Merge
- No estables: Quick, Selection

---

## Código del MuServer

### Inventory Sort - QuickSort

```cpp
// Ordenar inventario por nivel
void SortInventory(CItem* inventory, int size)
{
    std::sort(inventory, inventory + size, 
        [](const CItem& a, const CItem& b)
        {
            return a.m_Level > b.m_Level;
        });
}
```

### Sort por Nivel de Item

```cpp
// Usando std::sort con comparador
void SortInventoryByLevel(CItem* items, int count)
{
    for(int i = 0; i < count; i++)
    {
        for(int j = i + 1; j < count; j++)
        {
            if(items[i].m_Level < items[j].m_Level)
            {
                CItem temp = items[i];
                items[i] = items[j];
                items[j] = temp;
            }
        }
    }
}
```

### MergeSort para Rankings

```cpp
void MergeSortPlayer(std::vector<OBJECTSTRUCT*>& players)
{
    std::sort(players.begin(), players.end(),
        [](OBJECTSTRUCT* a, OBJECTSTRUCT* b)
        {
            return a->Level > b->Level;
        });
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Sort

Para un inventario de 76 items:
1. ¿Cuántas comparaciones hace Bubble Sort?
2. ¿Cuántas hace Quick Sort (promedio)?

### Ejercicio 2: Implementar Bubble Sort

Implementa Bubble Sort para un array de ints.

### Ejercicio 3: Ordenar por Múltiples Campos

Ordena primero por nivel (desc), luego por nombre (asc).