# Análisis Profundo: Sistema de Skills

## Estructura CSkill

En `Skill.h`:

```cpp
class CSkill {
public:
    int m_Skill;          // Skill index
    int m_Level;         // Skill level
    int m_SP;            // SP requerido
    int m_Caster;
    BYTE m_Durability;
    BYTE m_Number;
    // ...
};
```

---

## Master Skill Tree

Sistema implementado parcialmente en `MasterSkillTree.cpp/h`.

```cpp
class CMasterSkillTree {
public:
    void Init();
    void Load(char* filename);
    int CheckMasterLevel(OBJECTSTRUCT* lpObj);
    void CalcMasterLevelNextExperience(OBJECTSTRUCT* lpObj);
    void GCMasterSkillListSend(int aIndex);
    void GCMasterInfoSend(OBJECTSTRUCT* lpObj);
    int GetMasterSkillValue(OBJECTSTRUCT* lpObj, int skillId);
    int GetMasterSkillLevel(OBJECTSTRUCT* lpObj, int skillId);
    // ...
};
```

### Skills de Maestro (Sample)

| Skill ID | Nombre | Efecto |
|----------|--------|---------|
| MASTER_SKILL_ADD_MANA_SHIELD_IMPROVED | Mana Shield | +Mana shield |
| MASTER_SKILL_ADD_GREATER_DAMAGE_IMPROVED | Greater Damage | +Daño |
| MASTER_SKILL_ADD_GREATER_DEFENSE_IMPROVED | Greater Defense | +Defensa |
| MASTER_SKILL_ADD_SUMMON_LIFE | Summon Life | +Vida summoner |
| MASTER_SKILL_ADD_IRON_DEFENSE | Iron Defense | +Defensa física |

---

## Skill Manager

```cpp
class CSkillManager {
public:
    bool AddSkill(OBJECTSTRUCT* lpObj, int skill);
    bool DelSkill(OBJECTSTRUCT* lpObj, int skill);
    void CGUseSkillRecv(PMSG_USE_SKILL_RECV* lpMsg, int aIndex);
    int GetSkillDamage(int aIndex, int skill, int target);
    // ...

    std::vector<CSkill> m_skilldb;
};
```

---

## Skills por Clase

### Dark Wizard
- Energy Ball
- Fireball
- Meteor
- Teleport
- Healing
- Poison
- Ice Storm
- Flame

### Dark Knight
- Slash
- Power Slash
- Twisting Slash
- Rush
- Stun
- Rageful Blow

### Fairy Elf
- Triple Shot
- Ice Arrow
- Multi-Shot
- Heal

### Magic Gladiator
- Sword Power
- Smash
- Blade Storm

---

## Problemas

### 1. Master Skill Tree incompleto

El sistema está implementado pero muchas functions no terminado:

```cpp
// Faltan funciones:
// - Master Skill tree UI
// - Master level reset
// - Some master skills no funcionales
```

### 2. No hay validación de skill usage

```cpp
// Cliente puede usar skills sin cooldown check
// Posible spam exploited
```

---

## Faltante (según README)

| Sistema | Estado |
|---------|--------|
| Master Skill Tree | ⚠️ Parcial |
| Mu Helper | ❌ No implementado |

---

*Análisis de Skills - MuServer S5U15*