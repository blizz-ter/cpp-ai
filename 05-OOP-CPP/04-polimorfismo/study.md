# Polimorfismo en C++

## Teoría

### Funciones Virtuales

Polimorfismo permite que clases derivadas redefinan métodos:
```cpp
class Base {
public:
    virtual void Draw() = 0;
};
```

### override

```cpp
class Derived : public Base {
public:
    void Draw() override;
};
```

### vtable

Cada clase virtual tiene una tabla de funciones virtuales.

---

## Código del MuServer

### Protocol Dispatch

```cpp
class IProtocolHandler
{
public:
    virtual void Handle(BYTE* buffer) = 0;
    virtual ~IProtocolHandler() {}
};

class LoginHandler : public IProtocolHandler
{
public:
    void Handle(BYTE* buffer) override;
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Virtual Function

Crea una clase base con virtual Update().

### Ejercicio 2: Override

Deriva Player y redefine Update().

### Ejercicio 3: Pure Virtual

Haz Update() = 0 para interfaz.