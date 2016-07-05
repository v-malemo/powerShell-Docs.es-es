---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Active Directory de principio a fin
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 0a262e2c83174db7041d3cf35d97542b1cac4386

---

# De un extremo a otro: Active Directory
Imagine que el ámbito de su programa ha aumentado.
Ahora es responsable de agregar JEA a controladores de dominio para realizar acciones de Active Directory.
El personal del departamento de soporte técnico va a usar JEA para desbloquear cuentas, restablecer contraseñas y otras acciones similares.

Debe exponer un conjunto de comandos completamente nuevo para un grupo diferente de personas.
Además, tiene que exponer numerosos scripts existentes de Active Directory.
Esta sección le guiará por el proceso de creación de una configuración de sesión y funcionalidad de rol para esta tarea.

## Requisitos previos
Para seguir esta sección paso a paso, debe trabajar en un controlador de dominio.
Si no tiene acceso al controlador de dominio, no se preocupe.
Intente seguir los pasos trabajando en otro escenario o rol con el que esté familiarizado.
Si quiere configurar rápidamente un nuevo controlador de dominio, consulte la sección [Creación de un controlador de dominio](#creating-a-domain-controller) del apéndice.

## Pasos para crear una nueva funcionalidad de rol y configuración de sesión

La tarea de crear una nueva funcionalidad de rol puede parecer desalentadora al principio, pero puede dividirse en pasos bastante simples:

1.  Identificar las tareas que se deben habilitar
2.  Restringir las tareas según sea necesario
3.  Confirmar que funcionan con JEA
4.  Colocarlas en un archivo de funcionalidad de rol
5.  Registrar una configuración de sesión que exponga esa funcionalidad de rol

## Paso 1: identificar lo que se debe exponer
Antes de crear una nueva funcionalidad de rol o configuración de sesión, debe identificar todas las acciones que los usuarios tendrán que realizar mediante el punto de conexión de JEA, así como la manera en que se llevarán a cabo a través de PowerShell.
Esto implica llevar a cabo una extensa recopilación e investigación de requisitos.
La manera en que realice este proceso dependerá de su organización y de sus objetivos.
Es importante destacar que la recopilación y la investigación de los requisitos es una parte fundamental del proceso real.
Este podría ser el paso más difícil del proceso de adopción de JEA.

### Buscar recursos
A continuación se indica una serie de recursos en línea que podría haber encontrado en su investigación para crear un punto de conexión de administración de Active Directory:
-   [Introducción a Active Directory PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2009/03/05/active-directory-powershell-overview.aspx)
-   [Guía de CMD a PowerShell para Active Directory](http://blogs.technet.com/b/ashleymcglone/archive/2013/01/02/free-download-cmd-to-powershell-guide-for-ad.aspx)

### Hacer una lista
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

## Paso 2: restringir las tareas según sea necesario

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

### Inciso: Agregar una función al módulo
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

## Paso 4: editar el archivo de funcionalidad de rol
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

## Paso 5: registrar una nueva configuración de sesión
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
## Pruébela
Obtenga las credenciales de usuario sin privilegios de administrador:
```PowerShell
$HelpDeskCred = Get-Credential
```
Si ha seguido la sección [Set Up Users and Groups](creating-a-domain-controller.md#set-up-users-and-groups) (Configuración de usuarios y grupos), las credenciales serán:
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
## Conceptos clave
**Modo NoLanguage**: cuando PowerShell está en modo "NoLanguage", los usuarios solo pueden ejecutar comandos; no pueden usar elementos del lenguaje.
Para más información, vea `Get-Help about_Language_Modes`.

**Funciones de PowerShell**: las funciones de PowerShell son fragmentos de código de PowerShell que se puede llamar por su nombre.
Para más información, vea `Get-Help about_Functions`.

**ValidateSet/ValidatePattern**: al exponer un comando, puede restringir los argumentos válidos para parámetros específicos.
ValidateSet es una lista específica de comandos válidos.
ValidatePattern es una expresión regular con la que deben coincidir los argumentos de ese parámetro.




<!--HONumber=Jun16_HO4-->


