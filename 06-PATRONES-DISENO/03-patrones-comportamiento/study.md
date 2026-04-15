# Patrones Comportamentales

## Teoría

### Observer

Define dependencia uno-a-muchos:
```cpp
class Observer { virtual void Update() = 0; };
class Subject {
    std::vector<Observer*> observers;
public:
    void Attach(Observer* o) { observers.push_back(o); }
    void Notify() { for(auto o: observers) o->Update(); }
};
```

---

## Ejercicio

### Ejercicio 1: Observer

Crea PlayerScoreObserver que notifica cambios.

### Ejercicio 2: Strategy

Usa diferentes estrategias de ataque.

### Ejercicio 3: Command

 encapsula comandos de usuario.