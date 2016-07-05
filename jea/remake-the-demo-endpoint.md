---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Recreación del punto de conexión de la demostración"
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: dabb5023012e90ace3fbc5f347c17821abd92595

---

# Recreación del punto de conexión de demostración
En esta sección obtendrá información sobre cómo generar una réplica exacta del punto de conexión de demostración usado en la sección anterior.
Aquí se introducirán conceptos básicos que son necesarios para entender JEA, incluidas las configuraciones de sesión de PowerShell y las funcionalidades de rol.

## Configuraciones de sesión de PowerShell
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
La mayoría viene con Windows, pero la configuración de sesión "JEA_Demo" se configuró en la sección [Requisitos previos](prerequisites.md).
Puede ver todas las configuraciones de sesión registradas ejecutando el comando siguiente en un símbolo del sistema de administrador de PowerShell:

```PowerShell
Get-PSSessionConfiguration
```

## Archivos de configuración de sesión de PowerShell
Puede establecer configuraciones de sesión nuevas registrando *archivos nuevos de configuración de sesión de PowerShell*.
Los archivos de configuración de sesión tienen la extensión de archivo ".pssc".
Puede generar archivos de configuración de sesión con el cmdlet New-PSSessionConfigurationFile.

Después, creará y registrará una nueva configuración de sesión para JEA.

## Generar y modificar la configuración de sesión de PowerShell
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
*RestrictedRemoteServer* define la configuración mínima necesaria para la administración remota.
De forma predeterminada, un punto de conexión *RestrictedRemoteServer* expone Get-Command, Get-FormatData, Select-Object, Get-Help, Measure-Object, Exit-PSSession, Clear-Host y Out-Default.
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

## Aplicar la configuración de sesión de PowerShell

Para crear un punto de conexión a partir de un archivo de configuración de sesión, debe registrar el archivo.
Para ello, se necesita cierta información:

1. La ruta de acceso al archivo de configuración de sesión.
2. El nombre de la configuración de sesión registrada. Este es el mismo nombre que los usuarios proporcionan al parámetro "ConfigurationName" cuando se conectan al punto de conexión con `Enter-PSSession` o `New-PSSession`.

Para registrar la configuración de sesión en la máquina local, ejecute el comando siguiente:

```PowerShell
Register-PSSessionConfiguration -Name 'JEADemo2' -Path "$env:ProgramData\JEAConfiguration\JEADemo2.pssc"
```

Enhorabuena, ha configurado el punto de conexión de JEA.

## Probar el punto de conexión
Vuelva a ejecutar los pasos enumerados en la sección [Uso de JEA](using-jea.md) con el nuevo punto de conexión para confirmar que funciona según lo previsto.
Asegúrese de usar el nombre nuevo del punto de conexión (JEADemo2) al proporcionar el nombre de la configuración a Enter-PSSession.

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEADemo2 -Credential $NonAdminCred
```

## Conceptos clave
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




<!--HONumber=Jun16_HO4-->


