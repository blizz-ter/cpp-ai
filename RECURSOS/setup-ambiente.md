# Setup del Ambiente de Desarrollo

## Windows - Para MuServer

### Requisitos
- Windows 10/11 (64-bit)
- Visual Studio 2019+ (Community funciona)
- SQL Server 2019+ (Express funciona)
- 8GB RAM mínimo
- 20GB disco libre

### Instalación paso a paso

1. **Visual Studio 2019+**
   - Descargar de visualstudio.microsoft.com
   - Instalación personalizada:
     - Desarrollo de escritorio con C++
     - Desarrollo de juegos con C++
     - Componentes: Windows 10/11 SDK

2. **SQL Server**
   - SQL Server Express (gratis)
   - Durante instalación:
     - Autenticación de Windows
     - Puertos: 1433 (default)
   - Instalar SQL Server Management Studio

3. **Compilar el MuServer**
   - Abrir `Source MuServer Update 15/GameServer/GameServer.sln`
   - Build → Build Solution
   - Release|x86 para los 4 servidores

### Puertos常用
| Servicio | Puerto |
|----------|--------|
| ConnectServer | 44401 |
| JoinServer | 44402 |
| DataServer | 44403 |
| GameServer | 44405 |

---

*Setup basado en el currículo de aprendiz.md*
*Equivalente a los primeros 2 años de Licenciatura en Sistemas*