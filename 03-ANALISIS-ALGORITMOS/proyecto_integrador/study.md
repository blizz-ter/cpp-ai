# Proyecto Integrador: Análisis de Algoritmos

## Descripción del Proyecto

Analizar y optimizar algoritmos de sorting y búsqueda usados en el servidor.

## Requisitos

- Analizar complexity de algoritmos existentes
- Proponer optimizaciones
- Implementar mejoras

## Algoritmos a Analizar

### Sorting de Jugadores por Level

```cpp
// Algoritmo actual en el servidor
void SortPlayersByLevel(OBJECTSTRUCT* players, int count) {
    for(int i = 0; i < count; i++) {
        for(int j = i + 1; j < count; j++) {
            if(players[i].m_Level < players[j].m_Level) {
                swap(players[i], players[j]);
            }
        }
    }
}
```

### Búsqueda de Items

```cpp
ITEM* FindItemByIndex(ITEM* inventory, int count, int index) {
    for(int i = 0; i < count; i++) {
        if(inventory[i].m_Index == index) {
            return &inventory[i];
        }
    }
    return nullptr;
}
```

## Análisis Requerido

| Algoritmo | Complexity Actual | Complexity Optimizada |
|----------|-----------------|---------------------|
| SortPlayers | O(n²) | |
| FindItem | O(n) | |

## Ejercicios

1. Calcular Big-O del algoritmo actual
2. Proponer mejora con QuickSort/Binary Search
3. Implementar y medir performance

## Entregable

1. Documento de análisis
2. Implementación optimizada
3. Benchmark comparativo