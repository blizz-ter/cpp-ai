# Contenedores STL en C++

## Teoría

### ¿Qué son los Contenedores STL?

La Standard Template Library (STL) de C++ proporciona contenedores genéricos que gestionan colecciones de objetos. Los principales tipos son:

#### Secuencias
- **std::vector**: Array dinámico de tamaño variable - acceso O(1),插入/删除 O(n)
- **std::list**: Lista doblemente enlazada - acceso O(n),插入/删除 O(1)
- **std::deque**: Cola de doble extremo - acceso O(1),插入/删除 O(1) al inicio/final

#### Contenedores Asociativos
- **std::map**: Árbol binario balanceado (clave-valor) - búsqueda O(log n)
- **std::multimap**: Map que permite claves duplicadas
- **std::set**: Conjunto ordenado sin duplicados
- **std::unordered_map**: Tabla hash - búsqueda O(1) promedio
- **std::unordered_set**: Set basado en hash

### Métodos Comunes

- `size()`: Retorna número de elementos
- `empty()`: Retorna si está vacío
- `begin()` / `end()`: Iteradores
- `push_back()`: Agregar al final (vector)
- `insert()`: Insertar elemento
- `erase()`: Eliminar elemento
- `find()`: Buscar elemento
- `clear()`: Vaciar contenedor
- `operator[]`: Acceso directo (map, vector)

---

## Ejemplos en MuServer

### std::vector - Listas de Tiempo

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\Time/CTimCheck.h`

```cpp
// Definición
std::vector<TimeCheck> stl_Time;
std::vector<TimeCheck>::iterator stl_Time_I;

// Uso típico
for(std::vector<TimeCheck>::iterator it = this->stl_Time.begin(); 
    it != this->stl_Time.end(); it++)
{
    // Procesar cada TimeCheck
}
```

### std::vector - Información de Items

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\ResetTable.h`

```cpp
struct RESET_TABLE_INFO
{
    int ResetMin;
    int ResetMax;
    int MasterResetMin;
    int MasterResetMax;
    int RewardZen;
    int RewardCredit;
};

class CResetTable
{
public:
    std::vector<RESET_TABLE_INFO> m_ResetTableInfo;
    
    // Cargar desde archivo
    void Load(const char* file)
    {
        // Cargar datos...
    }
    
    // Buscar configuración de reset
    RESET_TABLE_INFO* GetResetInfo(int resetCount)
    {
        for(std::vector<RESET_TABLE_INFO>::iterator it = this->m_ResetTableInfo.begin();
            it != this->m_ResetTableInfo.end(); it++)
        {
            if(it->ResetMin <= resetCount && it->ResetMax >= resetCount)
            {
                return &(*it);
            }
        }
        return nullptr;
    }
};
```

### std::map - Gestión de Habilidades

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\SkillManager.h`

```cpp
struct SKILL_INFO
{
    int Index;
    int SkillID;
    int Level;
    int Damage;
    int Mana;
    int BP;
    int ReqLevel;
    int ReqSkill;
};

class CSkillManager
{
public:
    std::map<int, SKILL_INFO> m_SkillInfo;
    
    // Buscar habilidad por índice
    SKILL_INFO* GetSkill(int index)
    {
        std::map<int, SKILL_INFO>::iterator it = this->m_SkillInfo.find(index);
        
        if(it != this->m_SkillInfo.end())
        {
            return &it->second;
        }
        
        return nullptr;
    }
    
    // Agregar habilidad
    void AddSkill(int index, SKILL_INFO info)
    {
        this->m_SkillInfo.insert(std::pair<int, SKILL_INFO>(index, info));
    }
};
```

### std::map con std::vector - Sistema de Items

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\ItemOption.h`

```cpp
// Map de vectores: cada clave tiene múltiples opciones
std::map<int, std::vector<ITEM_OPTION_INFO>> m_ItemOptionInfo;

// Ejemplo de uso
bool CItemOption::Load(const char* file)
{
    // ...
    
    std::map<int, std::vector<ITEM_OPTION_INFO>>::iterator it = 
        this->m_ItemOptionInfo.find(info.Index);
    
    if(it == this->m_ItemOptionInfo.end())
    {
        // No existe, crear nueva entrada
        this->m_ItemOptionInfo.insert(
            std::pair<int, std::vector<ITEM_OPTION_INFO>>(
                info.Index, 
                std::vector<ITEM_OPTION_INFO>(1, info)
            )
        );
    }
    else
    {
        // Ya existe, agregar a la lista
        it->second.push_back(info);
    }
    
    return true;
}

// Obtener opciones de un item
ITEM_OPTION_INFO* GetItemOption(int index)
{
    std::map<int, std::vector<ITEM_OPTION_INFO>>::iterator ItemOptionInfo = 
        this->m_ItemOptionInfo.find(index);
    
    if(ItemOptionInfo == this->m_ItemOptionInfo.end())
    {
        return nullptr;
    }
    
    // Retornar primera opción
    if(!ItemOptionInfo->second.empty())
    {
        return &ItemOptionInfo->second[0];
    }
    
    return nullptr;
}
```

### std::vector - Lista de Usuarios

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\RaklionBattleUserMng.h`

```cpp
class CRaklionBattleUserMng
{
public:
    std::vector<int> m_UserInfo;
    
    void AddUser(int aIndex)
    {
        this->m_UserInfo.push_back(aIndex);
    }
    
    void RemoveUser(int aIndex)
    {
        std::vector<int>::iterator it = std::find(
            this->m_UserInfo.begin(), 
            this->m_UserInfo.end(), 
            aIndex
        );
        
        if(it != this->m_UserInfo.end())
        {
            this->m_UserInfo.erase(it);
        }
    }
    
    int GetUserCount()
    {
        return this->m_UserInfo.size();
    }
};
```

### std::map - Sistema de Mensajes

**Archivo**: `Source MuServer Update 15\GameServer\GameServer\Message.h`

```cpp
struct MESSAGE_INFO
{
    int Index;
    char Message[256];
};

class CMessage
{
public:
    std::map<int, MESSAGE_INFO> m_MessageInfo;
    
    MESSAGE_INFO* GetMessage(int index)
    {
        std::map<int, MESSAGE_INFO>::iterator it = this->m_MessageInfo.find(index);
        
        if(it != this->m_MessageInfo.end())
        {
            return &it->second;
        }
        
        return nullptr;
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Elegir el Contenedor Correcto

Para cada escenario, indica qué contenedor STL usarías y por qué:

1. **Inventario de tienda**: Necesitas buscar items por ID rápidamente y mantener orden
2. **Lista de jugadores online**: Añadir/remover frecuentemente, iterar para procesar
3. **Caché de datos de usuario**: Búsqueda por nombre de usuario, datos simples
4. **Cola de mensajes**: Añadir al final, procesar desde el frente

### Ejercicio 2: Implementar un Sistema de Configuración

Crea una clase `ConfigManager` que:
1. Use `std::map<std::string, std::string>` para almacenar configuraciones
2. Tenga método `Load(key, value)` para agregar configuración
3. Tenga método `GetString(key, defaultValue)` para obtener string
4. Tenga método `GetInt(key, defaultValue)` para obtener entero
5. Tenga método `GetBool(key, defaultValue)` para obtener booleano

```cpp
class ConfigManager
{
private:
    std::map<std::string, std::string> m_Config;
    
public:
    void Load(const std::string& key, const std::string& value);
    std::string GetString(const std::string& key, const std::string& defaultValue);
    int GetInt(const std::string& key, int defaultValue);
    bool GetBool(const std::string& key, bool defaultValue);
};
```

### Ejercicio 3: Convertir Array a Vector

El código actual usa un array estático:
```cpp
#define MAX_ITEMS 100
CItem g_ItemList[MAX_ITEMS];
```

Convártelo a usar `std::vector<CItem>` y añade:
1. Método para agregar un item
2. Método para buscar un item por índice
3. Método para remover un item
4. Método para listar todos los items

### Ejercicio 4: Análisis de Código Real

Analiza el siguiente código de MuServer y explica qué hace:

```cpp
std::map<int, std::vector<ITEM_OPTION_INFO>>::iterator it = 
    this->m_ItemOptionInfo.find(index);

if(it != this->m_ItemOptionInfo.end())
{
    for(std::vector<ITEM_OPTION_INFO>::iterator optIt = it->second.begin();
        optIt != it->second.end(); optIt++)
    {
        // Procesar cada opción
    }
}
```
