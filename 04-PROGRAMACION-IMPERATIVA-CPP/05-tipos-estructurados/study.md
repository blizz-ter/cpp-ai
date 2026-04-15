# Tipos Estructurados en C++

## Teoría

### Estructuras (struct)

Una estructura es un tipo de dato compuesto:
- Grupo de variables bajo un nombre
- Acceso con operador punto (.)
- En memoria: campos consecutivos

### Definición de struct

```cpp
struct PlayerData {
    char name[11];
    int level;
    float health;
};
```

### Diferencia con class

- struct: miembros públicos por defecto
- class: miembros privados por defecto
- Otherwise idénticos

---

## Código del MuServer

### OBJECTSTRUCT

```cpp
//Archivo: User.h
struct OBJECTSTRUCT
{
    int Index;
    char Account[11];
    char Name[11];
    short Level;
    float Life;
    float MaxLife;
    DWORD Experience;
    WORD DamageMin;
    WORD DamageMax;
    short X;
    short Y;
    BYTE Map;
    BYTE Class;
};
```

### Uso de struct

```cpp
void CreatePlayer(OBJECTSTRUCT* player)
{
    player->Level = 1;
    player->Life = 100.0f;
    player->MaxLife = 100.0f;
    strcpy(player->Name, "Player1");
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Crear struct

Crea un struct ItemData con:
- name[31]
- level
- damage
- defense
- durability

### Ejercicio 2: Acceso

Dado:
```cpp
ItemData item;
item.level = 5;
```
Cómo accedes y modificas damage?

### Ejercicio 3: Array de structs

Crea un array de 76 ItemData e inicialízalos.