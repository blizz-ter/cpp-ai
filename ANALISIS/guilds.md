# Análisis Profundo: Sistema de Guilds

## Estructura GUILD_INFO_STRUCT

En `Guild.h`:

```cpp
struct GUILD_INFO_STRUCT {
    int Index;
    char Name[11];
    char Master[11];
    int Score;
    int TotalScore;
    int MemberCount;
    int MaxMemberCount;
    BYTE Mark[32];  // Guild mark (32 bytes)
    int Ranking;
    int Wins;
    int Losses;
    int Draws;
    // ...
};
```

---

## Guild Member

```cpp
struct GUILD_USER_STRUCT {
    int aIndex;
    char Name[11];
    BYTE Status;  // 0xff=offline, 1=online
    BYTE Represent;  // Role: GM=0, Normal=1
};
```

---

## Funciones del Sistema

| Función | Descripción |
|---------|-------------|
| GuildCreate | Crear guild |
| GuildDestroy | Disolver guild |
| GuildInsertMember | Agregar miembro |
| GuildDeleteMember | Expulsar miembro |
| GuildSetMaster | Transferir master |
| GuildWarDeclare | Declarar guerra |
| GuildWarSurrender | Rendirse |
| GuildNotice | Noticia guild |

---

## Castle Siege

Sistema de guilds avanzado em `CastleSiege.cpp`:

```cpp
class CCastleSiege {
    // Estados castle siege
    // Money per week
    // Guilds participantes
    // Monarch election
    // ...
};
```

---

## Estado del Sistema

| Sistema | Archivo | Estado |
|---------|---------|--------|
| Basic Guild | Guild.cpp | ✅ Working |
| Guild Matching | GuildMatching.cpp | ⚠️ Parcial |
| Guild Warehouse | Warehouse.cpp | ✅ Working |
| Castle Siege | CastleSiege.cpp | ✅ Working Only (GAMESERVER_TYPE=1) |

---

## Problemas

### 1. GUILD_TYPE restriction

```cpp
// CastleSiege solo disponible en GAMESERVER_TYPE=1
#if(GAMESERVER_TYPE==1)
class CCastleSiege { ... }
#endif
```

### 2. Guild Matching incomplete

Hay código pero parcialmente implementado.

---

*Análisis de Guilds - MuServer S5U15*