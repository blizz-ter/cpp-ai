# Bases de Datos desde Código

## Teoría

- **ODBC**: Open Database Connectivity
- **ADO.NET**: Para .NET
- **SQLAlchemy**: ORM para Python
- **SQLite/MySQL/PostgreSQL**: Diferentes engines
- **Connection pooling**: Reutilización de conexiones

## Código MuServer

```cpp
// DBCmd.cpp
SQLHENV henv;
SQLHDBC hdbc;
SQLConnect(hdbc, (SQLCHAR*)"MuOnline", SQL_NTS, 
    (SQLCHAR*)"sa", SQL_NTS, (SQLCHAR*)"password", SQL_NTS);

// Query
SQLExecDirect(hstmt, (SQLCHAR*)"SELECT * FROM Character", SQL_NTS);
while (SQLFetch(hstmt) == SQL_SUCCESS) {
    SQLGetData(hstmt, 1, SQL_C_CHAR, name, 32, &cbName);
}
```

## Ejercicio

1. Mapear tablas de Mu Online (Character, Inventory, etc.)
2. Crear queries para operaciones comunes