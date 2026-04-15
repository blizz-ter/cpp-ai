# Replicación y Clustering

## Teoría

- **Replicación master-slave**: Un primary, múltiples réplicas
- **Replicación multi-master**: Escritura en múltiples nodos
- **Sharding**: Particionamiento horizontal de datos
- **Clustering**: Alta disponibilidad (HA)
- **Failover automático**: Recuperación ante fallos

## Código MuServer

```cpp
// ODBC connection para replicación
// Verificar en DBCmd.cpp o similares
SQLRETURN SQLExecDirect(SQLHSTMT StatementHandle, 
    SQLCHAR* StatementText, SQLINTEGER TextLength);
```

## Ejercicio

1. Buscar manejo de conexiones en código MuServer
2. Documentar estrategias de recovery