---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Implementación y mantenimiento de varias máquinas"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 784806197a64eb30af1ecea4af55575434ce7b87

---

# Implementación y mantenimiento de varias máquinas
En este momento, ha implementado JEA en sistemas locales varias veces.
Dado que su entorno de producción probablemente esté formado por más de una máquina, es importante que siga los pasos fundamentales del proceso de implementación que deben repetirse en cada máquina.

## Pasos de alto nivel:
1.  Copie los módulos (con funcionalidades de rol) en cada nodo.
2.  Copie los archivos de configuración de sesión en cada nodo.
3.  Ejecute `Register-PSSessionConfiguration` con la configuración de sesión.
4.  Conserve una copia de la configuración de sesión y de los kits de herramientas en una ubicación segura.
Como realizará modificaciones, es conveniente tener un "origen único de verdad."

## Script de ejemplo
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
## Modificar funcionalidades
Cuando se trabaja con varias máquinas, es importante que las modificaciones se implementen de forma coherente.
Una vez que JEA tenga un recurso de DSC, podrá asegurarse más fácilmente de que el entorno esté sincronizado.
Hasta ese momento, se recomienda que conserve una copia maestra de las configuraciones de sesión y que vuelva a implementarla cada vez que realice una modificación.

## Quitar funcionalidades
Para quitar la configuración de JEA de sus sistemas, use el comando siguiente en cada máquina:
```PowerShell
Unregister-PSSessionConfiguration -Name JEADemo
```




<!--HONumber=Jul16_HO1-->


