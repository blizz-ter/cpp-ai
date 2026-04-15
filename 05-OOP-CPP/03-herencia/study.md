# Herencia en C++

## Teoría

### Herencia

Herencia permite crear clases derivadas de clases base:
```cpp
class Base { };
class Derived : public Base { };
```

### Miembros protected

- **private**: solo accesible por la clase
- **protected**: accesible por clases derivadas
- **public**: accesible por todos

---

## Código del MuServer

### Inheritance de Object

```cpp
class CGameObject
{
protected:
    short m_X;
    short m_Y;
    BYTE m_Map;
    
public:
    virtual void Update() = 0;
};

class CPlayer : public CGameObject
{
public:
    void Update() override;
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Crear Herencia

Crea una jerarquía:
- Item (base)
- Weapon : Item
- Armor : Item

### Ejercicio 2: Protected Members

Usa protected para position, public para getters.

### Ejercicio 3: Multiple Inheritance

Hereda de dos clases si es necesario.