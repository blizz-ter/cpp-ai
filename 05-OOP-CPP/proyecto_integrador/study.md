# Proyecto Integrador: OOP C++

## Descripcion del Proyecto

Analizar la clase `CItem` del codigo MuServer y agregar un nuevo metodo a la clase. Este proyecto te introduce a la programacion orientada a objetos en C++ con un ejemplo real de un servidor de juegos.

## Archivos a Estudiar

- `Source MuServer Update 15/GameServer/GameServer/Item.h` - Declaracion de CItem (lineas 90-210)
- `Source MuServer Update 15/GameServer/GameServer/Item.cpp` - Implementacion de metodos

## Pasos para Completar

### Paso 1: Analisis de la Clase CItem

1. Lee Item.h lineas 90-129 para ver la declaracion de metodos
2. Identifica los metodos existentes:
   - `IsItem()` - Verifica si es un item valido
   - `IsExcItem()` - Verifica si es item excellent
   - `IsSetItem()` - Verifica si es set item
   - `GetDamageMin()`, `GetDamageMax()` - Calculo de daño
   - `GetDefense()` - Calculo de defensa

### Paso 2: Disenar un Nuevo Metodo

Crea un metodo que verifique si el item es transferable (se puede mover entre cuentas):

```cpp
bool IsTransferable();
```

Este metodo debe:
1. Verificar si es un item bounded (no transferable)
2. Verificar si es item de quest
3. Verificar si esta en el inventario principal

### Paso 3: Implementar en Item.h

Agrega la declaracion:

```cpp
bool IsTransferable();
```

### Paso 4: Implementar en Item.cpp

Busca donde están implementados los otros métodos Is* y agrega tu implementacion:

```cpp
bool CItem::IsTransferable() // OK
{
    if(this->m_IsValidItem == false)
        return false;

    if(this->m_QuestItem == true)
        return false;

    for(int i = 0; i < MAX_SPECIAL; i++)
    {
        if(this->m_SpecialIndex[i] == ITEM_SPECIAL_BOUND)
            return false;
    }

    return true;
}
```

## Archivos a Modificar

- `Item.h` - Agregar declaracion del metodo
- `Item.cpp` - Agregar implementacion del metodo

## Como Verificar

1. Compila sin errores
2. El metodo devuelve true para items transferibles
3. El metodo devuelve false para items de quest y bound

## Recursos Adicionales

- Busca `ITEM_SPECIAL_BOUND` en el codigo para entender el sistema de bound items
- Observa como otros metodos Is* verifican condiciones similares