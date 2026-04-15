# Proyecto Integrador: Python Backend

## Descripción del Proyecto

Crear API REST para sistema de estadísticas del juego.

## Requisitos

- Crear endpoint para estadísticas de jugadores
- Usar FastAPI o Flask
- Conectar a MySQL

## Implementación

### Estructura del Proyecto

```
backend/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── models/
│   │   └── player.py
│   ├── routes/
│   │   └── stats.py
│   └── database/
│       └── db.py
├── requirements.txt
└── test/
```

### API con FastAPI

```python
from fastapi import FastAPI, Depends
from pydantic import BaseModel

app = FastAPI()

class PlayerStats(BaseModel):
    username: str
    level: int
    experience: int
    kills: int
    deaths: int

@app.get("/api/players/{username}/stats")
async def get_player_stats(username: str):
    # Query to database
    return PlayerStats(
        username=username,
        level=50,
        experience=100000,
        kills=150,
        deaths=30
    )

@app.post("/api/players/{username}/stats")
async def update_player_stats(username: str, stats: PlayerStats):
    # Update database
    return {"status": "updated"}
```

### Conexión a MySQL

```python
import mysql.connector

def get_connection():
    return mysql.connector.connect(
        host="localhost",
        user="muserver",
        password="password",
        database="muserver_db"
    )
```

## Entregable

1. API funcional
2. Tests unitarios
3. README con instrucciones

## Evaluación

-Funcionalidad de endpoints
- Código limpio
- Tests passing