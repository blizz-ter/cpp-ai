# 01-paradigmas - Paradigmas de Programación aplicado al MuServer

## Objetivo
Entender los paradigmas de programación studiando el código real.

## Paradigmas en el MuServer

### Imperativo (C)
El MuServer usa mucho C-style imperativo:
```cpp
// Example from Protocol.cpp - estilo imperativo
void ProtocolCore(BYTE head, BYTE* lpMsg, int size, int aIndex, int encrypt, int serial) {
    switch(head) {
        case 0x00:
            CGChatRecv((PMSG_CHAT_RECV*)lpMsg, aIndex);
            break;
        // ...
    }
}
```

### Orientado a Objetos (C++)
La mayoría del código es OOP:
```cpp
// Item.h - clase con métodos
class CItem {
public:
    void Clear();
    bool IsItem();
    // ...
};
```

### mixto
El código mezcla ambos estilos.

## Código Relacionado en MuServer

| Archivo | Paradigma | Descripción |
|--------|----------|-------------|
| User.h | Struct | Estructura de datos |
| Item.h | Clase | Sistema de items |
| Protocol.cpp | Funciones | Procesamiento |
| GameServer.cpp | WinMain | Entry point |

## Ejercicio
1. Leer `Protocol.cpp` líneas 75-150
2. Identificar estilo imperativo vs OOP
3. Analizar cuándo se usa cada uno