# Seguridad en Bases de Datos

## Teoría

- **SQL Injection**: Prevención con parameterized queries
- **Control de acceso**: Roles, usuarios, permisos
- **Cifrado de datos**: TLS, cifrado en reposo
- **Auditoría**: Logs de actividad
- **Backup seguro**: Encriptación de backups

## Código MuServer

```cpp
// ODBC con SQLExecDirect (vulnerable si no usa parámetros)
SQLHSTMT hstmt;
SQLCHAR sql[256] = "SELECT * FROM Account WHERE Id='";
strcat(sql, userInput); // PELIGRO: SQL Injection
SQLExecDirect(hstmt, sql, SQL_NTS);
```

## Ejercicio

1. Buscar queries en MuServer que usen concatenación
2. Proponer solución con SQLPrepare