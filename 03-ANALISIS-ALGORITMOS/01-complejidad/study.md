# Complejidad Algorítmica

## Teoría

### Notación Big-O

Big-O describe el límite superior del tiempo de un algoritmo:
- **O(1)**: Constante
- **O(log n)**: Logarítmico
- **O(n)**: Lineal
- **O(n log n)**: Linearítmico
- **O(n²)**: Cuadrático
- **O(2^n)**: Exponencial

### Análisis de Complejidad

Para analizar un algoritmo:
1. Identificar la entrada (n)
2. Contar operaciones básicas
3. Expresar en términos de n

### Complejidad en MuServer

- gObj: O(n) para buscar
- Inventory: O(n) para buscar item
- Sorting: O(n log n)

---

## Código del MuServer

### O(n) - Loop Simple

```cpp
// O(n) - busca todos los objetos
int gObjGetOnlineCount()
{
    int count = 0;
    for(int n = 0; n < MAX_OBJECT; n++)  // n = MAX_OBJECT
    {
        if(gObj[n].Connected == OBJECT_ONLINE)
        {
            count++;
        }
    }
    return count;  // O(n)
}
```

### O(n²) - Bucles Anidados

```cpp
// O(n²) - comparar todos contra todos
void CheckCollisions()
{
    for(int i = 0; i < MAX_OBJECT; i++)       // n
    {
        if(!gObj[i].Live) continue;
        
        for(int j = 0; j < MAX_OBJECT; j++)   // n
        {
            if(i == j) continue;
            if(!gObj[j].Live) continue;
            
            // Verificar colisión
            if(IsInRange(gObj[i], gObj[j]))
            {
                //處理碰撞
            }
        }
    }
}
```

### O(log n) - Binary Search

```cpp
// O(log n) - búsqueda binaria si está ordenado
int BinarySearch(OBJECTSTRUCT* array, int size, const char* name)
{
    int left = 0;
    int right = size - 1;
    
    while(left <= right)
    {
        int mid = (left + right) / 2;
        
        int cmp = strcmp(array[mid].Name, name);
        if(cmp == 0) return mid;
        if(cmp < 0) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Complejidad

Determina la complejidad de:
```cpp
for(int i = 0; i < n; i++)
    for(int j = 0; j < n; j++)
        arr[i] = j;
```

### Ejercicio 2: Mejorar Complejidad

El siguiente código es O(n²). Cómo lo改进?
```cpp
for(int i = 0; i < n; i++)
    for(int j = 0; j < n; j++)
        if(arr[i] == arr[j]) ...
```

### Ejercicio 3: Encontrar O

Cuál es la complejidad de gObjFindName?