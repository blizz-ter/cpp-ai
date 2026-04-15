# Const Correctness en C++

## Teoría

### const

```cpp
const int MAX = 100;
int GetValue() const;
```

### const correctness

- Métodos que no modifican estado: const
- Parámetros que no modifican: const ref

---

## Código

### Getter const

```cpp
class CPlayer
{
public:
    short GetLevel() const { return m_Level; }
    int GetIndex() const { return m_Index; }
};
```

### const Reference

```cpp
void SendPacket(const CPlayer& player)
{
    // No puede modificar player
}
```

---

## Ejercicio

### Ejercicio 1: Getters const

Marca todos los getters como const.

### Ejercicio 2: const_iterator

Usa const_iterators para solo lectura.

### Ejercicio 3: const Correct

Pasa objetos por const ref cuando no los modificas.