# Just Enough Administration (JEA): Introducción

## Tabla de contenido
Después de leer este documento, podrá crear, implementar, usar, mantener y auditar una implementación Just Enough Administration (JEA).
Estos son los temas que se tratan en esta guía de introducción:

1.  [Introducción](#introduction): revise brevemente por qué debería interesarle JEA.

2.  [Requisitos previos](#prerequisites): configure el entorno.

3.  [Uso de JEA](#using-jea): entienda la experiencia de usuario de JEA.

4.  [Recreación de la demostración](#remake-the-demo-endpoint): cree una configuración de sesión de JEA desde cero.

5.  [Funcionalidades de rol](#role-capabilities): obtenga información sobre cómo personalizar las funcionalidades JEA con archivos de funcionalidad de rol.

6.  [De un extremo a otro: Active Directory](#end-to-end---active-directory): cree un punto de conexión completamente nuevo para administrar Active Directory.

7.  [Implementación y mantenimiento de varias máquinas](#multi-machine-deployment-and-maintenance): descubra cómo cambia la implementación y la creación según la escala.

8.  [Generación de informes en JEA](#reporting-on-jea): descubra cómo auditar e informar sobre todas las acciones y la infraestructura de JEA.

9.  [Apéndice](#appendix): habilidades importantes y puntos de análisis.

## Introducción

### Motivaciones
Cuando le concede a alguien el acceso con privilegios a los sistemas, está ampliando el límite de confianza a esa persona.
Esto es un riesgo, ya que los administradores son una superficie expuesta a ataques.
Los ataques internos y las credenciales robadas son una realidad habitual.

Este problema no es nuevo.
Probablemente esté familiarizado con el principio del privilegio mínimo y use algún tipo de control de acceso basado en rol (RBAC) con aplicaciones que lo proporcionan.
Pero la eficacia y la facilidad de uso de estas soluciones suelen verse limitadas por su amplio alcance y su imprecisión.
Además, existen lagunas en la cobertura de RBAC.
Por ejemplo, en Windows, el acceso privilegiado es en gran medida un conmutador binario, que le obliga a conceder permisos innecesarios al agregar usuarios al grupo de administradores.

Just Enough Administration (JEA) proporciona una plataforma RBAC a través de Comunicación remota de PowerShell.
*Permite a usuarios específicos realizar tareas administrativas específicas en servidores sin concederles derechos de administrador.*
Esto permite llenar el vacío de las soluciones de RBAC existentes y simplifica la administración de las configuraciones.

## Requisitos previos

### Estado inicial
Antes de comenzar esta sección, compruebe lo siguiente:

1. JEA está disponible en el sistema. Consulte el archivo [LÉAME](./README.md) de los sistemas operativos admitidos actualmente y de las descargas necesarias.
2. Tiene derechos administrativos en el equipo en el que va a probar JEA.
3. El equipo está unido al dominio.
Consulte la sección [Creación de un controlador de dominio](#creating-a-domain-controller) para configurar rápidamente un nuevo dominio en un servidor si aún no tiene uno.

### Habilitar Comunicación remota con PowerShell
La administración con JEA se produce a través de Comunicación remota de PowerShell.
Ejecute lo siguiente en una ventana de administrador de PowerShell para asegurarse de que esté habilitado y configurado correctamente:

```PowerShell
Enable-PSRemoting 
```

Si no está familiarizado con Comunicación remota de PowerShell, sería conveniente ejecutar `Get-Help about_Remote` para obtener información sobre este concepto básico.

### Identificar los usuarios o grupos
Para mostrar JEA en acción, debe identificar los usuarios y grupos no administradores que va a usar a lo largo de esta guía.
  
Si usa un dominio existente, identifique o cree algunos usuarios y grupos sin privilegios.
Les dará acceso a JEA a estos usuarios no administradores.
Cuando vea la variable `$NonAdministrator` en la parte superior de un script, asígnelo a los usuarios o grupos no administradores seleccionados. 

Si crea un dominio desde cero, es mucho más fácil.
Use la sección [Configurar usuarios y grupos](#set-up-users-and-groups) del apéndice para crear usuarios y grupos no administradores.
Los valores predeterminados de `$NonAdministrator` serán los grupos creados en esa sección.

### Configurar el archivo de funcionalidad de rol de mantenimiento
Ejecute los comandos siguientes en PowerShell para crear el archivo de funcionalidad de rol de demostración que usaremos para la sección siguiente.
Más adelante en esta guía, obtendrá información sobre lo que hace este archivo.

```PowerShell
# Fields in the role capability
$MaintenanceRoleCapabilityCreationParams = @{
    Author = 'Contoso Admin'
    CompanyName = 'Contoso'
    VisibleCmdlets = 'Restart-Service'
    FunctionDefinitions = 
            @{ Name = 'Get-UserInfo'; ScriptBlock = { $PSSenderInfo } }
}

# Create the demo module, which will contain the maintenance Role Capability File
New-Item -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module" -ItemType Directory
New-ModuleManifest -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module\Demo_Module.psd1"
New-Item -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module\RoleCapabilities" -ItemType Directory 

# Create the Role Capability file
New-PSRoleCapabilityFile -Path "$env:ProgramFiles\WindowsPowerShell\Modules\Demo_Module\RoleCapabilities\Maintenance.psrc" @MaintenanceRoleCapabilityCreationParams 
```
  
### Crear y registrar el archivo de configuración de sesión de demostración
Ejecute los comandos siguientes para crear y registrar el archivo de configuración de sesión de demostración que usaremos para la sección siguiente.
Más adelante en esta guía, obtendrá información sobre lo que hace este archivo.

```PowerShell
# Determine domain
$domain = (Get-CimInstance -ClassName Win32_ComputerSystem).Domain

# Replace with your non-admin group name
$NonAdministrator = "$domain\JEA_NonAdmin_Operator"

# Specify the settings for this JEA endpoint
# Note: You will not be able to use a virtual account if you are using WMF 5.0 on Windows 7 or Windows Server 2008 R2
$JEAConfigParams = @{
    SessionType = 'RestrictedRemoteServer'
    RunAsVirtualAccount = $true
    RoleDefinitions = @{
        $NonAdministrator = @{ RoleCapabilities = 'Maintenance' }
    }
    TranscriptDirectory = "$env:ProgramData\JEAConfiguration\Transcripts"
}

# Set up a folder for the Session Configuration files
if (-not (Test-Path "$env:ProgramData\JEAConfiguration"))
{
    New-Item -Path "$env:ProgramData\JEAConfiguration" -ItemType Directory
}

# Specify the name of the JEA endpoint
$sessionName = 'JEA_Demo'

if (Get-PSSessionConfiguration -Name $sessionName -ErrorAction SilentlyContinue)
{
    Unregister-PSSessionConfiguration -Name $sessionName -ErrorAction Stop
}

New-PSSessionConfigurationFile -Path "$env:ProgramData\JEAConfiguration\JEADemo.pssc" @JEAConfigParams

# Register the session configuration
Register-PSSessionConfiguration -Name $sessionName -Path "$env:ProgramData\JEAConfiguration\JEADemo.pssc"
```
 
### Habilitar el registro de módulos de PowerShell (opcional)
En los siguientes pasos se habilita el registro para todas las acciones de PowerShell del sistema.
No es necesario habilitarlo para que JEA funcione, pero será útil en la sección [Generación de informes en JEA](#reporting-on-jea).

1. Abra el Editor de directivas de grupo Local
2. Vaya a "Configuración del equipo\Plantillas administrativas\Componentes de Windows\Windows PowerShell".
3. Haga doble clic en "Activar registro de módulos".
4. Haga clic en "Habilitado".
5. En la sección Opciones, haga clic en "Mostrar" junto a Nombres de módulos.
6. Escriba "*" en la ventana emergente. Esto significa que PowerShell registrará los comandos de todos los módulos.
7. Haga clic en Aceptar y aplique la directiva.

Nota: También puede habilitar la transcripción de PowerShell de todo el sistema a través de la directiva de grupo.

**Enhorabuena, ha configurado el equipo con el punto de conexión de demostración y está listo para empezar a trabajar con JEA.**

## Uso de JEA
Esta sección se centra en la comprensión de la experiencia del *uso de JEA* por parte del usuario final.
En la sección Requisitos previos, creó un punto de conexión de JEA de demostración.
Usaremos esta demostración para mostrar JEA en acción.
En secciones posteriores, la guía funcionará con versiones anteriores, es decir, introducirá acciones y archivos que hicieron posible la experiencia del usuario final.

### Uso de JEA como no administrador
Para mostrar JEA en acción, debe usar Comunicación remota de PowerShell como si fuera un usuario sin privilegios de administrador.
Ejecute el comando siguiente en una nueva ventana de PowerShell:   

```PowerShell
$NonAdminCred = Get-Credential
```

Escriba las credenciales de la cuenta que no es de administrador cuando se le solicite.
Si ha seguido la sección [Configurar usuarios y grupos](#set-up-users-and-groups), serán:
-   Nombre de usuario = "OperatorUser"
-   Contraseña = "pa$$w0rd"

Después, ejecute el comando siguiente para conectarse al punto de conexión de demostración mediante las credenciales proporcionadas:

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEA_Demo -Credential $NonAdminCred 
```

Ahora se encuentra en una sesión remota interactiva de PowerShell en la máquina local. Mediante el parámetro "Credential", se ha conectado *como si fuera* OperatorUser (o la cuenta que usase).
El cambio en el símbolo del sistema a `[localhost]: PS>` indica que está trabajando en una sesión remota.  

Ejecute lo siguiente en el símbolo del sistema remoto para mostrar los comandos disponibles:

```PowerShell
Get-Command 
```

Como puede ver, se trata de un subconjunto muy limitado de los comandos disponibles en una ventana normal de PowerShell (que a menudo puede incluir varios miles comandos).
En concreto, solo muestra los 7 cmdlets de JEA predeterminados (Clear-Host, Exit-PSSession, Get-Command, Get-FormatData, Get-Help, Measure-Object, Out-Default, Select-Object) y los dos comandos incluidos explícitamente en el archivo de funcionalidad de rol de mantenimiento.

Ahora echaremos un vistazo al contexto de usuario en el que opera esta sesión. Para ello, invocaremos la función personalizada que se incluye en el archivo de funcionalidad de rol de mantenimiento:

```PowerShell
Get-UserInfo
```
 
La salida de esta función muestra "ConnectedUser" (usuario conectado) y "RunAsUser" (usuario de ejecución).
El usuario conectado es la cuenta que se conectó a la sesión remota (por ejemplo, su cuenta).
No es necesario que el usuario conectado tenga privilegios de administrador.
La cuenta de ejecución es la cuenta que realmente realiza las acciones con privilegios.
El proceso de conectarse como un usuario y realizar la ejecución como un usuario con privilegios permite que los usuarios sin privilegios realicen tareas administrativas específicas sin concederles derechos administrativos.

Para demostrarlo en la práctica, ejecute el siguiente comando:

```PowerShell
Restart-Service -Name Spooler -Verbose
```

Normalmente, Restart-Service requiere que se ejecuten privilegios de administrador.
En cambio, con la cuenta virtual de JEA, podemos ejecutarlo con credenciales sin privilegios.

De este modo, JEA le permite realizar su trabajo con los comandos que ya usa.
Pero ¿qué sucede con los comandos que *no debería* poder usar?
Intente ejecutar un comando diferente en la sesión de JEA, como `Restart-Computer`. Observe que JEA impide que se ejecute este tipo de comandos.

```PowerShell
[localhost]: PS> Restart-Computer
The term 'Restart-Computer' is not recognized as the name of a cmdlet, function, script file, or
operable program. Check the spelling of the name, or if a path was included, verify that the path
is correct and try again.
    + CategoryInfo          : ObjectNotFound: (Restart-Computer:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException 
```

Por último, para salir del punto de conexión restringido de JEA, ejecute el siguiente comando:

```PowerShell
Exit-PSSession
```

Esto lo desconecta de la sesión remota de PowerShell.

## Recreación del punto de conexión de demostración
En esta sección obtendrá información sobre cómo generar una réplica exacta del punto de conexión de demostración usado en la sección anterior.
Aquí se introducirán conceptos básicos que son necesarios para entender JEA, incluidas las configuraciones de sesión de PowerShell y las funcionalidades de rol. 

### Configuraciones de sesión de PowerShell
Cuando usó JEA en la sección anterior, comenzó ejecutando el comando siguiente:

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEA_Demo -Credential $NonAdminCred
```

Aunque la mayoría de los parámetros se explican por sí solos, el parámetro *ConfigurationName* puede parecer confuso al principio.
Este parámetro especifica la configuración de sesión de PowerShell a la que se conecta. 

*Configuración de sesión de PowerShell* es un término sofisticado que hace referencia a un punto de conexión de PowerShell.
Es el "lugar" metafórico donde los usuarios se conectan y obtienen acceso a la funcionalidad de PowerShell.
Según cómo haya establecido una configuración de sesión, puede proporcionar una funcionalidad diferente a los usuarios que se conectan.
En el caso de JEA, se usan configuraciones de sesión para restringir PowerShell a un conjunto limitado de funcionalidades y para ejecutar como una cuenta virtual con privilegios.

Ya tiene varias configuraciones de sesión de PowerShell registradas en la máquina, cada una de ellas configurada de manera ligeramente diferente.
La mayoría viene con Windows, pero la configuración de sesión "JEA_Demo" se configuró en la sección [Requisitos previos](#prerequisites).
Puede ver todas las configuraciones de sesión registradas ejecutando el comando siguiente en un símbolo del sistema de administrador de PowerShell:

```PowerShell
Get-PSSessionConfiguration
```

### Archivos de configuración de sesión de PowerShell
Puede establecer configuraciones de sesión nuevas registrando *archivos nuevos de configuración de sesión de PowerShell*.
Los archivos de configuración de sesión tienen la extensión de archivo ".pssc".
Puede generar archivos de configuración de sesión con el cmdlet New-PSSessionConfigurationFile.

Después, creará y registrará una nueva configuración de sesión para JEA. 

### Generar y modificar la configuración de sesión de PowerShell
Ejecute el comando siguiente para generar un archivo "esqueleto" de configuración de sesión de PowerShell.

```PowerShell
New-PSSessionConfigurationFile -Path "$env:ProgramData\JEAConfiguration\JEADemo2.pssc"
```

> **SUGERENCIA:** En el archivo esqueleto solo se incluyen de forma predeterminada las opciones de configuración más comunes.
> Use el parámetro `-Full` para incluir todas las opciones aplicables en el PSSC generado.

Abra el archivo en PowerShell ISE o en su editor de texto preferido.

```PowerShell
ise "$env:ProgramData\JEAConfiguration\JEADemo2.pssc" 
```

Actualice los campos siguientes en el archivo con los valores indicados (no olvide reemplazarlos en su propio grupo de seguridad no administrador): 

```PowerShell
# OLD: SessionType = 'Default'
SessionType = 'RestrictedRemoteServer'

# OLD: TranscriptDirectory = 'C:\Transcripts\'
TranscriptDirectory = "C:\ProgramData\JEAConfiguration\Transcripts"

# OLD: # RunAsVirtualAccount = $true
RunAsVirtualAccount = $true

# OLD: RoleDefinitions = @{ 'CONTOSO\SqlAdmins' = @{ RoleCapabilities = 'SqlAdministration' }; 'CONTOSO\ServerMonitors' = @{ VisibleCmdlets = 'Get-Process' } }
RoleDefinitions = @{'CONTOSO\JEA_NonAdmin_Operator' = @{ RoleCapabilities =  'Maintenance' }}
```

Esto es lo que significa cada una de las entradas:

1.  El campo *SessionType* define la configuración predeterminada que se va a usar con este punto de conexión.
*RestrictedRemoteServer* define la configuración mínima necesaria para la administración remota. De forma predeterminada, un punto de conexión *RestrictedRemoteServer* expone Get-Command, Get-FormatData, Select-Object, Get-Help, Measure-Object, Exit-PSSession, Clear-Host y Out-Default.
Establece *ExecutionPolicy* en *RemoteSigned* y *LanguageMode* en *NoLanguage*.
El efecto neto de estos valores es un punto de partida seguro y mínimo para configurar el punto de conexión.

2.  El campo *RoleDefinitions* asigna funcionalidades de rol a grupos específicos.
Define quién puede hacer qué como una cuenta con privilegios.
Con este campo, puede especificar la funcionalidad disponible para cualquier usuario que se conecte en función de la pertenencia al grupo.
Este es el núcleo de la funcionalidad RBAC de JEA.
En este ejemplo, expone la funcionalidad de rol "de demostración" creada previamente a los miembros del grupo "Contoso\JEA_NonAdmin_Operator".

3.  El campo *RunAsVirtualAccount* indica que PowerShell debe "ejecutarse como" una cuenta virtual en este punto de conexión.
De forma predeterminada, la cuenta virtual es miembro del grupo de administradores integrado.
En un controlador de dominio, también es miembro del grupo de administradores de dominio de forma predeterminada.
Más adelante en esta guía, obtendrá información sobre cómo personalizar los privilegios de la cuenta virtual.

4.  El campo *TranscriptDirectory* define dónde se guardan las transcripciones "con consentimiento temporal" de PowerShell después de cada sesión remota.
Estas transcripciones permiten inspeccionar las acciones realizadas en cada sesión de manera legible.
Para obtener más información sobre las transcripciones de PowerShell, consulte esta [entrada de blog](http://blogs.msdn.com/b/powershell/archive/2015/06/09/powershell-the-blue-team.aspx).
Nota: Los Eventos de Windows normales también capturan información sobre lo que cada usuario ejecutó con PowerShell.
Las transcripciones son simplemente un poco más legibles.

Por último, guarde los cambios en *JEADemo2.pssc*.

### Aplicar la configuración de sesión de PowerShell 

Para crear un punto de conexión a partir de un archivo de configuración de sesión, debe registrar el archivo.
Para ello, se necesita cierta información:

1. La ruta de acceso al archivo de configuración de sesión.
2. El nombre de la configuración de sesión registrada. Este es el mismo nombre que los usuarios proporcionan al parámetro "ConfigurationName" cuando se conectan al punto de conexión con `Enter-PSSession` o `New-PSSession`.

Para registrar la configuración de sesión en la máquina local, ejecute el comando siguiente:

```PowerShell
Register-PSSessionConfiguration -Name 'JEADemo2' -Path "$env:ProgramData\JEAConfiguration\JEADemo2.pssc"
```

Enhorabuena, ha configurado el punto de conexión de JEA.

### Probar el punto de conexión
Vuelva a ejecutar los pasos enumerados en la sección [Uso de JEA](#using-jea) con el nuevo punto de conexión para confirmar que funciona según lo previsto.
Asegúrese de usar el nombre nuevo del punto de conexión (JEADemo2) al proporcionar el nombre de la configuración a Enter-PSSession.

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEADemo2 -Credential $NonAdminCred
```

### Conceptos clave
**Configuración de sesión de PowerShell**: denominada también *punto de conexión de PowerShell*, es el "lugar" metafórico donde los usuarios se conectan y obtienen acceso a la funcionalidad de PowerShell.
Puede enumerar las configuraciones de sesión registradas en el sistema ejecutando `Get-PSSessionConfiguration`.
Cuando se configura de forma específica, la configuración de sesión de PowerShell también se puede denominar *punto de conexión de JEA*.

**Archivo de configuración de sesión de PowerShell (.pssc)**: archivo que, cuando se registra, define los valores de una configuración de sesión de PowerShell.
Contiene especificaciones para los roles de usuario que pueden conectarse al punto de conexión, la cuenta virtual de ejecución y mucho más.     

**Definiciones de rol**: campo de un archivo de configuración de sesión que define las funcionalidades de rol concedidas a los usuarios que se conectan.
Define *quién* puede hacer *qué* como una cuenta con privilegios.
Este es el núcleo de las funcionalidades RBAC de JEA.

**SessionType**: campo del archivo de configuración de sesión que representa la configuración predeterminada de una configuración de sesión.
En el caso de los puntos de conexión de JEA, debe establecerse en RestrictedRemoteServer.

**Transcripción de PowerShell**: archivo que contiene una vista "con consentimiento temporal" de una sesión de PowerShell.
Puede establecer que PowerShell genere transcripciones para sesiones de JEA mediante el campo TranscriptDirectory.
Para obtener más información sobre las transcripciones, consulte esta [entrada de blog](https://technet.microsoft.com/en-us/magazine/ff687007.aspx).

## Funcionalidades de rol

### Introducción
En la sección anterior, aprendió que el campo RoleDefinitions define los grupos que tienen acceso a las funcionalidades de rol.
Probablemente se pregunte qué son las funcionalidades de rol.
En esta sección responderemos a esta pregunta.  

## Introducción a las funcionalidades de rol de PowerShell
Las funcionalidades de rol de PowerShell definen qué puede hacer un usuario en un punto de conexión de JEA.
Detallan una lista blanca de elementos como comandos visibles, aplicaciones visibles, etc.
Las funcionalidades de rol se definen mediante archivos con la extensión ".psrc".

## Contenido de las funcionalidades de rol
Empezaremos examinando y modificando el archivo de funcionalidad de rol de demostración que usó anteriormente.
Imagine que ha implementado la configuración de sesión en su entorno, pero de acuerdo con los comentarios recibidos debe cambiar las funcionalidades expuestas a los usuarios.
Los operadores necesitan poder reiniciar las máquinas y también quieren poder obtener información sobre la configuración de red.
Además, el equipo de seguridad le ha informado de no es aceptable permitir que los usuarios ejecuten "Restart-Service" sin restricciones.
Debe restringir los servicios que pueden reiniciar los operadores.

Para realizar estos cambios, primero ejecute PowerShell ISE como administrador y abra el archivo siguiente:

```
C:\Program Files\WindowsPowerShell\Modules\Demo_Module\RoleCapabilities\Maintenance.psrc
```

Ahora busque y actualice las líneas siguientes en el archivo: 

```PowerShell
# OLD: VisibleCmdlets = 'Restart-Service'
VisibleCmdlets = 'Restart-Computer',
                 @{
                     Name = 'Restart-Service'
                     Parameters = @{ Name = 'Name'; ValidateSet = 'Spooler' }
                 },
                 'NetTCPIP\Get-*'

# OLD: VisibleExternalCommands = 'Item1', 'Item2'
VisibleExternalCommands = 'C:\Windows\system32\ipconfig.exe'
```

Esto contiene algunos ejemplos interesantes:

1.  Ha restringido Restart-Service de modo que los operadores solo podrán usar Restart-Service con el parámetro - Name y solo podrán proporcionar "Spooler" como argumento para ese parámetro.
Si quiere, también puede restringir los argumentos con una expresión regular usando "ValidatePattern" en vez de "ValidateSet".

2.  Ha expuesto todos los comandos con el verbo "Get" desde el módulo NetTCPIP.
Dado que los comandos "Get" no suelen cambiar el estado del sistema, es una acción relativamente segura.
Aun así, se recomienda encarecidamente que examine todos los comandos que expone a través de JEA.

3.  Ha expuesto un ejecutable (ipconfig) mediante VisibleExternalCommands.
También puede exponer los scripts de PowerShell completos con este campo.
Es importante que siempre proporcione la ruta de acceso completa de los comandos externos para asegurarse de que no se ejecute en su lugar un programa con el mismo nombre (y potencialmente malicioso) colocado en la ruta de acceso del usuario.

Guarde el archivo y conéctese de nuevo al punto de conexión de demostración para confirmar que los cambios han funcionado.

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEADemo2 -Credential $NonAdminCred
Get-Command
```
Como solo ha modificado el archivo de funcionalidad de rol, no hace falta que vuelva a registrar la configuración de sesión.
PowerShell buscará automáticamente la funcionalidad de rol actualizada cuando un usuario se conecte.
Puesto que las funcionalidades de rol se cargan cuando se inicia la sesión, las sesiones existentes no se ven afectadas por las actualizaciones de los archivos de funcionalidad de rol.

Ahora, confirme que puede reiniciar el equipo ejecutando Restart-Computer con el parámetro -WhatIf (a menos que realmente quiera reiniciar el equipo).

```PowerShell
Restart-Computer -WhatIf 
```

Confirme que puede ejecutar "ipconfig".

```PowerShell
ipconfig
```

Por último, confirme que Restart-Service solo funciona para el servicio de trabajos de impresión.

```PowerShell
Restart-Service Spooler # This should work
Restart-Service WSearch # This should fail 
```

Salga de la sesión cuando haya terminado.

```PowerShell
Exit-PSSession 
```

### Creación de funcionalidades de rol
En la siguiente sección, creará un punto de conexión de JEA para los usuarios del departamento de soporte técnico de AD.
Como preparación, crearemos un archivo de funcionalidad de rol en blanco para rellenarlo para esa sección.
Las funcionalidades de rol deben crearse dentro de una carpeta "RoleCapabilities" en un módulo de PowerShell válido para que se resuelvan cuando se inicie una sesión.

Los módulos de PowerShell son básicamente paquetes de funcionalidad de PowerShell.
Pueden contener funciones de PowerShell, cmdlets, recursos de DSC, funcionalidades de rol y mucho más.
Encontrará información sobre los módulos si ejecuta `Get-Help about_Modules` en una consola de PowerShell.

Para crear un nuevo módulo de PowerShell con un archivo de funcionalidad de rol en blanco, ejecute los comandos siguientes:  

```PowerShell
# Create a new folder for the module.
New-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module' -ItemType Directory

# Add a module manifest to contain metadata for this module.
New-ModuleManifest -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\Contoso_AD_Module.psd1' -RootModule Contoso_AD_Module.psm1

# Create a blank script module. You'll use this for custom functions in the next section.
New-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\Contoso_AD_Module.psm1' -ItemType File 

# Create a RoleCapabilities folder in the AD_Module folder. PowerShell expects Role Capabilities to be located in a "RoleCapabilities" folder within a module.
New-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\RoleCapabilities' -ItemType Directory

# Create a blank Role Capability in your RoleCapabilities folder. Running this command without any additional parameters just creates a blank template.
New-PSRoleCapabilityFile -Path 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\RoleCapabilities\ADHelpDesk.psrc' 
```

Enhorabuena, ha creado un archivo de funcionalidad de rol en blanco.
Lo usará en la sección siguiente.

### Conceptos clave
**Funcionalidad de rol (.psrc)**: archivo que define qué puede hacer un usuario en un punto de conexión de JEA.
Detalla una lista blanca de elementos como comandos visibles, aplicaciones de consola visibles, etc.
Para que PowerShell detecte las funcionalidades de rol, debe colocarlas en una carpeta "RoleCapabilities" en un módulo de PowerShell válido.

**Módulo de PowerShell**: paquete de funcionalidad de PowerShell.
Puede contener funciones de PowerShell, cmdlets, recursos de DSC, funcionalidades de rol y mucho más.
Para que se carguen automáticamente, los módulos de PowerShell deben estar ubicados en una ruta de acceso en `$env:PSModulePath`. 

## De un extremo a otro: Active Directory
Imagine que el ámbito de su programa ha aumentado.
Ahora es responsable de agregar JEA a controladores de dominio para realizar acciones de Active Directory.
El personal del departamento de soporte técnico va a usar JEA para desbloquear cuentas, restablecer contraseñas y otras acciones similares.

Debe exponer un conjunto de comandos completamente nuevo para un grupo diferente de personas.
Además, tiene que exponer numerosos scripts existentes de Active Directory.
Esta sección le guiará por el proceso de creación de una configuración de sesión y funcionalidad de rol para esta tarea.

### Requisitos previos
Para seguir esta sección paso a paso, debe trabajar en un controlador de dominio.
Si no tiene acceso al controlador de dominio, no se preocupe.
Intente seguir los pasos trabajando en otro escenario o rol con el que esté familiarizado.
Si quiere configurar rápidamente un nuevo controlador de dominio, consulte la sección [Creación de un controlador de dominio](#creating-a-domain-controller) del apéndice.

### Pasos para crear una nueva funcionalidad de rol y configuración de sesión

La tarea de crear una nueva funcionalidad de rol puede parecer desalentadora al principio, pero puede dividirse en pasos bastante simples:

1.  Identificar las tareas que se deben habilitar
2.  Restringir las tareas según sea necesario
3.  Confirmar que funcionan con JEA
4.  Colocarlas en un archivo de funcionalidad de rol
5.  Registrar una configuración de sesión que exponga esa funcionalidad de rol

### Paso 1: identificar lo que se debe exponer
Antes de crear una nueva funcionalidad de rol o configuración de sesión, debe identificar todas las acciones que los usuarios tendrán que realizar mediante el punto de conexión de JEA, así como la manera en que se llevarán a cabo a través de PowerShell.
Esto implica llevar a cabo una extensa recopilación e investigación de requisitos.
La manera en que realice este proceso dependerá de su organización y de sus objetivos.
Es importante destacar que la recopilación y la investigación de los requisitos es una parte fundamental del proceso real.
Este podría ser el paso más difícil del proceso de adopción de JEA.

#### Buscar recursos
A continuación se indica una serie de recursos en línea que podría haber encontrado en su investigación para crear un punto de conexión de administración de Active Directory:
-   [Introducción a Active Directory PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2009/03/05/active-directory-powershell-overview.aspx) 
-   [Guía de CMD a PowerShell para Active Directory](http://blogs.technet.com/b/ashleymcglone/archive/2013/01/02/free-download-cmd-to-powershell-guide-for-ad.aspx)

#### Hacer una lista
A continuación se muestran diez acciones con las que trabajará en el resto de la sección.
Tenga en cuenta que se trata básicamente de un ejemplo y que los requisitos de las organizaciones podrían ser diferentes:

|Acción                                                         |Comando de PowerShell                                             |
|---------------------------------------------------------------|---------------------------------------------------------------|
|Desbloquear cuenta                                                 |`Unlock-ADAccount`                                             |
|Restablecimiento de contraseña                                                 |`Set-ADAccountPassword` y `Set-ADUser -ChangePasswordAtLogon`|
|Cambiar el cargo de un usuario                                          |`Set-ADUser -Title`                                            |  
|Buscar cuentas de AD que estén bloqueadas, deshabilitadas, inactivas, etc. |`Search-ADAccount`                                             | 
|Agregar un usuario a un grupo                                              |`Add-ADGroupMember -Identity (with whitelist) -Members`        | 
|Quitar un usuario de un grupo                                         |`Remove-ADGroupMember -Identity (with whitelist) -Members`     | 
|Habilitar una cuenta de usuario                                          |`Enable-ADAccount`                                             |
|Deshabilitar una cuenta de usuario                                         |`Disable-ADAccount`                                            |

### Paso 2: restringir las tareas según sea necesario

Ahora que tiene la lista de acciones, debe considerar detenidamente las capacidades de cada comando.
Hay dos razones importantes para hacerlo:

1.  Es fácil exponer a los usuarios más funcionalidades de las previstas.
Por ejemplo, `Set-ADUser` es un comando increíblemente eficaz y flexible.
Probablemente no le interese exponer todo lo que puede hacer para ayudar a los usuarios del departamento de soporte técnico.  

2.  Peor aún, es posible exponer los comandos que permiten a los usuarios evitar las restricciones de JEA.
Si esto ocurre, JEA deja de funcionar como límite de seguridad.
Tenga cuidado al seleccionar comandos.
Por ejemplo, Invoke-Expression permitirá que los usuarios ejecuten código sin restricciones.
Para obtener más información sobre este tema, consulte la sección Consideraciones al limitar comandos.

Después de revisar cada comando, decide restringir lo siguiente:

1.  `Set-ADUser` solo se debe poder ejecutar con el parámetro "-Title". 

2.  `Add-ADGroupMember` y `Remove-ADGroupMember` solo deben funcionar con determinados grupos.

### Paso 3: confirmar que las tareas funcionan con JEA
En realidad, el uso de estos cmdlets podría no ser sencillo en el entorno de JEA restringido.
JEA se ejecuta en el modo *Sin lenguaje* que, entre otras cosas, impide que los usuarios usen variables.
Para asegurarse de que los usuarios finales tengan una experiencia sin problemas, es importante comprobar ciertas cosas.

Por ejemplo, considere el comando `Set-ADAccountPassword`.
El parámetro "-NewPassword" requiere una cadena segura.
A menudo, los usuarios crean una cadena segura y la pasan como variable (como puede verse más abajo):

```PowerShell
$newPassword = (Read-Host -Prompt "Specify a new password" -AsSecureString)
Set-ADAccountPassword -Identity mollyd -NewPassword $newPassword -Reset
```

Pero el modo Sin lenguaje impide el uso de variables.
Puede evitar esta restricción de dos maneras:

1.  Puede requerir a los usuarios que ejecuten el comando sin asignar variables.
Esto es fácil de configurar, pero puede ser complicado para los operadores que usan el punto de conexión.
A nadie le gusta la idea de tener que escribir lo siguiente cada vez que restablece una contraseña.
```PowerShell
Set-ADAccountPassword -Identity mollyd -NewPassword (Read-Host -Prompt "Specify a new password" -AsSecureString) -Reset
```

2.  Puede ajustar la complejidad de un script o una función que exponga a los usuarios finales.
Los scripts y las funciones que se exponen se ejecutan en un contexto sin restricciones; puede escribir funciones que usen variables y llamen a otros comandos sin ningún problema.
Este enfoque simplifica la experiencia del usuario final, evita errores, reduce los conocimientos necesarios de PowerShell y reduce la exposición involuntaria de demasiadas funcionalidades.
La única desventaja es el costo de crear y mantener la función.

#### Inciso: Agregar una función al módulo
Si adopta el segundo enfoque, tendrá que escribir una función de PowerShell denominada `Reset-ContosoUserPassword`.
Esta función se encarga de todo lo que debe ocurrir cuando se restablece una contraseña de usuario.
En su organización, esto podría obligarle a hacer cosas sofisticadas y complicadas.
Ya que esto es solo un ejemplo, el comando simplemente restablecerá la contraseña y solicitará al usuario que cambie la contraseña al iniciar sesión.
Lo colocaremos en el módulo Contoso_AD_Module que creó en la última sección.

1. En PowerShell ISE, abra "Contoso_AD_Module.psm1".
```PowerShell
ISE 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\Contoso_AD_Module.psm1' 
```

2. Pulse Ctrl+J para abrir el menú de fragmentos de código.

3. Pulse la tecla abajo hasta que encuentre "function" y pulse ENTRAR.
Esto mostrará un esqueleto muy básico de una función.

4. Cambie el nombre de la función a "Reset-ContosoUserPassword".  

5. Cambie el nombre de uno de los parámetros a "Identity" y elimine el segundo.

6. Copie lo siguiente en el cuerpo de la función.
```PowerShell
# Get the new password
$NewPassword = Read-Host -Prompt "Enter a new password" -AsSecureString
# Reset the password
Set-ADAccountPassword -Identity $Identity -NewPassword $NewPassword -Reset
# Require the user to reset at next logon
Set-ADUser -Identity $Identity -ChangePasswordAtLogon
```

7. Guarde el archivo.
Debería obtener un resultado parecido a este:
```PowerShell
function Reset-ContosoUserPassword ($Identity)
{
# Get the new password
$NewPassword = Read-Host -Prompt "Enter a new password" -AsSecureString
# Reset the password
Set-ADAccountPassword -Identity $Identity -NewPassword $NewPassword -Reset
# Require the user to reset at next logon
Set-ADUser -Identity $Identity -ChangePasswordAtLogon
} 
```
Ahora, los usuarios pueden llamar simplemente a `Reset-ContosoUserPassword` y no tienen que recordar la sintaxis para crear una cadena segura de insertada.

### Paso 4: editar el archivo de funcionalidad de rol
En la sección [Creación de funcionalidades de rol](#role-capability-creation), creó un archivo de funcionalidad de rol en blanco.
En esta sección, rellenará los valores de ese archivo.

En primer lugar, abra el archivo de funcionalidad de rol en ISE.
```PowerShell
ise 'C:\Program Files\WindowsPowerShell\Modules\Contoso_AD_Module\RoleCapabilities\ADHelpDesk.psrc' 
```
Actualice el archivo con los cambios siguientes:
```PowerShell
# OLD: VisibleCmdlets = 'Invoke-Cmdlet1', @{ Name = 'Invoke-Cmdlet2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }
VisibleCmdlets =
    'Unlock-ADAccount',
    'Search-ADAccount',
    'Enable-ADAccount',
    'Disable-ADAccount',
    @{ Name = 'Set-ADUser'; Parameters = @{ Name = 'Title'; ValidateSet = 'Manager', 'Engineer' }},
    @{ Name = 'Add-ADGroupMember'; Parameters = 
        @{Name = 'Identity'; ValidateSet = 'TestGroup'},
        @{Name = 'Members'}},
    @{ Name = 'Remove-ADGroupMember'; Parameters = 
        @{Name = 'Identity'; ValidateSet = 'TestGroup'},
        @{Name = 'Members'}}
  
# OLD: VisibleFunctions = 'Invoke-Function1', @{ Name = 'Invoke-Function2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }   
VisibleFunctions = 'Reset-ContosoUserPassword'
```

Hay algunos aspectos que debe tener en cuenta sobre lo anterior:
1.  PowerShell intentará cargar automáticamente los módulos necesarios para la funcionalidad de rol.
Podría tener que mostrar explícitamente los nombres de los módulos en el campo "ModulesToImport" si experimenta problemas con un módulo porque no se carga automáticamente.

2.  Si no está seguro de si un comando es un cmdlet o una función, ejecute `Get-Command` y observe el valor "CommandType".

3.  ValidatePattern permite usar una expresión regular para restringir los argumentos del parámetro si no resulta sencillo definir un conjunto de valores permitidos.
No puede definir ValidatePattern y ValidateSet para un solo parámetro.

### Paso 5: registrar una nueva configuración de sesión
A continuación, creará un nuevo archivo de configuración de sesión que expondrá la funcionalidad de rol a los miembros del grupo "JEA_NonAdmin_HelpDesk" de AD. 

Primero cree y abra un nuevo archivo de configuración de sesión en blanco en PowerShell ISE.
```PowerShell
New-PSSessionConfigurationFile -Path "$env:ProgramData\JEAConfiguration\HelpDeskDemo.pssc" 
ise "$env:ProgramData\JEAConfiguration\HelpDeskDemo.pssc"
```
Modifique los campos siguientes en el archivo PSSC.
Si trabaja en su propio entorno, debe reemplazar "CONTOSO\JEA_NonAdmins_Helpdesk" por su propio usuario o grupo sin privilegios de administrador.
```PowerShell
# OLD: Description = ''
Description = 'An endpoint for active directory tasks.' 

# OLD: SessionType = 'Default'
SessionType = 'RestrictedRemoteServer'

# OLD: TranscriptDirectory = 'C:\Transcripts\'
TranscriptDirectory = "C:\ProgramData\JEAConfiguration\Transcripts"

# OLD: RunAsVirtualAccount = $true
RunAsVirtualAccount = $true

# OLD: RoleDefinitions = @{ 'CONTOSO\SqlAdmins' = @{ RoleCapabilities = 'SqlAdministration' }; 'CONTOSO\ServerMonitors' = @{ VisibleCmdlets = 'Get-Process' } }
RoleDefinitions = @{ 'CONTOSO\JEA_NonAdmin_HelpDesk' = @{ RoleCapabilities =  'ADHelpDesk' }} 
```
Guarde y registre la configuración de sesión.
```PowerShell
Register-PSSessionConfiguration -Name ADHelpDesk -Path "$env:ProgramData\JEAConfiguration\HelpDeskDemo.pssc" 
```
### Pruébela
Obtenga las credenciales de usuario sin privilegios de administrador:
```PowerShell
$HelpDeskCred = Get-Credential
```
Si ha seguido la sección Configurar usuarios y grupos, las credenciales serán:
-   Nombre de usuario = "HelpDeskUser"
-   Contraseña = "pa$$w0rd"

Obtenga acceso de manera remota al punto de conexión del departamento de soporte técnico de AD mediante la credencial sin privilegios de administrador:
```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName ADHelpDesk -Credential $HelpDeskCred 
```
Use Set-ADUser para restablecer el cargo de un usuario.
```PowerShell
Set-ADUser -Identity OperatorUser -Title Engineer 
```
Compruebe que el cargo ha cambiado.
```PowerShell
Get-ADUser -Identity OperatorUser -Property Title 
```
Ahora, use Add-ADGroupMember para agregar un usuario a TestGroup.
Nota: Asegúrese de haber creado con antelación el grupo denominado TestGroup.
```PowerShell
Add-ADGroupMember TestGroup -Member OperatorUser -Verbose 
```
Salga de la sesión:
```PowerShell
Exit-PSSession
```
### Conceptos clave
**Modo NoLanguage**: cuando PowerShell está en modo "NoLanguage", los usuarios solo pueden ejecutar comandos; no pueden usar elementos del lenguaje.
Para más información, vea `Get-Help about_Language_Modes`.

**Funciones de PowerShell**: las funciones de PowerShell son fragmentos de código de PowerShell que se puede llamar por su nombre.
Para más información, vea `Get-Help about_Functions`.

**ValidateSet/ValidatePattern**: al exponer un comando, puede restringir los argumentos válidos para parámetros específicos.
ValidateSet es una lista específica de comandos válidos.
ValidatePattern es una expresión regular con la que deben coincidir los argumentos de ese parámetro.

## Implementación y mantenimiento de varias máquinas
En este momento, ha implementado JEA en sistemas locales varias veces.
Dado que su entorno de producción probablemente esté formado por más de una máquina, es importante que siga los pasos fundamentales del proceso de implementación que deben repetirse en cada máquina.

### Pasos de alto nivel:
1.  Copie los módulos (con funcionalidades de rol) en cada nodo.
2.  Copie los archivos de configuración de sesión en cada nodo.
3.  Ejecute `Register-PSSessionConfiguration` con la configuración de sesión.
4.  Conserve una copia de la configuración de sesión y de los kits de herramientas en una ubicación segura.
Como realizará modificaciones, es conveniente tener un "origen único de verdad."

### Script de ejemplo
A continuación se muestra un script de ejemplo para la implementación.
Para usarlo en su entorno, deberá usar los nombres o las rutas de acceso de recursos compartidos de archivos y módulos reales.
```PowerShell
# First, copy the session configuration and modules (containing role capability files) onto a file share you have access to.
Copy-Item -Path 'C:\Demo\Demo.pssc' -Destination '\\FileShare\JEA\Demo.pssc'
Copy-Item -Path 'C:\Program Files\WindowsPowerShell\Modules\SomeModule\' -Recurse -Destination '\\FileShare\JEA\SomeModule'

# Next, author a setup script (C:\JEA\Deploy.ps1) to run on each individual node
    # Contents of C:\JEA\Deploy.ps1
    New-Item -ItemType Directory -Path C:\JEADeploy
    Copy-Item -Path '\\FileShare\JEA\Demo.pssc' -Destination 'C:\JEADeploy\'
    Copy-Item -Path '\\FileShare\JEA\SomeModule' -Recurse -Destination 'C:\Program Files\WindowsPowerShell\Modules' # Remember, Role Capability Files are found in modules
    if (Get-PSSessionConfiguration -Name JEADemo -ErrorAction SilentlyContinue)
    {
        Unregister-PSSessionConfiguration -Name JEADemo -ErrorAction Stop
    }

    Register-PSSessionConfiguration -Name JEADemo -Path 'C:\JEADeploy\Demo.pssc'
    Remove-Item -Path 'C:\JEADeploy' # Don't forget to clean up!

# Now, invoke the script on all of the target machines.
# Note: this requires PowerShell Remoting be enabled on each machine. Enabling PowerShell remoting is a requirement to use JEA as well.
# You may need to provide the "-Credential" parameter if your current user account does not have admin permissions on these machines.
Invoke-Command –ComputerName 'Node1', 'Node2', 'Node3', 'NodeN' -FilePath 'C:\JEA\Deploy.ps1'

# Finally, delete the session configuration and role capability files from the file share.
Remove-Item -Path '\\FileShare\JEA\Demo.pssc'
Remove-Item -Path '\\FileShare\JEA\SomeModule' -Recurse
```
### Modificar funcionalidades
Cuando se trabaja con varias máquinas, es importante que las modificaciones se implementen de forma coherente.
Una vez que JEA tenga un recurso de DSC, podrá asegurarse más fácilmente de que el entorno esté sincronizado.
Hasta ese momento, se recomienda que conserve una copia maestra de las configuraciones de sesión y que vuelva a implementarla cada vez que realice una modificación.

### Quitar funcionalidades
Para quitar la configuración de JEA de sus sistemas, use el comando siguiente en cada máquina:
```PowerShell
Unregister-PSSessionConfiguration -Name JEADemo 
```
## Generación de informes en JEA
El registro y la auditoría son muy importantes, ya que JEA permite que usuarios sin privilegios se ejecuten en un contexto con privilegios.
En esta sección veremos las herramientas que puede usar como ayuda para el registro y la creación de informes.

### Acciones de generación de informes en JEA
#### Transcripción con consentimiento temporal
Una de las formas más rápidas de obtener un resumen de lo que ocurre en una sesión de PowerShell es mirar por encima del hombro de la persona que está escribiendo.
De este modo ve los comandos y la salida de los comandos y comprueba que todo está bien.
O no, pero al menos lo sabe.
La transcripción de PowerShell está diseñada para proporcionarle una vista similar a posteriori.

Cuando se usa el campo "TranscriptDirectory" en la configuración de sesión, PowerShell graba automáticamente una transcripción de todas las acciones realizadas en una sesión determinada.
Verá transcripciones de sus sesiones en el documento que se encuentra aquí: "$env:ProgramData\JEAConfiguration\Transcripts".

Como puede imaginar, la transcripción registra información sobre el usuario conectado, el usuario de ejecución, los comandos que se ejecutaron en la sesión, etc.
Para obtener más información sobre la transcripción de PowerShell, consulte [esta entrada de blog](http://blogs.msdn.com/b/powershell/archive/2015/06/09/powershell-the-blue-team.aspx).

#### Registros de eventos de PowerShell
Una vez que ha activado el registro de módulos, también se registran todas las acciones de PowerShell en los registros de eventos de Windows normales.
Esto es ligeramente más complicado que las transcripciones, pero el nivel de detalle que ofrece puede resultarle útil.

En el registro operativo "PowerShell", el identificador de evento 4104 registrará cada comando que se invoque si ha habilitado el registro del módulos.

#### Otros registros de eventos
A diferencia de los registros y las transcripciones de PowerShell, otros mecanismos de registro no capturan al "usuario conectado".
Deberá establecer alguna correlación entre los demás registros y los registros de PowerShell para que las acciones realizadas coincidan.

En el registro operativo "Administración remota de Windows", el identificador de evento 193 registrará el SID y el nombre del usuario que se conecta, así como el SID de la cuenta virtual de ejecución para ayudar a esta correlación.
Probablemente habrá observado que el nombre de la cuenta virtual de ejecución incluye al final el dominio y el nombre del usuario que se conecta.

### Configuración de la generación de informes en JEA
#### Get-PSSessionConfiguration
Para realizar informes sobre el estado de su entorno, es importante saber cuántos puntos de conexión de JEA ha configurado en la máquina.
`Get-PSSessionConfiguration` hace precisamente eso.
 
#### Get-PSSessionCapability
La realización manual de informes sobre las funcionalidades de un usuario dado a través de un punto de conexión de JEA puede ser bastante compleja.
Probablemente tendría que inspeccionar varias funcionalidades de rol.
Por suerte, el cmdlet "Get-PSSessionCapability" lo hace automáticamente.

Para probarlo, ejecute el comando siguiente desde un símbolo del sistema de administrador de PowerShell:
```PowerShell
Get-PSSessionCapability -Username 'CONTOSO\OperatorUser' -ConfigurationName JEADemo
```
## Conclusión 
Cuando complete esta guía, dispondrá de las herramientas y el vocabulario necesarios para crear su propio punto de conexión de JEA. Gracias por leerla.

## Apéndice

## Conceptos clave usados a lo largo de esta guía
**Comunicación remota de PowerShell**: permite ejecutar comandos de PowerShell en máquinas remotas.
Puede operar en uno o varios equipos y usar conexiones temporales o permanentes.
En esta demostración, obtuvo acceso de manera remota a la máquina local con una sesión interactiva.
JEA restringe la funcionalidad disponible a través de Comunicación remota de PowerShell.
Para obtener más información sobre Comunicación remota de PowerShell, ejecute `Get-Help about_Remote`.

**Usuario de ejecución**: en JEA, un usuario sin privilegios de administrador se ejecuta como una cuenta virtual con privilegios.
La cuenta virtual solo dura mientras se produzca la sesión remota.
Es decir, se crea cuando un usuario se conecta al punto de conexión y se destruye cuando el usuario finaliza la sesión.
De forma predeterminada, la cuenta virtual es miembro del grupo de administradores local.
En un controlador de dominio, es miembro de los administradores de dominio.
Las cuentas virtuales son locales en la máquina en la que se crean y no tienen permisos fuera de esa máquina.
Esto significa que no están registradas en Active Directory (no se les ha asignado ningún RID).
Además, si un comando o script permitido intenta obtener acceso a recursos fuera de la máquina local, obtendrá acceso a esos recursos con la identidad de la máquina, no con la identidad de la cuenta virtual.

**Usuario conectado**: usuario sin privilegios de administrador que se conecta al punto de conexión de JEA y al que se asignan roles.
Los comandos que ejecute este usuario se ejecutan en el contexto del usuario de ejecución o de la cuenta virtual.


### Creación de un controlador de dominio

Este documento da por supuesto que la máquina está unida al dominio.
Si actualmente no tiene un dominio al que unirse, esta sección puede ayudarle a crear rápidamente un controlador de dominio con DSC.

#### Requisitos previos

1.  La máquina se encuentra en una red interna.
2.  La máquina no está unida a un dominio existente.
3.  La máquina ejecuta Windows Server 2016 o tiene instalado WMF 5.0.

#### Instalación de xActiveDirectory
Si la máquina tiene una conexión a Internet activa, ejecute el comando siguiente en una ventana de PowerShell con privilegios elevados:
```PowerShell
Install-Module xActiveDirectory -Force 
```
Si no tiene una conexión a Internet, instale xActiveDirectory en otra máquina y, después, copie la carpeta xActiveDirectory en la carpeta "C:\Archivos de programa\WindowsPowerShell\Modules" en su equipo.

Para confirmar que la instalación se ha realizado correctamente, ejecute el comando siguiente:
```PowerShell
Get-Module xActiveDirectory -ListAvailable
``` 

#### Configurar un dominio con DSC
Copie el script siguiente en PowerShell para hacer que la máquina sea el controlador de dominio de un dominio nuevo.
**NOTA DEL AUTOR: HAY UN PROBLEMA CONOCIDO CON LAS CREDENCIALES PROPORCIONADAS SI NO SE USAN.  PARA QUE SEAN SEGURAS, NO OLVIDE LA CONTRASEÑA DE ADMINISTRADOR LOCAL.**

```PowerShell
Set-Item WSMan:\localhost\Client\TrustedHosts -Value $env:COMPUTERNAME -Force 

# This "MetaConfiguration" sets the DSC Engine to automatically reboot if required
[DscLocalConfigurationManager()]
Configuration MetaConfiguration
{
    Node $env:Computername
    {
        Settings
        {
            RebootNodeIfNeeded = $true
        }
    }
    
}

MetaConfiguration
# Apply the MetaConfiguration
Set-DscLocalConfigurationManager .\MetaConfiguration

# Configure a domain controller of a new "Contoso" domain
configuration DomainController
{
    param
    (
        $node,
        $cred
    )
    Import-DscResource -ModuleName xActiveDirectory

    Node $node
    {
        WindowsFeature ADDS
        {
            Ensure = 'Present'
            Name = 'AD-Domain-Services'
        }

        xADDomain Contoso
        {
            DomainName = 'contoso.com'
            DomainAdministratorCredential = $cred
            SafemodeAdministratorPassword = $cred
            DependsOn = '[WindowsFeature]ADDS'
        }

        file temp
        {
            DestinationPath = 'C:\temp.txt'
            Contents = 'Domain has been created'
            DependsOn = '[xADDomain]Contoso'
        }
    }
}

$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = $env:Computername
            PSDscAllowPlainTextPassword = $true
        }
    )
}

# Enter your desired password for the domain administrator (note, this will be stored as plain text)
DomainController -cred (Get-Credential -Message "Enter desired credential for domain administrator") -node $env:Computername -configurationData $ConfigData

# Apply the configuration to create the domain controller
Start-DSCConfiguration -path .\DomainController -ComputerName $env:Computername -Wait -Force -Verbose
```
La máquina se reiniciará varias veces.
Sabrá que el proceso se ha completado cuando vea un archivo denominado "C:\temp.txt" con el texto "Domain has been created" (Se ha creado el dominio). 

#### Configurar usuarios y grupos
Los comandos siguientes configurarán un grupo de operadores y departamento de soporte técnico en su dominio y un usuario correspondiente sin privilegios de administrador que es miembro de ese grupo.
```PowerShell
# Make Groups
$NonAdminOperatorGroup = New-ADGroup -Name "JEA_NonAdmin_Operator" -GroupScope DomainLocal -PassThru
$NonAdminHelpDeskGroup = New-ADGroup -Name "JEA_NonAdmin_HelpDesk" -GroupScope DomainLocal -PassThru
$TestGroup = New-ADGroup -Name "Test_Group" -GroupScope DomainLocal -PassThru

# Make Users
$OperatorUser = New-ADUser -Name "OperatorUser" -AccountPassword (ConvertTo-SecureString "pa`$`$w0rd" -AsPlainText -Force) -PassThru
Enable-ADAccount -Identity $OperatorUser

$HelpDeskUser = New-ADUser -name "HelpDeskUser" -AccountPassword (ConvertTo-SecureString "pa`$`$w0rd" -AsPlainText -Force) -PassThru
Enable-ADAccount -Identity $HelpDeskUser

# Add Users to Groups
Add-ADGroupMember -Identity $NonAdminOperatorGroup -Members $OperatorUser
Add-ADGroupMember -Identity $NonAdminHelpDeskGroup -Members $HelpDeskUser
```

### Acerca de la lista negra
Ahora que se ha familiarizado con JEA, probablemente se pregunte si es posible incluir comandos en una lista negra.
Se trata de una solicitud comprensible, pero actualmente no está previsto para JEA por las razones siguientes:

1.  Hemos diseñado JEA para limitar a los operadores a las acciones que necesitan realizar.
Una lista negra es lo contrario a esto.

2.  Los autores de los comandos de PowerShell no diseñaron dichos comandos con JEA en mente.
En una instalación nueva de Windows Server 2016, existen aproximadamente 1520 comandos disponibles de inmediato.
Los modelos de riesgos para estos comandos no incluían la posibilidad de que un usuario pudiese ejecutar comandos como una cuenta con más privilegios.
Por ejemplo, determinados comandos permiten la inserción de código por diseño (por ejemplo, Invoke-Command y Add-Type en el módulo principal de PowerShell).
JEA puede avisarle cuando exponga los comandos específicos que conocemos, pero no hemos vuelto a evaluar todos los demás comandos de Windows en función del nuevo modelo de riesgos.
Debe comprender las funcionalidades de los comandos que exponga a través de JEA.  

3.  Además, incluso aunque JEA bloquease todos los comandos con vulnerabilidades de inserción de código, no hay ninguna garantía de que un usuario malintencionado no pueda llevar a cabo una acción incluida en la lista negra con otro comando relacionado.
A menos que comprenda todos los comandos que expone, es imposible garantizar que una acción determinada no es posible.
Usted tiene la responsabilidad de comprender qué comandos expone, tanto si se incluyen en una lista blanca como en una lista negra.
El número de comandos que expondría una lista negra sería imposible de administrar, por lo que JEA se implementa mediante listas blancas.

### Consideraciones al limitar comandos
En este paso, hay que hacer hincapié en un aspecto importante.
Es fundamental que todas las funcionalidades que se exponen a través de JEA se encuentren en áreas restringidas por el administrador.
Los usuarios sin privilegios de administrador no deben tener la capacidad de modificar los scripts que se usan a través de puntos de conexión de JEA.

Además, es fundamental que no ofrezca a los usuarios de JEA la posibilidad de sobrescribir configuraciones de JEA y scripts incluidos en una lista blanca dentro de sus sesiones de JEA.
Es muy importante que tenga mucho cuidado al exponer comandos como `Copy-Item`.

### Dificultades comunes de las funcionalidades de rol
Es posible que se encuentre con algunas dificultades comunes cuando lleve a cabo este proceso usted mismo.
A continuación se incluye una guía rápida en la que se explica cómo identificar y corregir estos problemas al modificar o crear un punto de conexión nuevo:

#### Funciones frente a Cmdlets
Los comandos de PowerShell escritos en PowerShell son funciones de PowerShell.
Los comandos de PowerShell escritos como clases .NET especializadas son cmdlets de PowerShell.
Para comprobar el tipo de comando, ejecute `Get-Command`.

#### VisibleProviders 
Deberá exponer los proveedores que necesiten sus comandos.
El más común es el proveedor FileSystem, pero puede que también necesite exponer otros, como el proveedor del Registro.
Para obtener una introducción a los proveedores, consulte esta [entrada del blog Hey, Scripting Guy](http://blogs.technet.com/b/heyscriptingguy/archive/2015/04/20/find-and-use-windows-powershell-providers.aspx).
Tenga cuidado al exponer los proveedores: a menudo es mejor definir una función propia que funcione con los proveedores subyacentes que exponer directamente el proveedor en una sesión de JEA.
De este modo, puede permitir que los usuarios sigan trabajando con archivos, claves del Registro, etc., pero mantiene el control sobre **qué* archivos y claves del Registro pueden usar mediante una lógica de validación personalizada.

<!--HONumber=Jun16_HO1-->


