# Optimización de Consultas

## Teoría

### EXPLAIN

Analiza el plan de ejecución:
```sql
EXPLAIN SELECT * FROM players WHERE level > 10;
```

### Índices

- **B-tree**: Default para equality/range
- **Hash**: Solo equality
- **Full-text**: Búsqueda de texto

---