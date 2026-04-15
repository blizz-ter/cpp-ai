# Tipos Avanzados - Templates y Genéricos

## Teoría

### Templates

Templates permiten escribir código genérico:
```cpp
template<typename T>
T max(T a, T b) { return a > b ? a : b; }
```

### Clase Template

```cpp
template<typename T>
class Stack {
    std::vector<T> data;
public:
    void push(T val);
    T pop();
};
```

---

## Código del MuServer

### ObjectPool Template

```cpp
template<typename T>
class TObjectPool
{
    std::vector<T*> m_FreeObjects;
    
public:
    T* allocate()
    {
        if(m_FreeObjects.empty())
            return new T();
        T* obj = m_FreeObjects.back();
        m_FreeObjects.pop_back();
        return obj;
    }
    
    void deallocate(T* obj)
    {
        m_FreeObjects.push_back(obj);
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Template Function

Crea un template que intercambie dos valores.

### Ejercicio 2: Template Class

Implementa un Array template.

### Ejercicio 3: Specialization

Especialización para tipos puntero.