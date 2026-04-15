# Proyecto Integrador: Bases de Datos

## Descripcion del Proyecto

Analizar los scripts SQL existentes del MuServer y rastrear los protocolos del DataServer. Este proyecto te permite entender como el servidor gestionar datos persistentes y la comunicación con la base de datos.

## Archivos a Estudiar

- `MuServer_Season_5_Update_15/ScriptSql/*.sql` - Scripts SQL del servidor
- `Source MuServer Update 15/DataServer/DataServer/DataServerProtocol.cpp` - Protocolos de DB
- `Source MuServer Update 15/DataServer/DataServer/QueryManager.*` - Gestor de consultas

## Pasos para Completar

### Paso 1: Analizar los Scripts SQL

Explora los scripts en `MuServer_Season_5_Update_15/ScriptSql/`:

1. **Tablas principales**:
   - `MEMB_INFO` - Cuentas de usuarios
   - `Character` - Personajes
   - `Warehouse` - Almacen del personaje
   - `Inventory` - Inventario

2. **Tablas de eventos**:
   - Busca archivos como `Update6 - Procedure WZ_Ranking*.sql`

3. **Stored Procedures**:
   - `WZ_GetCoin` - Sistema de monedas
   - `WZ_DeleteCharacter` - Eliminacion de personaje

### Paso 2: Analizar DataServerProtocol

Lee DataServerProtocol.cpp lineas 36-200:

1. Identifica los protocolos principales:
   - `0x01` - GDCharacterListRecv (lista de personajes)
   - `0x02` - GDCharacterCreateRecv (crear personaje)
   - `0x04` - GDCharacterInfoRecv (info de personaje)
   - `0x07` - GDCreateItemRecv (crear item)

2. Observa el patron:
   ```
   GameServer -> Paquete -> DataServer -> Query SQL -> Respuesta
   ```

### Paso 3: Rastrear un Flujo Completo

Ejemplo: Crear personaje nuevo
```
1. Cliente -> /character create -> GameServer
2. GameServer -> 0x02 -> DataServerProtocolCore
3. GDCharacterCreateRecv -> QueryManager
4. QueryManager->Query("INSERT INTO Character...")
5. QueryManager->Query("INSERT INTO Inventory...")
6. Respuesta al GameServer
```

### Paso 4: Analizar un Stored Procedure

Lee uno de los archivos SQL de procedimientos, por ejemplo:

```
Update3 - Procedure Reward.sql
```

1. Identifica los parametros de entrada
2. Identifica las tablas que modifica
3. Observa las condiciones (WHERE)

### Paso 5: Crear un Script SQL

Crea un script que agregue una tabla de rankings simple:

```sql
-- Tabla de ranking por kills
CREATE TABLE Ranking_Kills (
    CharacterID VARCHAR(10) PRIMARY KEY,
    Kills INT DEFAULT 0,
    Deads INT DEFAULT 0,
    UpdateDate DATETIME DEFAULT GETDATE()
);

-- Procedure para actualizar
CREATE PROCEDURE sp_UpdateRankingKills
    @CharacterName VARCHAR(10),
    @Kills INT,
    @Deads INT
AS
BEGIN
    INSERT INTO Ranking_Kills 
        (CharacterID, Kills, Deads, UpdateDate)
    VALUES 
        (@CharacterName, @Kills, @Deads, GETDATE())
    ON DUPLICATE KEY UPDATE 
        Kills = Kills + @Kills,
        Deads = Deads + @Deads,
        UpdateDate = GETDATE();
END;
```

## Archivos a Modificar

- Crea un archivo `mi_ranking.sql` con tu script

## Como Verificar

1. Entiendes la estructura de las principales tablas
2. Puedes rastrear un flujo de datos completo
3. Puedes escribir un stored procedure funcional

## Recursos Adicionales

- Busca ejemplos en `ScriptSql/Update6 - Procedure*.sql`
- Observa QueryManager.cpp para ver ejecutar SQL