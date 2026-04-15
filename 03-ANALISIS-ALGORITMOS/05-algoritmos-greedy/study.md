# Algoritmos Greedy

## Teoría

### Características Greedy

Los algoritmos greedy toman decisiones localmente óptimas:
- Eligen la mejor opción en cada paso
- No reconsideran decisiones previas
- No siempre encuentran solución óptima
- Funcionan para problemas con "propiedad greedy"

### Ejemplos de Problemas Greedy

- **Dijkstra**: Siempre expandir el nodo con menor distancia
- **Huffman**: Siempre fusionar los dos nodos con menor frecuencia
- **Fractional Knapsack**: Siempre tomar el item con mejor ratio
- **Scheduling**: Seleccionar la tarea más corta primero

### Greedy vs DP

- Greedy: O(n log n), no siempre óptimo
- DP: O(n²) o más, siempre óptimo

---

## Código del MuServer

### Algoritmo Greedy en Pathfinding

```cpp
//Archivo: Pathfinding.cpp
//Algoritmo greedy para encontrar camino

bool FindPathGreedy(short startX, short startY,
                   short endX, short endY,
                   std::vector<short>& path)
{
    short x = startX;
    short y = startY;
    
    while(x != endX || y != endY)
    {
        // Siempre mover hacia la dirección con menor distancia
        short dx = (endX > x) ? 1 : (endX < x) ? -1 : 0;
        short dy = (endY > y) ? 1 : (endY < y) ? -1 : 0;
        
        if(IsWalkable(x + dx, y + dy))
            x += dx;
        else if(IsWalkable(x, y + dy))
            y += dy;
        else if(IsWalkable(x + dx, y))
            x += dx;
        else
            return false;  // No hay camino
            
        path.push_back(x);
        path.push_back(y);
    }
    return true;
}
```

### Dijkstra Simplificado

```cpp
//Versión simplificada de Dijkstra
struct PathNode
{
    short x, y;
    int dist;
};

int FindBestPath(CMap* map, short startX, short startY,
               short endX, short endY)
{
    std::priority_queue<PathNode> pq;
    std::set<std::pair<short, short>> visited;
    
    pq.push({startX, startY, 0});
    
    while(!pq.empty())
    {
        auto current = pq.top();
        pq.pop();
        
        if(current.x == endX && current.y == endY)
            return current.dist;
            
        auto key = std::make_pair(current.x, current.y);
        if(visited.count(key)) continue;
        visited.insert(key);
        
        // Expandir vecinos (greedy: siempre menor distancia)
        for(auto neighbor : GetNeighbors(current.x, current.y))
        {
            if(!visited.count(neighbor))
            {
                int newDist = current.dist + 1;
                pq.push({neighbor.x, neighbor.y, newDist});
            }
        }
    }
    return -1;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Greedy vs DP Scheduling

Tienes 5 tasks con duración: [3, 5, 2, 7, 1]
Minimiza el tiempo total de completion.

**Preguntas**:
1. Cuál orden greedy funciona?
2. Es óptimo?

### Ejercicio 2: Greedy Path

Implementa un algoritmo greedy para grid 8x8 que:
- Siempre intente moverse en X primero
- Luego Y

### Ejercicio 3: Fractional Knapsack

Knapsack fraccional: puedes tomar fracciones de items.
Peso: [10, 20, 15], Valor: [60, 100, 80], Capacidad: 25
Usa greedy.