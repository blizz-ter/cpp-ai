# Proyecto Integrador: Patrones de Diseño

## Descripción del Proyecto

Refactorizar código del MuServer aplicando patrones de diseño.

## Requisitos

- Aplicar al menos 3 patrones de diseño
- Mejorar arquitectura del código
- Documentar cambios

## Patrones a Aplicar

### 1. Singleton - GameServer

```cpp
class GameServer {
private:
    static GameServer* instance;
    GameServer() {}
    
public:
    static GameServer* GetInstance() {
        if(!instance) {
            instance = new GameServer();
        }
        return instance;
    }
};
```

### 2. Observer - Event System

```cpp
class IEventListener {
public:
    virtual void OnPlayerLogin(int aIndex) = 0;
    virtual void OnPlayerLogout(int aIndex) = 0;
    virtual void OnAttack(int attacker, int target) = 0;
};

class EventManager {
private:
    std::vector<IEventListener*> listeners;
    
public:
    void AddListener(IEventListener* listener);
    void NotifyPlayerLogin(int aIndex);
};
```

### 3. Factory - Item Factory

```cpp
class ItemFactory {
public:
    static ITEM* CreateItem(int type, int level) {
        switch(type) {
            case ITEM_SWORD:
                return new SwordItem(level);
            case ITEM_BOW:
                return new BowItem(level);
            case ITEM_STAFF:
                return new StaffItem(level);
        }
        return nullptr;
    }
};
```

## Entregable

1. Código refactorizado
2. Diagrama de arquitectura
3. Documentación de patrones

## Evaluación

- Aplicación correcta de patrones
- Mejoría en código
- Documentación clara