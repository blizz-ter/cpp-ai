# Análisis Profundo: Protocolo de Red

## Estructura de Paquetes

El servidor usa 4 tipos de headers de paquetes:

| Type | Tamaño header | Uso |
|------|---------------|-----|
| C1 | 3 bytes | Paquete normal pequeño |
| C2 | 4 bytes | Paquete grande (>255 bytes) |
| C3 | 3 bytes | Paquete encriptado pequeño |
| C4 | 4 bytes | Paquete encriptado grande |

### Definiciones en Protocol.h

```cpp
struct PBMSG_HEAD {  // C1
    BYTE type;    // 0xC1
    BYTE size;    
    BYTE head;
};

struct PWMSG_HEAD { // C2
    BYTE type;    // 0xC2
    BYTE size[2]; // WORD little-endian
    BYTE head;
};

struct PSBMSG_HEAD { // C1 con subhead
    BYTE type;
    BYTE size;
    BYTE head;
    BYTE subh;
};
```

### Códigos de Protocolo por Idioma

Según `GAMESERVER_LANGUAGE`:

| Language | CODE1 | CODE2 | CODE3 | CODE4 |
|----------|-------|-------|-------|-------|
| 0 (Default) | 0xD3 | 0xD7 | 0xDF | 0x10 |
| 1 | 0xD4 | 0x11 | 0x15 | 0xDB |
| 2 | 0x1D | 0xDC | 0xD6 | 0xD7 |
| 3 | 0xD9 | 0xD7 | 0xD0 | 0x1D |
| 4 | 0x00 | 0x00 | 0x00 | 0x00 |
| 5 | 0xD6 | 0xDD | 0xDF | 0xD2 |
| 6 | 0xDD | 0xD6 | 0xDF | 0x11 |
| 7 | 0xD9 | 0x15 | 0xDC | 0x1D |

## Paquetes Principales (Client → Server)

### Sistema de Autenticación

| Header | Función | Tamaño |
|--------|---------|-------|
| 0xF1 | Login | Variable |
| 0xF3:00 | Request Character List | 3 |
| 0xF3:01 | Create Character | 25 |
| 0xF3:02 | Delete Character | 4 |
| 0xF3:03 | Select Character | 3 |
| 0xF3:04 | Close Game | 3 |

### Sistema de Chat

| Header | Función | Archivo |
|--------|---------|---------|
| 0x00 | Chat Global | CGChatRecv |
| 0x02 | Whisper | CGChatWhisperRecv |

### Sistema de Movimiento

| Header | Función | Archivo |
|--------|---------|---------|
| 0x10 | Walk | CGMoveRecv |
| 0x11 | Sit/Stand | CGSitRecv |
| 0x14 | Get out | CGGetOutRecv |
| 0x15 | Move | CGCharacterMoveRecv |
| 0x16 | Move | CGPositionRecv |

### Sistema de Combate

| Header | Función | Archivo |
|--------|---------|---------|
| 0x18 | Action (ataque) | CGActionRecv |
| 0x19 | Skill Attack | CGSkillAttackRecv |
| 0x1A | Attack (short) | CGAttackRecv |
| PROTOCOL_CODE2 | Attack | gAttack.CGAttackRecv |

### Sistema de Items

| Header | Función | Archivo |
|--------|---------|---------|
| 0x22 | Pick Item | CGItemGetRecv |
| 0x23 | Drop Item | CGItemDropRecv |
| 0x24 | Move Item | CGItemMoveRecv |
| 0x26 | Use Item | CGItemUseRecv |
| 0x14 0x07 | Buy Item | CGBuyReplyRecv |
| 0x14 0x08 | Sell | CGSellRecv |
| 0x30 | Repair | CGNpcRepairRecv |
| 0x31 | Upgrade | CGNpcUpgradeRecv |

### Sistema NPCs

| Header | Función | Archivo |
|--------|---------|---------|
| 0x30 | Talk | CGNpcTalkRecv |
| 0x31 | Trade | CGNpcShopRecv |

### Skills

| Header | Función | Archivo |
|--------|---------|---------|
| 0x19 | Use Skill | CGSkillAttackRecv |
| 0x24 | Skill (use) | CGUseSkillRecv |
| 0x1A | Use Skill | CGMagicAttackRecv |

### Keep-Alive

| Header | Función | Archivo |
|--------|---------|---------|
| 0x0E | Client Alive | CGLiveClientRecv |
| 0x03 | Main Check | CGMainCheckRecv |

---

## Análisis de Seguridad

### AntiHack (COMENTADO!)

En `Protocol.cpp:77`:

```cpp
// ESTO ESTÁ COMENTADO - DEBE DESCOMENTARSE!
// if(gObj[aIndex].Type == OBJECT_USER && 
//    gHackPacketCheck.CheckPacketHack(aIndex,head,((lpMsg[0]==0xC1)?lpMsg[3]:lpMsg[4]),encrypt,serial) == 0)
// {
//     return;
// }
```

### Problemas de Seguridad:

1. ✗ AntiHack comentado - permite packets falsos
2. ✗ Sin validación de coordinates del cliente
3. ✗ Sin validación de damage calculada
4. ✗ Serial check incompleto
5. ⚠ Logs demasiado extensos en producción

---

## Mejora Recomendada

Para habilitar AntiHack, quitar comentarios en `Protocol.cpp:77`:

```cpp
// Descomentar esta sección:
if(gObj[aIndex].Type == OBJECT_USER && 
   gHackPacketCheck.CheckPacketHack(aIndex,head,((lpMsg[0]==0xC1)?lpMsg[3]:lpMsg[4]),encrypt,serial) == 0)
{
    return;
}
```

---

*Análisis del Protocolo - MuServer S5U15*
*Actualizado: Abril 2026*