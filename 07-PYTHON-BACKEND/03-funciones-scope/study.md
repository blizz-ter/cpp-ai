# Funciones y Scope en Python

## Teoría

### Definir Funciones

```python
def saludar(nombre):
    return f"Hola, {nombre}!"

def calcular_daño(fuerza, nivel):
    return fuerza * nivel
```

### Scope

- **Local**: Dentro de función
- **Enclosing**: Funciones anidadas
- **Global**: Módulo
- **Built-in**: funciones integradas

---

## Ejercicio

### Ejercicio 1: Función Player

Crea función que reciba nombre y nivel y retorne string de presentación.

### Ejercicio 2: Scope

Experimenta con variables locales vs globales.