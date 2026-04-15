# Proyecto Integrador: Fundamentos de Computación

## Descripción del Proyecto

Aplicar los fundamentos de computación para analizar el sistema de protocolos del MuServer.

## Requisitos

- Analizar la máquina de estados de conexión del servidor
- Entender el flujo de datos binario
- Documentar los protocolos de comunicación

## Código del MuServer

### User.h - Estructura de Usuario

```cpp
struct OBJECTSTRUCT
{
    int m_Index;
    char m_CharName[11];
    int m_Level;
    int m_Experience;
    int m_Life;
    int m_Mana;
    // ...
};
```

### Protocol.cpp -Máquina de Estados

```cpp
enum eUserState 
{
    eUserState_Create = 0,
    eUserState_Playing = 1,
    eUserState_Logged = 2,
    eUserState_Quit = 3,
};

void ProtocolCore(BYTE head, BYTE* lpMsg, int size, int aIndex, int encrypt, int serial) {
    switch(head) {
        case 0x00:
            CGChatRecv((PMSG_CHAT_RECV*)lpMsg, aIndex);
            break;
    }
}
```

## Entregable

1. Diagrama de máquina de estados
2. Documentación de protocolos
3. Análisis de flujo de datos

## Evaluación

- Completitud del diagrama
- Precisión técnica
- Calidad de documentación