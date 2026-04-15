# NoSQL Databases

## Teoría

### Tipos

- **Documento**: MongoDB, CouchDB
- **Key-Value**: Redis, DynamoDB
- **Columna**: Cassandra
- **Grafo**: Neo4j

### Cuando Usar NoSQL

- Datos no estructurados
- Alta escalabilidad
- Schemas flexibles

---

## Redis en Juegos

```python
import redis
r = redis.Redis()
r.set("player:1:level", 10)
r.get("player:1:level")
```