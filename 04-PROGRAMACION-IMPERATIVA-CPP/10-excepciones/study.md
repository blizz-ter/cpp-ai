# Excepciones en C++

## Teoría

### try-catch

```cpp
try {
    riskyOperation();
} catch (const std::exception& e) {
    std::cerr << e.what();
}
```

### throw

```cpp
void ProcessItem(CItem* item) {
    if(!item)
        throw std::invalid_argument("nullptr item");
}
```

---

## Ejercicio Práctico

### Ejercicio 1: try-catch

Maneja la división por cero.

### Ejercicio 2: Custom Exception

Crea una GameException class.

### Ejercicio 3: Exception Safety

 обеспечь safe cleanup con exceptions.