# Registrar un repositorio de PowerShell
Puede configurar PowerShellGet para operar en repositorios internos. Esto se realiza mediante las siguientes adiciones:
- Register-PSRepository: registra un repositorio del usuario actual.
- Unregister-PSRepository: quita un repositorio registrado del usuario actual.
- Set-PSRepository: establece los valores de un repositorio registrado.
- Get-PSRepository: obtiene todos los repositorios registrados del usuario actual.

Una vez registrado un repositorio, puede usar Find-Module e Install-Module para trabajar con él.

```powershell
\#Register a default repository
Register-PSRepository –Name DemoRepo –SourceLocation “https://www.myget.org/F/powershellgetdemo/api/v2” –PublishLocation “<https://www.myget.org/F/powershellgetdemo/api/v2>/package” –InstallationPolicy –Trusted

\#Get all of the registered repositories
Get-PSRepository
Name SourceLocation
---- --------------
PSGallery https://msconfiggallery.cloudapp...
DemoRepo https://www.myget.org/F/powershe...

\#Search only the new repository for modules
Find-Module -Repository DemoRepo
Repository Version Name
---------- ------- ----
DemoRepo 1.0.1 xActiveDirectory
DemoRepo 1.1.1 SomeModule

\#By default, PowerShellGet operates against all registered repositories when none is specified. In this example, the “SomeModule” module is installed from the DemoRepo.
Install-Module SomeModule

\#Removing a repository
Unregister-PSRepository DemoRepo
```<!--HONumber=Mar16_HO2-->
