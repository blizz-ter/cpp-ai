# Proyecto Integrador: Estructuras Discretas

## Descripción del Proyecto

Implementar estructuras de datos para gestionar el inventario del juego.

## Requisitos

- Implementar sistema de inventario usando estructuras jerárquicas
- Usar grafos para rutas de NPCs
- Optimizar búsqueda de items

## Código del MuServer

### Inventory System

```cpp
#define MAX_ITEMS 64
#define MAX_ITEM_TYPE 12

struct ITEM {
    int m_Index;
    int m_Type;
    int m_Level;
    int m_Option;
    BYTE m_ItemSlot[MAX_ITEM_TYPE];
};

// Árbol de items por categoría
class CItemManager {
private:
    std::map<int, ITEM> m_Inventory;
    std::vector<ITEM> m_ItemList;
    
public:
    bool AddItem(ITEM item);
    bool RemoveItem(int index);
    ITEM* FindItem(int index);
    std::vector<ITEM> GetItemsByType(int type);
};
```

### Pathfinding con Grafos

```cpp
class CPathFinder {
private:
    struct Node {
        int x, y;
        std::vector<Node*> neighbors;
    };
    std::map<int, Node*> worldMap;
    
public:
    std::vector<Position> FindPath(Position start, Position end);
};
```

## Entregable

1. Implementación de C++
2. Tests unitarios
3. Documentación de complexity

## Evaluación

- Funcionalidad correcta
- Eficiencia (Big-O)
- Código limpio