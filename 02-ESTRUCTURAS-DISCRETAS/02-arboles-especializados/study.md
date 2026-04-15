# trees Especializados

## Teoría

### Binary Search Tree (BST)

Un BST es un árbol donde:
- Nodos menores van a la izquierda
- Nodos mayores van a la derecha
- Búsqueda O(log n) promedio

### Operaciones en BST

- **Insert**: O(log n) promedio
- **Search**: O(log n) promedio
- **Delete**: O(log n) promedio
- **Traversal**: In-order, pre-order, post-order

### Balanced trees

- **AVL**: Diferencia de altura max 1
- **Red-Black**: Altura ≤ 2*log(n+1)
- **B-trees**: Para bases de datos

### Aplicaciones

- **Skill Tree**: Árbol de habilidades
- **Inventory sorting**: Organización
- **Pathfinding**: Decisiones

---

## Código del MuServer

### SkillTree - Árbol de Habilidades

```cpp
// Archivo: SkillTree.h
// El árbol de habilidades es un BST

struct SKILL_TREE_NODE
{
    short skillId;
    short parentSkillId;  // skill padre
    short levelRequired;
    std::vector<SKILL_TREE_NODE*> children;
};

class CSkillTree
{
private:
    std::map<short, SKILL_TREE_NODE> m_Skills;
    
public:
    bool CanLearn(short playerSkillId, short skillId)
    {
        // Verificar pre-requisitos
        auto& skill = m_Skills[skillId];
        
        if(skill.parentSkillId != 0)
        {
            // Debe tener el skill padre
            if(!playerHasSkill(skill.parentSkillId))
                return false;
        }
        
        if(playerLevel < skill.levelRequired)
            return false;
            
        return true;
    }
};
```

### In-order Traversal

```cpp
void InOrderTraversal(SKILL_TREE_NODE* node)
{
    if(node == nullptr) return;
    
    // Izquierda
    for(auto child : node->children)
    {
        InOrderTraversal(child);
    }
    
    // Procesar nodo
    ProcessSkill(node->skillId);
    
    // Derecha (manual ordering)
}
```

### BST de Inventory

```cpp
// Sorted inventory usando BST
class CSortedInventory
{
private:
    struct INVENTORY_NODE
    {
        CItem* item;
        INVENTORY_NODE* left;
        INVENTORY_NODE* right;
    };
    
    INVENTORY_NODE* root;
    
public:
    void Insert(CItem* item)
    {
        // BST insert
    }
    
    void TraverseInOrder()
    {
        // Recorrer en orden
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar SkillTree

Dado un SkillTree con:
- Skill 1 (raíz)
- Skills 2,3 hijos de 1
- Skills 4 hijo de 2
- Skill 5 hijo de 3

**Preguntas**:
1. ¿Cuántos nodos tiene el árbol?
2. ¿Cuál es la profundidad máxima?
3. ¿Qué skills requiere skill 4?

### Ejercicio 2: Implementar BST

Implementa un BST simple para enteros con:
- Insert
- Search
- InOrderTraversal

### Ejercicio 3: CanLearn

Implementa la función CanLearn() que verifique:
1. Tiene el skill padre
2. Tiene el nivel requerido
3. No tiene el skill ya