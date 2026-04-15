# Sistemas Operativos I - Procesos y Threads

## Teoría

### Procesos

Un proceso es un programa en ejecución:
- Tiene su propio espacio de dirección
- Puede contener múltiples threads
- Recursos: memoria, archivos, handles

### Threads

Un thread es una unidad de ejecución dentro de un proceso:
- Comparte recursos del proceso padre
- Cada thread tiene su propio stack
- Ejecución并行 (simultánea)

### Concurrencia vs Paralelismo

- **Concurrencia**: Múltientes tareas progresan overlapping en el tiempo
- **Paralelismo**: Múltiendes tareas ejecutan simultaneously en múltiples CPUs

### Scheduling

El SO decide qué thread ejecutar:
- Time slices (quantum)
- Prioridades
- Preemptive vs cooperative

---

## Código del MuServer

### GameServer.cpp - Threads del Juego

```cpp
// Archivo: GameServer.cpp
#include "stdafx.h"

// Threads principales del servidor
HANDLE hGameServerThread;
HANDLE hEventServerThread;
HANDLE hDataServerThread;

DWORD WINAPI GameServerMain(LPVOID lpParam)
{
    while(true)
    {
        // Procesar objetos
        gObjProcess();
        
        // Procesar mobs
        gMobsProcess();
        
        // Procesar eventos
        gEventProcess();
        
        Sleep(1); // Yield al CPU
    }
    return 0;
}
```

### Creación de Threads

```cpp
// Crear thread de servidor
void StartServerThreads()
{
    DWORD threadId;
    
    hGameServerThread = CreateThread(
        NULL,                    // security attributes
        0,                      // stack size (0 = default)
        GameServerMain,           // función del thread
        NULL,                   // parámetros
        0,                      // flags
        &threadId               // thread ID
    );
    
    hDataServerThread = CreateThread(
        NULL, 0, DataServerMain, NULL, 0, &threadId
    );
}
```

### Sincronización con CriticalSection

```cpp
// Proteger datos compartidos
CRITICAL_SECTION csProcess;

void gObjSecondProc()
{
    EnterCriticalSection(&csProcess);
    
    // Acceso a OBJECTSTRUCT
    for(int n = 0; n < MAX_OBJECT; n++)
    {
        if(gObj[n].Live)
        {
            // Procesar jugador
        }
    }
    
    LeaveCriticalSection(&csProcess);
}
```

---

## Ejercicio Práctico

### Ejercicio 1: Analizar Threads

Dado el código:
```cpp
HANDLE hThread1, hThread2;

DWORD WINAPI WorkerThread(LPVOID p)
{
    int* counter = (int*)p;
    for(int i = 0; i < 1000; i++)
        (*counter)++;
    return 0;
}
```

**Preguntas**:
1. ¿Cuántos threads se crean?
2. ¿Comparten la variable counter?
3. ¿Hay race condition?

### Ejercicio 2: Crear Thread

Crea un programa que:
1. Cree un thread que imprima números del 1 al 10
2. El thread principal espere a que termine
3. Imprima "Terminado"

### Ejercicio 3: Sincronización

Añade un CriticalSection al ejercicio anterior para proteger el counter.