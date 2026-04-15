# Arquitectura de Computadoras

## Teoría

### Componentes Fundamentales

La arquitectura de computadoras incluye:

- **CPU (Central Processing Unit)**: Ejecuta instrucciones
- **Memoria (RAM)**: Almacena datos temporalmente
- **Entrada/Salida (I/O)**: Dispositivos externos
- **Bus de datos**: Comunicación entre componentes

### Ciclo de Ejecución

1. Fetch: Obtener instrucción de memoria
2. Decode: Interpretar instrucción
3. Execute: Realizar operación
4. Store: Guardar resultado

### Registros de CPU

Los registros son memorias rápidas dentro de la CPU:
- **EAX, EBX, ECX, EDX**: Propósito general
- **ESP**: Stack pointer
- **EIP**: Instruction pointer
- **FLAGS**: Flags de estado

### Memoria en Windows

- ** stack**: Variables locales, parámetros
- ** heap**: Asignación dinámica
- ** data**: Variables globales
- ** code**: Instrucciones del programa

---

## Código del MuServer

### Winmain.cpp - Punto de Entrada

**Archivo**: `Source Main 5.2\source\Winmain.cpp`

```cpp
// WinMain es el punto de entrada en aplicaciones Windows
int APIENTRY WinMain(HINSTANCE hInstance,
                     HINSTANCE hPrevInstance,
                     LPSTR     lpCmdLine,
                     int       nCmdShow)
{
    // Inicialización del servidor
    // 1. Cargar configuración
    // 2. Inicializar sockets
    // 3. Crear threads de juego
    // 4. Entrar en loop principal
    
    return 0;
}
```

### GameServer.cpp - Thread Principal

```cpp
// Loop principal del servidor de juego
DWORD WINAPI GameServerMain(LPVOID lpParam)
{
    while(true)
    {
        // Procesar jugadores
        // Procesar mobs
        // Actualizar eventos
        // Guardar datos
        
        Sleep(1); // Yield al CPU
    }
    return 0;
}
```

### Memoria en Objetos

```cpp
// OBJECTSTRUCT usa memoria asignada dinámicamente
struct OBJECTSTRUCT
{
    // Datos del jugador en memoria
    char Account[11];
    char Name[11];
    
    // Posición
    short X;
    short Y;
    BYTE Map;
    
    // Stats
    short Level;
    float Life;
    float MaxLife;
    DWORD Experience;
    
    // Punteros a otros datos
    CItem* Inventory;
    CItem* Trade;
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar OBJECTSTRUCT

Dado el código de User.h con OBJECTSTRUCT:

```cpp
struct OBJECTSTRUCT
{
    char Name[11];
    short X;
    short Y;
    BYTE Map;
    float Life;
};
```

**Preguntas**:
1. ¿Qué tipo de memoria usa Name[11]?
2. ¿Cuántos bytes ocupa X + Y?
3. ¿Por qué Map es BYTE?

### Ejercicio 2: Punto de Entrada

Crea una función main() simple que:
1. Imprima "Servidor iniciando..."
2. Inicialice un contador
3. En un loop, aumente el contador
4. Cuando llegue a 100, termine

### Ejercicio 3: Memoria de Inventory

El Inventory de un jugador tiene 76 slots (12x6 + 4). Si cada item ocupa 80 bytes:
1. ¿Cuántos bytes de heap necesita?
2. ¿Dónde se almacenan los 76 slots en OBJECTSTRUCT?