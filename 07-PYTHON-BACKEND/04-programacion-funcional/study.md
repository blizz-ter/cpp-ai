# Programación Funcional en Python

## Teoría

### Lambdas

```python
doblar = lambda x: x * 2
```

### map, filter, reduce

```python
nums = [1, 2, 3]
dobles = list(map(lambda x: x * 2, nums))
pares = list(filter(lambda x: x % 2 == 0, nums))
```

---

## Ejercicio

### map/filter

Aplica map y filter a una lista de niveles.