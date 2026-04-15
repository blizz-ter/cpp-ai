# Búsqueda de Strings

## Teoría

### Algoritmos de Búsqueda en Strings

Buscar patrones en texto es fundamental:
- **Brute Force**: O(n*m) - probar cada posición
- **KMP**: O(n+m) - usar información de prefijos
- **Boyer-Moore**: O(n*m worst, O(n) promedio)
- **Rabin-Karp**: O(n+m) - hashing

### conceptos

- **Patrón**: String a buscar
- **Texto**: String donde buscar
- **Prefix function**: Longitud del prefijo más largo que también es sufijo

### Aplicaciones

- Búsqueda de nombres de jugadores
- Chat filtering
- Nombre de items

---

## Código del MuServer

### gObjFindName - Búsqueda de Jugadores

```cpp
//Archivo: User.cpp
//Búsqueda de jugadores por nombre

int gObjFindName(char* name)
{
    for(int n = 0; n < MAX_OBJECT; n++)
    {
        if(gObj[n].Connected == OBJECT_ONLINE)
        {
            if(strcmp(gObj[n].Name, name) == 0)
            {
                return n;
            }
        }
    }
    return -1;
}
```

### Brute Force para Nombres

```cpp
//Búsqueda simple - O(n*m)
int FindPlayerByName(const char* name)
{
    for(int i = 0; i < MAX_OBJECT; i++)
    {
        if(gObj[i].Connected == OBJECT_ONLINE)
        {
            //strcmp es O(m)
            if(strcmp(gObj[i].Name, name) == 0)
            {
                return i;
            }
        }
    }
    return -1;
}
```

### Filter de Nombres

```cpp
//Filtrar palabras inapropiadas
bool ContainsFilterWord(const char* text)
{
    const char* filterWords[] = {"spam", "hack", "bot"};
    int filterCount = 3;
    
    for(int i = 0; i < filterCount; i++)
    {
        if(strstr(text, filterWords[i]) != nullptr)
        {
            return true;
        }
    }
    return false;
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar gObjFind

Dado un servidor con 1000 jugadores:
- 500 están online
- Buscamos "Player100"

**Preguntas**:
1. ¿Cuántas comparaciones hace strcmp en worst case?
2. ¿Cuántas en average case?

### Ejercicio 2: Implementar Búsqueda

Implementa una función que busque en un array de strings:
```cpp
int FindString(const char** array, int size, const char* target);
```

### Ejercicio 3: Búsqueda Parcial

Implementa búsqueda parcial (contiene) vs búsqueda exacta.