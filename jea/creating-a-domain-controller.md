---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "creación de un controlador de dominio"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: d4a72a7c5883b1d3ba8de3dbc9cfe016a6fb3498
ms.openlocfilehash: 8473eb668e4da5bab01c2f2b7647cbced413bd22

---

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
$OperatorUser = New-ADUser -Name "OperatorUser" -AccountPassword (ConvertTo-SecureString 'pa$$w0rd' -AsPlainText -Force) -PassThru
Enable-ADAccount -Identity $OperatorUser

$HelpDeskUser = New-ADUser -name "HelpDeskUser" -AccountPassword (ConvertTo-SecureString 'pa$$w0rd' -AsPlainText -Force) -PassThru
Enable-ADAccount -Identity $HelpDeskUser

# Add Users to Groups
Add-ADGroupMember -Identity $NonAdminOperatorGroup -Members $OperatorUser
Add-ADGroupMember -Identity $NonAdminHelpDeskGroup -Members $HelpDeskUser
```




<!--HONumber=Jul16_HO1-->


