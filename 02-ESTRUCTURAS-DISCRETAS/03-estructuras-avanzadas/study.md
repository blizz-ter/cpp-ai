# Estructuras de Datos Avanzadas

## Teoría

### Hash Tables

Tablas hash almacenan datos con acceso O(1) promedio:
- **Función hash**: Convierte clave en índice
- **Resolución de colisiones**: Chaining o open addressing
- **Load factor**: α = n/m (elementos / buckets)

### Object Pools

Pools de objetos reutilizan memoria:
- Evita asignación repetida
- Memoria pre-allocada
-归还机制 para objetos freed

### Estructuras Híbridas

Combinan ventajas de múltiples estructuras:
- **Hash + List**: Búsqueda + orden
- **Tree + Hash**: Range + punto
- **Stack + Queue**: Scheduler

---

## Código del MuServer

### Object Pool de Jugadores

```cpp
//Archivo: ObjectPool.h
//Pool de objetos para gestionar OBJECTSTRUCT

class CObjectPool
{
private:
    OBJECTSTRUCT* m_Objects;
    std::queue<int> m_FreeIndices;
    
public:
    CObjectPool(int maxSize)
    {
        m_Objects = new OBJECTSTRUCT[maxSize];
        
        for(int i = 0; i < maxSize; i++)
        {
            m_FreeIndices.push(i);
        }
    }
    
    OBJECTSTRUCT* Allocate()
    {
        if(m_FreeIndices.empty())
            return nullptr;
            
        int index = m_FreeIndices.front();
        m_FreeIndices.pop();
        
        return &m_Objects[index];
    }
    
    void Deallocate(OBJECTSTRUCT* obj)
    {
        int index = obj - m_Objects;
        m_FreeIndices.push(index);
    }
};
```

### Hash Table para Player Lookup

```cpp
//Lookup de jugadores por nombre
class CPlayerHash
{
private:
    static const int HASH_SIZE = 1024;
    std::list<OBJECTSTRUCT*> m_HashTable[HASH_SIZE];
    
public:
    int Hash(const char* name)
    {
        int hash = 0;
        for(int i = 0; name[i]; i++)
        {
            hash = (hash * 31 + name[i]) % HASH_SIZE;
        }
        return hash;
    }
    
    void Insert(OBJECTSTRUCT* player)
    {
        int h = Hash(player->Name);
        m_HashTable[h].push_back(player);
    }
    
    OBJECTSTRUCT* Find(const char* name)
    {
        int h = Hash(name);
        for(auto p : m_HashTable[h])
        {
            if(strcmp(p->Name, name) == 0)
                return p;
        }
        return nullptr;
    }
};
```

### Inventory Pool

```cpp
//Pool de items usando object pool
class CItemPool
{
private:
    CItem* m_Items;
    bool* m_InUse;
    
public:
    CItemPool(int size)
    {
        m_Items = new CItem[size];
        m_InUse = new bool[size]();
    }
    
    CItem* Allocate()
    {
        for(int i = 0; i < MAX_ITEM; i++)
        {
            if(!m_InUse[i])
            {
                m_InUse[i] = true;
                return &m_Items[i];
            }
        }
        return nullptr;
    }
    
    void Deallocate(CItem* item)
    {
        int index = item - m_Items;
        m_InUse[index] = false;
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Pool

Dado un pool de 1000 objetos:
- Se allocaron 500 objetos
- Se liberaron 100

**Preguntas**:
1. ¿Cuántos objetos hay en uso?
2. ¿Cuántos índices libres hay?
3. ¿Qué pasa si allocamos 900 más?

### Ejercicio 2: Implementar Hash

Implementa una tabla hash para strings con:
- Insert(key, value)
- Find(key) → value
- Remove(key)

### Ejercicio 3: Object Pool

Implementa un pool de integers con:
- Allocate() → int*
- Deallocate(int*)
- Count() de objetos