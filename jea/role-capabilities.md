---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Funcionalidades de rol
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 5b6dcb205d2c3cbb1a98c6465cb1002b9ed61459

---

# Funcionalidades de rol

## Introducción
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

## Creación de funcionalidades de rol
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

## Conceptos clave
**Funcionalidad de rol (.psrc)**: archivo que define qué puede hacer un usuario en un punto de conexión de JEA.
Detalla una lista blanca de elementos como comandos visibles, aplicaciones de consola visibles, etc.
Para que PowerShell detecte las funcionalidades de rol, debe colocarlas en una carpeta "RoleCapabilities" en un módulo de PowerShell válido.

**Módulo de PowerShell**: paquete de funcionalidad de PowerShell.
Puede contener funciones de PowerShell, cmdlets, recursos de DSC, funcionalidades de rol y mucho más.
Para que se carguen automáticamente, los módulos de PowerShell deben estar ubicados en una ruta de acceso en `$env:PSModulePath`.




<!--HONumber=Jun16_HO4-->


