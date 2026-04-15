---
name: muserver-code-analyzer
description: Analiza el código fuente del MuServer (GameServer). Úsalo cuando el usuario wants entender la estructura, arquitectura, o analizar archivos específicos del servidor. Puede explicar sistemas, encontrar vulnerabilidades, o documentar funcionalidad.
---

# MuServer Code Analyzer

Skill para analizar el código fuente del MuServer Season 5 Update 15.

## Cuándo usar este skill

Usa este skill cuando:
- El usuario pida analizar el servidor
- Necesite entender la estructura del código
- Pida explicar sistemas específicos (protocolo, items, mobs, eventos)
- Quiere encontrar problemas o entender cómo funciona algo

## Estructura del código

### Archivos principales

```
GameServer/
├── GameServer.cpp     # Entry point, WinMain
├── Protocol.cpp       # Router de paquetes (5207 líneas)
├── GameMain.cpp      # Main loop
├── User.h            # OBJECTSTRUCT (jugador)
├── Item.h            # CItem (items)
├── Skill.h           # CSkill
├── Map.h            # Mapas
├── SocketManager*.h   # Networking
└── *.cpp            # Sistemas (events, features)
```

### Sistemas implementados

| Sistema | Archivo |
|--------|---------|
| Protocolo red | Protocol.cpp |
| Objetos | User.h/cpp |
| Items | Item.h/cpp |
| Skills | Skill.h/cpp |
| Mapas | Map.h/cpp |
| Eventos | Event*.cpp (BloodCastle, ChaosCastle, etc) |
| Guilds | Guild.h/cpp |
| Personal Shop | PersonalShop.cpp |
| CashShop | CashShop.cpp |
| Rankings | CustomRanking.cpp |

## Cómo analizar

### Para analizar un archivo específico:

1. Leer el archivo con Read tool
2. Identificar las estructuras clave (structs, clases)
3. Explicar las funciones principales
4. Dar ejemplos de uso en el código

### Para analizar el flujo de red:

1. Protocol.cpp:75 muestra el router principal
2. Cada case es un tipo de paquete
3. gSocketManager / gSocketManagerModern manejan conexiones

### Para entender Objetos:

```cpp
// User.h
struct OBJECTSTRUCT {
    int Index;
    int Connected;
    char Account[11];
    char Name[11];
    WORD Class;
    BYTE ChangeUp;
    short Level;
    DWORD Experience;
    // ... stats, inventory, skills
};
```

## Comandos útiles

- `glob pattern` para encontrar archivos
- `read filePath` para leer código
- `grep pattern` para buscar en código

## Output

Al analizar, proporciona:
1. Resumen del sistema
2. Estructuras clave
3. Funciones principales
4. Cómo se usa en otros archivos
5. Notas de problemas o mejoras