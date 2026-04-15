# Análisis Profundo: Sistema de Mapas

## Estructura CMap

En `Map.h`:

```cpp
class CMap {
public:
    BYTE GetAttr(int x, int y);
    bool CheckAttr(int x, int y, BYTE attr);
    bool CheckStandAttr(int x, int y);
    // ...
};
```

---

## Mapas Implementados

| ID | Nombre | Archivo |
|----|--------|---------|
| 0 | Lorencia | 0.map |
| 1 | Dungeon | 1.map |
| 2 | Devias | 2.map |
| 3 | Noria | 3.map |
| 4 | LostTower | 4.map |
| 5 | Excile | 5.map |
| 7 | Atlans | 7.map |
| 8 | Tarkan | 8.map |
| 9 | DevilSquare | 9.map |
| 11-17 | Blood Castle 1-7 | 11-17.map |
| 18-23 | Chaos Castle 1-7 | 18-23.map |
| 24-30 | Kalima 1-7 | 24-30.map |
| 31 | Kanturu | 31.map |
| 34 | Castle Siege | 34.map |
| 41 | Common | 41.map |
| 51 | Base | 51.map |

---

## Mapas Faltantes (según README)

| Mapa | Estado |
|------|--------|
| Karutan Map | ⚠️ Listo para implementar |

---

## Pathfinding

```cpp
// En Move.cpp
int PathFind4JR(OBJECTSTRUCT* lpObj, int sx, int sy, int tx, int ty, int count);
// Usado para:
- AI de monstruos
- movement automático
```

---

## Problemas

### 1. Sin validación de coordenadas

```cpp
// Cliente envía posición, servidor no valida
// teleport hack possible
```

### 2. Pathfinding básico

Solo soporta movimiento básico, no hay A*.

---

*Análisis de Mapas - MuServer S5U15*