---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: requisitos previos
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: ac9231a475ba84e9051bbd06a65f3f20c9e49846

---

# Requisitos previos

## Estado inicial
Antes de comenzar esta sección, compruebe lo siguiente:

1. JEA está disponible en el sistema. Consulte el archivo [LÉAME](./README.md) de los sistemas operativos admitidos actualmente y de las descargas necesarias.
2. Tiene derechos administrativos en el equipo en el que va a probar JEA.
3. El equipo está unido al dominio.
Consulte la sección [Creación de un controlador de dominio](#creating-a-domain-controller) para configurar rápidamente un nuevo dominio en un servidor si aún no tiene uno.

## Habilitar Comunicación remota con PowerShell
La administración con JEA se produce a través de Comunicación remota de PowerShell.
Ejecute lo siguiente en una ventana de administrador de PowerShell para asegurarse de que esté habilitado y configurado correctamente:

```PowerShell
Enable-PSRemoting
```

Si no está familiarizado con Comunicación remota de PowerShell, sería conveniente ejecutar `Get-Help about_Remote` para obtener información sobre este concepto básico.

## Identificar los usuarios o grupos
Para mostrar JEA en acción, debe identificar los usuarios y grupos no administradores que va a usar a lo largo de esta guía.

Si usa un dominio existente, identifique o cree algunos usuarios y grupos sin privilegios.
Les dará acceso a JEA a estos usuarios no administradores.
Cuando vea la variable `$NonAdministrator` en la parte superior de un script, asígnelo a los usuarios o grupos no administradores seleccionados.

Si crea un dominio desde cero, es mucho más fácil.
Use la sección [Set Up Users and Groups](creating-a-domain-controller.md#set-up-users-and-groups) (Configuración de usuarios y grupos) del apéndice para crear grupos y usuarios no administradores.
Los valores predeterminados de `$NonAdministrator` serán los grupos creados en esa sección.

## Configurar el archivo de funcionalidad de rol de mantenimiento
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

## Crear y registrar el archivo de configuración de sesión de demostración
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

## Habilitar el registro de módulos de PowerShell (opcional)
En los siguientes pasos se habilita el registro para todas las acciones de PowerShell del sistema.
No es necesario habilitarlo para que JEA funcione, pero será útil en la sección [Generación de informes en JEA](reporting-on-jea.md).

1. Abra el Editor de directivas de grupo Local
2. Vaya a "Configuración del equipo\Plantillas administrativas\Componentes de Windows\Windows PowerShell".
3. Haga doble clic en "Activar registro de módulos".
4. Haga clic en "Habilitado".
5. En la sección Opciones, haga clic en "Mostrar" junto a Nombres de módulos.
6. Escriba "\*" en la ventana emergente. Esto significa que PowerShell registrará los comandos de todos los módulos.
7. Haga clic en Aceptar y aplique la directiva.

Nota: También puede habilitar la transcripción de PowerShell de todo el sistema a través de la directiva de grupo.

**Enhorabuena, ha configurado el equipo con el punto de conexión de demostración y está listo para empezar a trabajar con JEA.**




<!--HONumber=Jun16_HO4-->


