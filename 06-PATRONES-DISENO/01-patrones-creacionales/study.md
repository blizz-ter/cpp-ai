# Patrones Creacionales

## Teoría

### Singleton

Garante una única instancia:
```cpp
class Singleton {
    static Singleton* instance;
    Singleton() {}
public:
    static Singleton* Get() {
        if(!instance) instance = new Singleton();
        return instance;
    }
};
```

---

## MuServer

### Singleton en Servers

```cpp
class CDataServer
{
    static CDataServer* m_pInstance;
    CDataServer() {}
public:
    static CDataServer* GetInstance() {
        if(!m_pInstance) m_pInstance = new CDataServer();
        return m_pInstance;
    }
};
```

---

## Ejercicio

### Ejercicio 1: Singleton

Implementa DatabaseSingleton.

### Ejercicio 2: Factory

Crea ItemFactory para spawn items.

### Ejercicio 3: Builder

Construye Player conBuilder pattern.