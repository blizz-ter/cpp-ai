# Iteradores en C++

## Teoría

### ¿Qué es un Iterador?

Un iterador es un objeto que permite traversar contenedores:
- Like un puntero avanzado
- Provee operadores: ++, *, ->
- Decouple el algoritmo del contenedor

### Tipos de Iteradores

- **Input**: Lecturaforwardonly
- **Output**: Escritura forward only
- **Forward**: Lectura/escritura forward
- **Bidirectional**: ++ y --
- **Random access**: +, -, []

---

## Código del MuServer

### Iterar sobre Inventory

```cpp
// Usar iterador
for(auto it = inventory.begin(); it != inventory.end(); ++it)
{
    if(it->m_Index > 0)
    {
        ProcessItem(*it);
    }
}
```

### std::find

```cpp
auto it = std::find(inventory.begin(), inventory.end(),
    [](const CItem& item) { return item.m_Index == swordId; });
```

---

## Ejercicio Práctico

### Ejercicio 1: Iterar

Itera sobre un array de 10 ints usando iteradores.

### Ejercicio 2: Find

Encuentra el primer item con level > 5.

### Ejercicio 3: Reverse Iteration

Itera en reversa sobre un vector.