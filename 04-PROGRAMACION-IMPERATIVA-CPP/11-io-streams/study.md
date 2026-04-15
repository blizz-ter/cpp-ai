# I/O Streams en C++

## Teoría

### Streams

- **ifstream**: Leer archivos
- **ofstream**: Escribir archivos
- **fstream**: Leer y escribir
- **stringstream**: Strings como streams

---

## Código del MuServer

### Leer ItemData

```cpp
ifstream in("items.txt");
while(in >> itemId >> name >> damage)
{
    ProcessItem(itemId, name, damage);
}
```

### Guardar Scores

```cpp
ofstream out("ranking.txt");
for(auto& p : players)
    out << p.name << " " << p.score << "\n";
```

---

## Ejercicio Práctico

### Ejercicio 1: Leer Archivo

Lee integers de un archivo, imprímelos.

### Ejercicio 2: Escribir

Escribe los números 1-100 a un archivo.