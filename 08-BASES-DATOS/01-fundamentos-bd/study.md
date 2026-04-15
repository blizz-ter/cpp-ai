# Fundamentos de Bases de Datos

## Teoría

### ¿Qué es una BD?

Una base de datos es una colección organizada de datos:
- **Relacional**: Tablas con relaciones (SQL)
- **NoSQL**: Documentos, key-value, grafos

### Componentes

- **Tabla**: Colección de filas
- **Columna**: Atributo
- **Fila**: Registro
- **Primary Key**: Identificador único
- **Foreign Key**: Referencia a otra tabla

---

## Código

### MySQL/PostgreSQL

```sql
CREATE TABLE players (
    id INT PRIMARY KEY,
    name VARCHAR(11),
    level INT,
    experience INT
);
```

---