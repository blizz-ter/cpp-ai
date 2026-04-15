# Async y Concurrencia

## Teoría

- **Threads**: Ejecución paralela
- **async/await**: Síntaxis para operaciones asíncronas
- **Event loop**: Procesamiento de eventos
- **Futures/Promises**: Resultados diferidos
- **Thread pool**: Pool de workers reutilizables

## Código MuServer

```cpp
// GameServer usa multithreading
HANDLE hClientThread = CreateThread(NULL, 0, 
    ClientThreadProc, lpParam, 0, &threadId);

// Event-driven con select()
fd_set readfds;
select(0, &readfds, NULL, NULL, &timeout);

// IOCP (Input Output Completion Port)
CreateIoCompletionPort(INVALID_HANDLE_VALUE, NULL, 0, 0);
```

## Ejercicio

1. Identificar modelo de concurrencia en MuServer
2. Comparar con async/await moderno