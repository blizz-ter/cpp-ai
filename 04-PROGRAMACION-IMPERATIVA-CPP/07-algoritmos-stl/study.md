# Algoritmos STL en C++

## Teoría

### ¿Qué son los Algoritmos STL?

La STL proporciona algoritmos genéricos que operan sobre contenedores. Se encuentran en el header `<algorithm>` y incluyen funciones para:

#### Algoritmos de Búsqueda
- **std::find**: Encuentra primera ocurrencia de un valor
- **std::find_if**: Encuentra elemento que cumple predicado
- **std::binary_search**: Búsqueda binaria en contenedores ordenados
- **std::lower_bound**: Primer elemento no menor que valor
- **std::upper_bound**: Primer elemento mayor que valor

#### Algoritmos de Ordenamiento
- **std::sort**: Ordena rango completo (O(n log n))
- **std::stable_sort**: Orden estable
- **std::partial_sort**: Ordena parcialmente
- **std::nth_element**: Encuentra el n-ésimo elemento

#### Algoritmos de Modificación
- **std::copy**: Copia elementos
- **std::transform**: Aplica función a cada elemento
- **std::remove / std::erase**: Elimina elementos
- **std::replace**: Reemplaza elementos
- **std::reverse**: Invierte orden

#### Algoritmos Numéricos
- **std::accumulate**: Suma todos los elementos
- **std::count / std::count_if**: Cuenta elementos
- **std::min_element / std::max_element**

### Lambdas

Las funciones lambda permiten escribir funciones anónimas inline:
```cpp
[capture](parameters) -> return_type { body }
```

---

## Ejemplos en MuServer

### std::find - Buscar en Vectores

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\UnionInfo.cpp`

```cpp
#include <algorithm>

// Buscar en lista de miembros de guild
std::vector<int>::iterator _Itor = std::find(
    this->m_vtUnionMember.begin(), 
    this->m_vtUnionMember.end(), 
    iGuildNumber
);

if(_Itor != this->m_vtUnionMember.end())
{
    // Encontrado
    this->m_vtUnionMember.erase(_Itor);
}

// Buscar en lista de rivales
_Itor = std::find(
    this->m_vtRivalMember.begin(), 
    this->m_vtRivalMember.end(), 
    iGuildNumber
);
```

### std::sort - Ordenar Jugadores por Puntuación

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\Crywolf.cpp`

```cpp
#include <algorithm>

// Estructura para ordenar por puntuación
struct CCrywolfUtil
{
    static bool CrywolfAllUserScoreSort(CrywolfUserScore a, CrywolfUserScore b)
    {
        return a.Score > b.Score;  // Orden descendente
    }
};

// En algún método
std::vector<CrywolfHeroList> CrywolfHeroList; // contiene datos de puntuación
std::sort(CrywolfHeroList.begin(), CrywolfHeroList.end(), 
          CCrywolfUtil::CrywolfAllUserScoreSort);
```

### std::find - Buscar en Usuarios de Castillo

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\CastleSiege.cpp`

```cpp
#include <algorithm>

// Verificar si usuario está en lista de request
if(std::find(
    this->m_vtMiniMapReqUser.begin(), 
    this->m_vtMiniMapReqUser.end(), 
    iIndex
) == this->m_vtMiniMapReqUser.end())
{
    // No encontrado, agregar
    this->m_vtMiniMapReqUser.push_back(iIndex);
}

// Remover usuario de lista
std::vector<int>::iterator it = std::find(
    this->m_vtMiniMapReqUser.begin(), 
    this->m_vtMiniMapReqUser.end(), 
    iIndex
);

if(it != this->m_vtMiniMapReqUser.end())
{
    this->m_vtMiniMapReqUser.erase(it);
}
```

### std::find_if - Búsqueda con Predicado

```cpp
#include <algorithm>

// Encontrar primer item con nivel específico
auto it = std::find_if(
    inventory.begin(), 
    inventory.end(),
    [](const CItem& item) {
        return item.m_Level == 5;
    }
);

if(it != inventory.end())
{
    // Encontrado, procesar
    CItem& foundItem = *it;
}

// Encontrar primer item有效期已过期
auto expiredIt = std::find_if(
    inventory.begin(),
    inventory.end(),
    [](const CItem& item) {
        return item.m_IsExpiredItem == true;
    }
);
```

### std::transform - Transformar Datos

```cpp
#include <algorithm>

// Transformar coordenadas relativas a absolutas
std::vector<short> relativeCoords = {10, 20, 30, 40};
std::vector<short> absoluteCoords(relativeCoords.size());

std::transform(
    relativeCoords.begin(), 
    relativeCoords.end(),
    absoluteCoords.begin(),
    [](short relative) {
        return relative + 100;  // Offset base
    }
);

// Aplicar bonificación a todos los stats
std::vector<int> stats = {100, 200, 300, 400, 500};
std::transform(
    stats.begin(),
    stats.end(),
    stats.begin(),
    [](int stat) {
        return stat * 1.5;  // 50% de bonificación
    }
);
```

### std::count_if - Contar Elementos

```cpp
#include <algorithm>

// Contar items de cierto nivel
int countLevel5 = std::count_if(
    inventory.begin(),
    inventory.end(),
    [](const CItem& item) {
        return item.m_Level >= 5;
    }
);

// Contar jugadores online con cierto nivel
int countHighLevel = std::count_if(
    playerList.begin(),
    playerList.end(),
    [](const LPOBJ& lpObj) {
        return lpObj->Level >= 300;
    }
);
```

### std::accumulate - Sumas Acumuladas

```cpp
#include <numeric>

// Calcular experiencia total de party
int totalExp = std::accumulate(
    partyMembers.begin(),
    partyMembers.end(),
    0,
    [](int sum, LPOBJ& member) {
        return sum + member->Experience;
    }
);

// Calcular daño total de todos los skills
int totalDamage = std::accumulate(
    skillList.begin(),
    skillList.end(),
    0,
    [](int sum, const SKILL_INFO& skill) {
        return sum + skill.Damage;
    }
);
```

### std::for_each - Procesar Todos los Elementos

```cpp
#include <algorithm>

// Procesar todos los items en inventario
std::for_each(
    inventory.begin(),
    inventory.end(),
    [](CItem& item) {
        if(item.IsItem())
        {
            item.CheckDurabilityState();
        }
    }
);
```

---

## Ejercicio Práctico

### Ejercicio 1: Implementar Ordenamiento de Líderes

Crea una función que ordene un vector de `PlayerScore` (con nombre y puntuación) de forma descendente. El código base:

```cpp
struct PlayerScore
{
    std::string Name;
    int Score;
};

std::vector<PlayerScore> leaderboard;

// Agregar algunos datos de prueba
leaderboard.push_back({"Player1", 5000});
leaderboard.push_back({"Player2", 8000});
leaderboard.push_back({"Player3", 3000});

// Ordenar usando lambda
std::sort(/* completar */);

// Imprimir resultados
for(const auto& p : leaderboard)
{
    std::cout << p.Name << ": " << p.Score << std::endl;
}
```

### Ejercicio 2: Buscar y Filtrar Items

Dado un inventario (vector de CItem), escribe código para:
1. Encontrar el primer item con nivel >= 10
2. Contar cuántos items tienen opción "Excelente"
3. Listar todos los items que vencen en menos de 24 horas
4. Remover todos los items expirados

```cpp
std::vector<CItem> inventory;

// Tu código aquí...
```

### Ejercicio 3: Análisis de Algoritmo

El código de Crywolf usa `std::sort` con una función de comparación. Responde:
1. ¿Qué complejidad tiene `std::sort`?
2. ¿Por qué se usa `stable_sort` en lugar de `sort` si necesitamos orden estable?
3. ¿Qué Pass: `std::sort` puede no ser estable?

### Ejercicio 4: Optimización de Búsqueda

Compara estas dos aproximaciones para buscar un item en un inventario:
1. Usar `std::find` en un vector no ordenado
2. Usar `std::map` con el índice del item como clave

Para 1000 items, ¿cuál es más rápido? ¿Por qué?
