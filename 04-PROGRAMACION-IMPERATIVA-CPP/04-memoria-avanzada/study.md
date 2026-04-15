# Memoria Avanzada y RAII

## Teoría

### RAII (Resource Acquisition Is Initialization)

RAII vincula recursos al ciclo de vida de objetos:
- Constructor: adquiere recursos
- Destructor: libera recursos
- Asegura cleanup automático

### Smart Pointers

- **unique_ptr**: Dueño único, movimiento automático
- **shared_ptr**: Propiedades compartidas, reference counting
- **weak_ptr**: Referencia no propietarios

### Gestión de Memoria en Juegos

- Allocate en login,Deallocate en logout
- Object pools para objetos frecuentes
- Cache de stats

---

## Código del MuServer

### RAII para Inventory

```cpp
// Inventory con RAII
class CInventory
{
private:
    CItem* m_Items;
    int m_Size;
    
public:
    CInventory(int size) : m_Size(size)
    {
        m_Items = new CItem[size];
    }
    
    ~CInventory()
    {
        delete[] m_Items;  // Cleanup automático
    }
};
```

### Object Pool con RAII

```cpp
// Pool de objetos con cleanup
class CObjectPool
{
private:
    std::vector<OBJECTSTRUCT> m_Objects;
    std::queue<int> m_FreeIndices;
    
public:
    CObjectPool(int maxSize)
    {
        m_Objects.resize(maxSize);
        for(int i = 0; i < maxSize; i++)
            m_FreeIndices.push(i);
    }
    
    ~CObjectPool()
    {
        // Cleanup automático
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: RAII Class

Crea una clase PlayerSession que:
- Allocate inventory en constructor
- Deallocate en destructor

### Ejercicio 2: Smart Pointer

Convierte a unique_ptr:
```cpp
CItem* item = new CItem[76];
```

### Ejercicio 3: Memory Leak

Encuentra el memory leak:
```cpp
void CreateInventory()
{
    CItem* inv = new CItem[76];
    return; // sin delete
}
```