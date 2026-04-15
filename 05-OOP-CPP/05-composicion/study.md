# Composición en C++

## Teoría

### HAS-A Relationship

Composición significa "tiene un":
- Un Player tiene un Inventory
- Un Player tiene un Skills

### Diferencia con Herencia

- Herencia: ES-UN (IS-A)
- Composición: TIENE-UN (HAS-A)

---

## Código del MuServer

### Inventory en Player

```cpp
class CPlayer
{
private:
    CInventory* m_pInventory;
    CSkillSystem* m_pSkills;
    
public:
    CPlayer()
    {
        m_pInventory = new CInventory(76);
        m_pSkills = new CSkillSystem();
    }
    
    ~CPlayer()
    {
        delete m_pInventory;
        delete m_pSkills;
    }
};
```

---

## Ejercicio Práctico

### Ejercicio 1: Composición

Un GameServer TIENE ServerSocket.

### Ejercicio 2: Referencias

Usa referencias en vez de punteros.

### Ejercicio 3: RAII

Garantiza cleanup con destructor.