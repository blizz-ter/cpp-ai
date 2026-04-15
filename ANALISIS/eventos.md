# Análisis Profundo: Sistema de Eventos

## Eventos Implementados

| Evento | Archivo | Estados | Reward |
|-------|---------|---------|--------|
| Blood Castle | BloodCastle.cpp | 8 niveles | wings/items |
| Chaos Castle | ChaosCastle.cpp | 7 niveles | items |
| Devil Square | DevilSquare.cpp | 2 versiones | devilsquare items |
| Crywolf | Crywolf.cpp | 5 estados | bless/diamond |
| Kanturu | Kanturu*.cpp | 3 fases | noble items |
| DoppelGanger | DoubleGoer.cpp | 4 fases | items |
| Imperial Guardian | ImperialGuardian.cpp | 4 fases | items |
| Illusion Temple | IllusionTemple.cpp | 6 niveles | wings |
| TvT | EventTvT.cpp | - | PC points |
| GvG | EventGvG.cpp | - | guild points |

---

## Sistema de Eventos (Genérico)

```cpp
// Event*.cpp siguen el mismo patrón:
class CEventName {
public:
    bool Enabled;
    int State;         // 0=closed, 1=open, 2=playing, 3=ending
    int Timer;
    int Remaintime;

    void Init();
    void Load();
    void Enable();
    void Disable();
    void RunLogic();  // Update cada segundo
    void CheckUsers(); // Validar users calificados
    void SpawnMonster(); // Spawn mobs del evento
    void GiveReward();  // Rewards al winner
};
```

---

## Estado del Sistema

| Sistema | Archivo | Estado |
|---------|---------|--------|
| Blood Castle | BloodCastle.cpp | ✅ Working |
| Chaos Castle | ChaosCastle.cpp | ✅ Working |
| Devil Square | DevilSquare.cpp | ✅ Working |
| Crywolf | Crywolf.cpp | ✅ Working |
| Kanturu | Kanturu*.cpp | ✅ Working |
| Doppler | DoubleGoer.cpp | ✅ Working |
| Imperial Guardian | ImperialGuardian.cpp | ✅ Working |
| TvT | EventTvT.cpp | ✅ Working |
| Custom Events | Custom*.cpp | ✅ Working |
| Arca Battle | ArcaBattle.cpp | ✅ Working |

---

## problemas

### 1. Sin anti-spam en eventos

```cpp
// Usuarios pueden join evento muchas veces
// Sin validation adecuada
```

### 2. Timers pueden desincronizarse

```cpp
// No hay sync entre server y eventos
// Possible desync
```

---

*Análisis de Eventos - MuServer S5U15*