# Teoría de Grafos

## Teoría

### Definiciones Fundamentales

Un grafo G = (V, E) donde:
- **V**: Conjunto de vértices (nodos)
- **E**: Conjunto de aristas (edges)

### Tipos de Grafos

- **Dirigidos vs No dirigidos**: Las aristas tienen dirección
- **Ponderados vs No ponderados**: Las aristas tienen peso
- **Cíclicos vs Acíclicos**: Contienen ciclos

### Representaciones

- **Matriz de adyacencia**: O(V²) memoria
- **Lista de adyacencia**: O(V + E) memoria

### Algoritmos Importantes

- **BFS** (Breadth-First Search): Encuentra camino más corto
- **DFS** (Depth-First Search): Explora profundo
- **Dijkstra**: Caminos más cortos en grafos ponderados
- **A***: Búsqueda heurística

---

## Código del MuServer

### Pathfinding - Grafos en Mapas

```cpp
// Archivo: MapPath.h
// El mapa es un grafo donde los nodos son coordenadas

#define MAX_MAP_WIDTH 256
#define MAX_MAP_HEIGHT 256

struct MAP_NODE
{
    short x, y;              // coordenadas
    bool walkable;            // si se puede caminar
    BYTE height;             // altura del terreno
    std::vector<MAP_NODE*> neighbors;  // nodos adyacentes
};

class CMapGraph
{
private:
    MAP_NODE m_Nodes[MAX_MAP_WIDTH][MAX_MAP_HEIGHT];
    
public:
    bool FindPath(short startX, short startY, 
                short endX, short endY,
                std::vector<short>& path);
                
    bool BFS(short startX, short startY,
           short endX, short endY,
           std::vector<short>& path);
};
```

### BFS en Pathfinding

```cpp
// BFS para encontrar camino
bool CMapGraph::BFS(short startX, short startY,
                   short endX, short endY,
                   std::vector<short>& path)
{
    std::queue<MAP_NODE*> queue;
    std::map<MAP_NODE*, MAP_NODE*> parent;
    
    MAP_NODE* start = &m_Nodes[startX][startY];
    MAP_NODE* end = &m_Nodes[endX][endY];
    
    queue.push(start);
    parent[start] = nullptr;
    
    while(!queue.empty())
    {
        MAP_NODE* current = queue.front();
        queue.pop();
        
        if(current == end)
        {
            // Reconstruir camino
            MAP_NODE* node = end;
            while(node)
            {
                path.push_back(node->x);
                path.push_back(node->y);
                node = parent[node];
            }
            return true;
        }
        
        // Explorar vecinos
        for(auto neighbor : current->neighbors)
        {
            if(neighbor->walkable && !parent.count(neighbor))
            {
                queue.push(neighbor);
                parent[neighbor] = current;
            }
        }
    }
    return false;
}
```

### Viewport como Grafo

```cpp
// El viewport del jugador también es un grafo
struct VIEWPORT_STRUCT
{
    int index;
    int number;
    short X;
    short Y;
    BYTE MapNumber;
    BYTE Dir;
    int Distance;  // distancia del jugador
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Pathfinding

Dado un mapa 8x8 donde:
- (0,0) a (0,7), (7,0) a (7,7) son paredes
- Solo (1,1) a (6,6) son caminables

**Preguntas**:
1. ¿Cuántos vértices tiene el grafo?
2. ¿Es el grafo conectado?
3. Dibuja las conexiones del nodo (1,1)

### Ejercicio 2: Implementar BFS

Implementa un BFS simple que encuentre el camino más corto en una grilla 4x4.

### Ejercicio 3: Dijkstra

Si las aristas tienen peso (terreno difícil = peso alto), implementa Dijkstra.