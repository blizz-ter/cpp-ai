# Lambdas y Programación Funcional

## Teoría

### Lambdas

Una lambda es una función anónima:
```cpp
auto add = [](int a, int b) { return a + b; };
```

### Capture modes

- []: Sin captura
- [=]: Copia todo por valor
- [&]: Referencia todo por referencia
- [this]: Captura this

### std::algorithm con Lambdas

```cpp
std::sort(v.begin(), v.end(), 
    [](int a, int b) { return a > b; });
```

---

## Código del MuServer

### Sort con Lambda

```cpp
std::sort(inventory, inventory + size,
    [](const CItem& a, const CItem& b)
    {
        return a.m_Level > b.m_Level;
    });
```

### Find con Lambda

```cpp
auto it = std::find_if(items, items + size,
    [](const CItem& item)
    {
        return item.m_Index == swordId;
    });
```

---

## Ejercicio Práctico

### Ejercicio 1: Lambda Simple

Crea una lambda que sume dos ints.

### Ejercicio 2: Capture

Usa lambda con captura de variable externa.

### Ejercicio 3: sort

Ordena un vector de ints en orden descendente.