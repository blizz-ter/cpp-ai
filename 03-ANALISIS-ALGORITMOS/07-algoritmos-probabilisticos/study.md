# Algoritmos Probabilísticos

## Teoría

### Introducción

Los algoritmos probabilísticos usan randomness para obtener mejores resultados promedio.

### Tipos

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| Monte Carlo | Probabilidad de error | Primality test |
| Las Vegas | Siempre correcto, tiempo variable | QuickSort randomized |
| Atlantic City | Entre ambos | |

### Randomized QuickSort

```cpp
int randomPartition(int arr[], int low, int high) {
    srand(time(nullptr));
    int random = low + rand() % (high - low);
    swap(arr[random], arr[high]);
    return partition(arr, low, high);
}

void randomizedQuickSort(int arr[], int low, int high) {
    if(low < high) {
        int pi = randomPartition(arr, low, high);
        randomizedQuickSort(arr, low, pi - 1);
        randomizedQuickSort(arr, pi + 1, high);
    }
}
```

### Probabilistic Data Structures

```cpp
class SkipList {
private:
    struct Node {
        int key;
        std::vector<Node*> forward;
    };
    float probability;
    
public:
    int search(int key);
    void insert(int key);
};
```

## Código del MuServer

### Random Spawn System

```cpp
void CMasterLevel::SpawnMonsterRandom(int mapId, int count) {
    for(int i = 0; i < count; i++) {
        int x = rand() % MAP_WIDTH;
        int y = rand() % MAP_HEIGHT;
        // Spawn en posición aleatoria
    }
}
```

## Ejercicio

1. Implementar QuickSort randomizado
2. Comparar con QuickSort determinista
3. Medir tiempo promedio