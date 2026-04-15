# Proyecto Integrador: Programacion Imperativa C++

## Descripcion del Proyecto

Analizar la estructura `OBJECTSTRUCT` del codigo MuServer y crear una clase que extienda la funcionalidad existente. Este proyecto te permite entender como se estructuran los datos en un servidor de juegos real y como trabajar con memoria y punteros en C++.

## Archivos a Estudiar

- `Source MuServer Update 15/GameServer/GameServer/User.h` - Define `OBJECTSTRUCT` y todas las constantes del juego
- `Source MuServer Update 15/GameServer/GameServer/Item.h` - Clase CItem para items

## Pasos para Completar

### Paso 1: Analisis de OBJECTSTRUCT

1. Lee User.h lineas 468-1076 (estructura OBJECTSTRUCT)
2. Identifica los campos relacionados con el inventario:
   - `CItem* Inventory` (linea 667)
   - `CItem* Warehouse` (linea 678)
   - `CItem* Trade` (linea 673)
3. Observa como se usan los punteros para gestionar la memoria

### Paso 2: Crear una Clase de Estadisticas

Crea un archivo `PlayerStats.h` que contenga una clase para calcular estadisticas del jugador:

```cpp
class PlayerStats {
public:
    static int CalculateAttackDamage(LPOBJ lpObj);
    static int CalculateDefense(LPOBJ lpObj);
    static int CalculateMagicDefense(LPOBJ lpObj);
    static int CalculateAttackSpeed(LPOBJ lpObj);
};
```

### Paso 3: Implementar los Metodos

Crea `PlayerStats.cpp` implementando cada metodo:

1. **CalculateAttackDamage**: Combina PhysyDamageMin/Max con bonuses
2. **CalculateDefense**: Suma la defensa base + bonificaciones de items
3. **CalculateMagicDefense**: Similar a defense pero para dano magico
4. **CalculateAttackSpeed**: Calcula la velocidad de ataque

### Paso 4: Integracion

1. Agrega tu clase al proyecto
2. Llama los metodos desde una funcion existente (busca `gObjSecondProc` en User.cpp)

## Archivos a Modificar/Crear

- `PlayerStats.h` - Declaracion de la clase
- `PlayerStats.cpp` - Implementacion

## Como Verificar

1. El codigo debe compilar sin errores
2. Los calculos deben ser consistentes con los existentes en el servidor
3. Agrega logs de debug para verificar los valores

## Comandos de Compilacion

```bash
# Compile with Visual Studio
devenv MuServer.sln /build Debug
```

## Recursos Adicionales

- Busca `gObjCalcDistance` en User.cpp para ejemplos de calculos
- Observa como se usan las macros como `CHECK_RANGE`, `OBJECT_RANGE`