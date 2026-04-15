# Optimización en Python

## Teoría

### Performance en Python

Python es más lento que C++ pero hay técnicas de optimización.

### Técnicas

| Técnica | Speedup | Cuándo usarla |
|---------|---------|---------------|
| list comprehensions | 2x | loops simples |
| built-in functions | 10x | map, filter, sorted |
| numpy/numba | 100x | computación numérica |
| C extensions | 1000x | hot paths |
| caching | variable | cálculos repetidos |

### Optimización de Código

```python
# Lento
result = []
for item in items:
    result.append(item.value * 2)

# Rápido
result = [item.value * 2 for item in items]

# Más rápido con map
result = list(map(lambda x: x.value * 2, items))
```

### Caching con LRU

```python
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

### NumPy Optimization

```python
import numpy as np

def matrix_multiply_slow(a, b):
    result = [[0] * len(b[0]) for _ in range(len(a))]
    for i in range(len(a)):
        for j in range(len(b[0])):
            for k in range(len(b)):
                result[i][j] += a[i][k] * b[k][j]
    return result

def matrix_multiply_fast(a, b):
    return np.dot(a, b)
```

## Código del Servidor

### Optimización de Queries

```python
import queries

# Lento - N+1 problem
users = []
for user_id in user_ids:
    users.append(db.get_user(user_id))

# Rápido
users = db.get_users_by_ids(user_ids)
```

## Ejercicio

1. Optimizar función lenta
2. Medir improvement
3. Aplicar caching