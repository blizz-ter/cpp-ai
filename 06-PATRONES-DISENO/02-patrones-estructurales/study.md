# Patrones Estructurales

## Teoría

### Adapter

Convierte una interfaz en otra:
```cpp
class Adapter : public Target {
    Adaptee* adaptee;
public:
    void Request() { adaptee->SpecificRequest(); }
};
```

---

## Ejercicio

### Ejercicio 1: Adapter

Adapta OldProtocol a NewProtocol.

### Ejercicio 2: Decorator

Añade funcionalidad a Item sin heredar.

### Ejercicio 3: Facade

Simplifica acceso a subsistemas.