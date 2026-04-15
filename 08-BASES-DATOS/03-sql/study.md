# SQL - Lenguaje de Consulta Estructurado

## Teoría

### ¿Qué es SQL?

SQL (Structured Query Language) es un lenguaje estándar para gestionar bases de datos relacionales. Permite:
- **DDL** (Data Definition Language): CREATE, ALTER, DROP
- **DML** (Data Manipulation Language): SELECT, INSERT, UPDATE, DELETE
- **DCL** (Data Control Language): GRANT, REVOKE
- **TCL** (Transaction Control Language): COMMIT, ROLLBACK

### Comandos Fundamentales

#### SELECT - Consultar Datos
```sql
SELECT columna1, columna2
FROM tabla
WHERE condicion
ORDER BY columna1 DESC
LIMIT 10;
```

#### INSERT - Insertar Datos
```sql
INSERT INTO tabla (columna1, columna2)
VALUES (valor1, valor2);
```

#### UPDATE - Actualizar Datos
```sql
UPDATE tabla
SET columna1 = nuevo_valor
WHERE condicion;
```

#### DELETE - Eliminar Datos
```sql
DELETE FROM tabla
WHERE condicion;
```

### Tipos de Datos Comunes

- **Numéricos**: INT, BIGINT, FLOAT, DOUBLE, DECIMAL
- **Texto**: CHAR(n), VARCHAR(n), TEXT
- **Fechas**: DATE, DATETIME, TIMESTAMP
- **Booleanos**: BOOLEAN/BIT

### JOINs

```sql
-- INNER JOIN
SELECT * FROM tabla1 INNER JOIN tabla2 ON tabla1.id = tabla2.id;

-- LEFT JOIN
SELECT * FROM tabla1 LEFT JOIN tabla2 ON tabla1.id = tabla2.id;

-- RIGHT JOIN
SELECT * FROM tabla1 RIGHT JOIN tabla2 ON tabla1.id = tabla2.id;
```

### Índices

```sql
-- Índice simple
CREATE INDEX idx_tabla_columna ON tabla(columna);

-- Índice compuesto
CREATE INDEX idx_tabla_cols ON tabla(columna1, columna2);

-- Índice único
CREATE UNIQUE INDEX idx_tabla_columna ON tabla(columna);
```

---

## Ejemplos en MySQL/SQL Server

### Tabla de Jugadores

```sql
CREATE TABLE Account (
    AccountID INT PRIMARY KEY AUTO_INCREMENT,
    Account VARCHAR(10) NOT NULL UNIQUE,
    Password VARCHAR(32) NOT NULL,
    Email VARCHAR(50),
    RegisterDate DATETIME DEFAULT CURRENT_TIMESTAMP,
    LastLogin DATETIME,
    Status TINYINT DEFAULT 1,
    AccountLevel INT DEFAULT 0
);

CREATE INDEX idx_account ON Account(Account);
```

### Tabla de Personajes

```sql
CREATE TABLE Character (
    CharacterID INT PRIMARY KEY AUTO_INCREMENT,
    AccountID INT NOT NULL,
    Name VARCHAR(10) NOT NULL UNIQUE,
    Class TINYINT NOT NULL,
    Level INT DEFAULT 1,
    Experience BIGINT DEFAULT 0,
    Strength INT DEFAULT 18,
    Dexterity INT DEFAULT 18,
    Vitality INT DEFAULT 18,
    Energy INT DEFAULT 18,
    Health INT DEFAULT 100,
    Mana INT DEFAULT 100,
    Map TINYINT DEFAULT 0,
    PosX SMALLINT DEFAULT 100,
    PosY SMALLINT DEFAULT 100,
    ResetCount INT DEFAULT 0,
    MasterLevel INT DEFAULT 0,
    MasterExperience BIGINT DEFAULT 0,
    Money INT DEFAULT 0,
    CreateDate DATETIME DEFAULT CURRENT_TIMESTAMP,
    LastPlay DATETIME,
    FOREIGN KEY (AccountID) REFERENCES Account(AccountID)
);

CREATE INDEX idx_character_name ON Character(Name);
CREATE INDEX idx_character_account ON Character(AccountID);
```

### Tabla de Inventory

```sql
CREATE TABLE Inventory (
    InventoryID INT PRIMARY KEY AUTO_INCREMENT,
    CharacterID INT NOT NULL,
    Slot TINYINT NOT NULL,
    ItemIndex INT NOT NULL,
    ItemLevel TINYINT DEFAULT 0,
    ItemDurability TINYINT DEFAULT 0,
    ItemOption1 TINYINT DEFAULT 0,
    ItemOption2 TINYINT DEFAULT 0,
    ItemOption3 TINYINT DEFAULT 0,
    ItemNewOption TINYINT DEFAULT 0,
    ItemSetOption TINYINT DEFAULT 0,
    ItemSocketOption1 TINYINT DEFAULT 255,
    ItemSocketOption2 TINYINT DEFAULT 255,
    ItemSocketOption3 TINYINT DEFAULT 255,
    ItemSocketOption4 TINYINT DEFAULT 255,
    ItemSocketOption5 TINYINT DEFAULT 255,
    Serial BIGINT DEFAULT 0,
    FOREIGN KEY (CharacterID) REFERENCES Character(CharacterID),
    UNIQUE KEY uk_character_slot (CharacterID, Slot)
);
```

### Tabla de Skills

```sql
CREATE TABLE Skill (
    SkillID INT PRIMARY KEY AUTO_INCREMENT,
    CharacterID INT NOT NULL,
    SkillIndex INT NOT NULL,
    SkillLevel TINYINT DEFAULT 1,
    FOREIGN KEY (CharacterID) REFERENCES Character(CharacterID),
    UNIQUE KEY uk_character_skill (CharacterID, SkillIndex)
);
```

### Consultas Comunes

#### Obtener personajes de una cuenta
```sql
SELECT Name, Class, Level, ResetCount
FROM Character
WHERE AccountID = 1
ORDER BY Level DESC;
```

#### Obtener inventory de un personaje
```sql
SELECT Slot, ItemIndex, ItemLevel, ItemOption1, ItemNewOption
FROM Inventory
WHERE CharacterID = 1
ORDER BY Slot;
```

#### Buscar items de alto nivel
```sql
SELECT c.Name, i.ItemIndex, i.ItemLevel, i.ItemNewOption
FROM Inventory i
INNER JOIN Character c ON i.CharacterID = c.CharacterID
WHERE i.ItemLevel >= 13
ORDER BY i.ItemLevel DESC;
```

#### Top players por level
```sql
SELECT Name, Class, Level, ResetCount, MasterLevel
FROM Character
WHERE Status = 1
ORDER BY Level DESC, MasterLevel DESC, ResetCount DESC
LIMIT 10;
```

#### Actualizar experiencia
```sql
UPDATE Character
SET Experience = Experience + 1000,
    Level = CASE
        WHEN Experience + 1000 >= 10000 THEN Level + 1
        ELSE Level
    END
WHERE CharacterID = 1;
```

#### Eliminar personaje
```sql
DELETE FROM Inventory WHERE CharacterID = 1;
DELETE FROM Skill WHERE CharacterID = 1;
DELETE FROM Character WHERE CharacterID = 1;
```

### Transacciones

```sql
START TRANSACTION;

INSERT INTO Character (AccountID, Name, Class) VALUES (1, 'TestChar', 0);

DECLARE @CharID INT = LAST_INSERT_ID();

INSERT INTO Inventory (CharacterID, Slot, ItemIndex) VALUES (@CharID, 0, 1234);

COMMIT;
-- En caso de error:
-- ROLLBACK;
```

### Stored Procedures

```sql
DELIMITER //

CREATE PROCEDURE CreateCharacter(
    IN p_AccountID INT,
    IN p_Name VARCHAR(10),
    IN p_Class TINYINT
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SELECT -1 AS Result;
    END;
    
    START TRANSACTION;
    
    INSERT INTO Character (AccountID, Name, Class, Level, Experience, 
        Strength, Dexterity, Vitality, Energy, Health, Mana, Map, PosX, PosY)
    VALUES (p_AccountID, p_Name, p_Class, 1, 0, 18, 18, 18, 18, 100, 100, 0, 100, 100);
    
    COMMIT;
    
    SELECT LAST_INSERT_ID() AS CharacterID;
END //

DELIMITER ;
```

---

## Ejercicio Práctico

### Ejercicio 1: Diseñar Esquema

Diseña las tablas SQL para un sistema deGuild:
- Guild: nombre, fecha creación, líder
- GuildMember: usuario, guild, rol, contribución

### Ejercicio 2: Consultas

Escribe las consultas SQL para:
1. Obtener top 5 players por reset
2. Contar cuántositems tiene un personaje
3. Buscar todos los personajes de clase Dark Knight nivel > 300

### Ejercicio 3: Optimización

Un query lento:
```sql
SELECT * FROM Character WHERE Name LIKE '%Dark%';
```

¿Cómo lo optimizarías?

### Ejercicio 4: Triggers

Crea un trigger que registre en una tabla de log cuando se elimine un personaje.

### Ejercicio 5: Mapping con MuServer

Analiza cómo se mapearía `OBJECTSTRUCT` a tablas SQL:

| OBJECTSTRUCT | Tabla SQL |
|--------------|-----------|
| Account | Account |
| Name | Character.Name |
| Level | Character.Level |
| Inventory | Inventory |
| Skill | Skill |

Diseña las consultas necesarias para cargar y guardar un personaje.
