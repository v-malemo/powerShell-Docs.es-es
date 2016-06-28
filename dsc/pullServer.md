---
title: "Configuración de un servidor de extracción web de DSC"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: a5f3025ff222d4a27c0da074df9e84d82c51a46f
ms.openlocfilehash: 7bbfc31fdebdde83ac1784373b51af40b1dc9492

---

# Configuración de un servidor de extracción web de DSC

> Se aplica a: Windows PowerShell 5.0

Un servidor de extracción web de DSC es un servicio web en IIS que usa una interfaz de OData para poner a disposición de los nodos de destino los archivos de configuración DSC cuando esos nodos los soliciten.

Requisitos para usar un servidor de extracción:

* Un servidor que ejecute:
  - WMF/PowerShell 5.0 o una versión posterior
  - Rol del servidor IIS
  - Servicio DSC
* Idealmente, algún medio para generar un certificado, para proteger las credenciales que se pasan al administrador de configuración local (LCM) en los nodos de destino

Puede agregar el rol del servidor IIS y el servicio DSC con el asistente para agregar roles y características en el administrador del servidor o mediante el uso de PowerShell. Los scripts de ejemplo incluidos en este tema también controlarán estos dos pasos por usted.

## Uso del recurso xWebService
La manera más fácil de configurar un servidor de extracción web es usar el recurso xWebService, incluido en el módulo xPSDesiredStateConfiguration. En los siguientes pasos se explica cómo usar el recurso en una configuración que configure el servicio web.

1. Llame al cmdlet [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) para instalar el módulo **xPSDesiredStateConfiguration**. **Nota**: **Install-Module** está incluido en el módulo **PowerShellGet**, que se incluye en PowerShell 5.0. Puede descargar el módulo **PowerShellGet** para PowerShell 3.0 y 4.0 en [PackageManagement PowerShell Modules Preview](https://www.microsoft.com/en-us/download/details.aspx?id=49186) (Vista previa de los módulos de PowerShell PackageManagement). 
1. Obtenga un certificado SSL para el servidor de incorporación de cambios de DSC de una entidad de certificación de confianza, ya sea dentro de su organización o de una entidad pública. El certificado recibido de la entidad suele tener el formato PFX. Instale el certificado en el nodo que se convertirá en el servidor de incorporación de cambios de DSC en la ubicación predeterminada que debe ser CERT: \LocalMachine\My. Anote la huella digital del certificado.
1. Seleccione un GUID para usarlo como clave de registro. Para generar uno con PowerShell, escriba lo siguiente en el símbolo de PS y presione ENTRAR: '``` [guid]::newGuid()```'. Esta clave la utilizarán los nodos de cliente como una clave compartida para la autenticación durante el registro. Para obtener más información, consulte la sección [Clave de registro](#RegKey) más adelante.
1. En PowerShell ISE, inicie (F5) el siguiente script de configuración (incluido en la carpeta Example del módulo **xPSDesiredStateConfiguration** como Sample_xDscWebService.ps1). Este script configura el servidor de incorporación de cambios.
  
```powershell
configuration Sample_xDscPullServer
{ 
    param  
    ( 
            [string[]]$NodeName = 'localhost', 
            
            [ValidateNotNullOrEmpty()] 
            [string] $certificateThumbPrint,
            
            [Parameter(Mandatory)]
            [ValidateNotNullOrEmpty()]
            [string] $RegistrationKey 
     ) 
 
 
     Import-DSCResource -ModuleName xPSDesiredStateConfiguration 

     Node $NodeName 
     { 
         WindowsFeature DSCServiceFeature 
         { 
             Ensure = 'Present'
             Name   = 'DSC-Service'             
         } 
 
         xDscWebService PSDSCPullServer 
         { 
             Ensure                  = 'Present' 
             EndpointName            = 'PSDSCPullServer' 
             Port                    = 8080 
             PhysicalPath            = "$env:SystemDrive\inetpub\PSDSCPullServer" 
             CertificateThumbPrint   = $certificateThumbPrint          
             ModulePath              = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules" 
             ConfigurationPath       = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration" 
             State                   = 'Started'
             DependsOn               = '[WindowsFeature]DSCServiceFeature'                         
         } 

        File RegistrationKeyFile
        {
            Ensure          = 'Present'
            Type            = 'File'
            DestinationPath = "$env:ProgramFiles\WindowsPowerShell\DscService\RegistrationKeys.txt"
            Contents        = $RegistrationKey
        }
    }
}

```

1. Ejecute la configuración pasando la huella digital del certificado SSL como el parámetro **certificateThumbPrint** y una clave de registro GUID como el parámetro **RegistrationKey**:

```powershell
# To find the Thumbprint for an installed SSL certificate for use with the pull server list all certifcates in your local store 
# and then copy the thumbprint for the appropriate certificate by reviewing the certificate subjects
dir Cert:\LocalMachine\my

# Then include this thumbprint when running the configuration
Sample_xDSCPullServer -certificateThumbprint 'A7000024B753FA6FFF88E966FD6E19301FAE9CCC' -RegistrationKey '140a952b-b9d6-406b-b416-e0f759c9c0e4' -OutputPath c:\Configs\PullServer

# Run the compiled configuration to make the target node a DSC Pull Server
Start-DscConfiguration -Path c:\Configs\PullServer -Wait -Verbose
```

## Clave de registro
Para permitir que los nodos de cliente se registren con el servidor de forma que puedan usar nombres de configuración en lugar de un identificador de configuración, se guarda una clave de registro creada mediante la configuración anterior en un archivo denominado `RegistrationKeys.txt` en `C:\Program Files\WindowsPowerShell\DscService`. La clave de registro funciona como un secreto compartido que se usa durante el registro inicial del cliente con el servidor de incorporación de cambios. El cliente generará un certificado autofirmado que se utiliza para autenticar el servidor de incorporación de cambios inequívocamente una vez que el registro se complete correctamente. La huella digital del certificado se almacena localmente y se asocia con la dirección URL del servidor de incorporación de cambios.
> **Nota**: No se admiten claves del registro en PowerShell 4.0. 

Para configurar un nodo para que se autentique con el servidor de incorporación de cambios, la clave de registro debe estar en la metaconfiguration de cualquier nodo de destino que se registre con este servidor de incorporación de cambios. Tenga en cuenta que el elemento **RegistrationKey** de la siguiente metaconfiguration se quita una vez que el equipo de destino se ha registrado correctamente, y que el valor '140a952b-b9d6-406b-b416-e0f759c9c0e4' debe coincidir con el valor almacenado en el archivo RegistrationKeys.txt del servidor de incorporación de cambios. Trate siempre el valor de clave de registro de forma segura, porque conocer esta clave conlleva que cualquier máquina de destino se registre al servidor de incorporación de cambios.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode          = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded   = $true
        }
        
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL          = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey    = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
            ConfigurationNames = @('ClientConfig')
        }   
        
        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL       = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
        }
    }
}

PullClientConfigID -OutputPath c:\Configs\TargetNodes
```
> **Nota**: La sección **ReportServerWeb** permite informar de datos que deben enviarse al servidor de incorporación de cambios. 

Si falta la propiedad **ConfigurationID** en el archivo de metaconfiguration, significa implícitamente que el servidor de incorporación de cambios es compatible con la versión V2 del protocolo del servidor de incorporación de cambios, lo que requiere un registro inicial. Por el contrario, si hay una propiedad **ConfigurationID**, significa que se usa la versión V1 del protocolo del servidor de incorporación de cambios y que no hay ningún procesamiento de registro.

>**Nota**: En un escenario de inserción, hay un error en la versión actual que hace que sea necesario definir una propiedad ConfigurationID en el archivo de metaconfiguration para los nodos que nunca se han registrado en un servidor de incorporación de cambios. De esta manera, se forzará el protocolo del servidor de incorporación de cambios V1 y se evitarán los mensajes de error de registro.

## Colocación de configuraciones y recursos
Una vez completada la configuración del servidor de incorporación de cambios, los módulos y las configuraciones que estarán disponibles para la incorporación de cambios de los nodos de destino se colocarán en las carpetas que definan las propiedades **ConfigurationPath** y **ModulePath** de la configuración del servidor de incorporación de cambios. Estos archivos deben estar en un formato concreto para que el servidor de incorporación de cambios los procese correctamente. 

### Formato del paquete de módulo de recursos de DSC
Cada módulo de recursos se debe comprimir y se le debe asignar un nombre de acuerdo con el siguiente patrón **{Module Name}_{Module Version}.zip**. Por ejemplo, un módulo denominado xWebAdminstration con una versión de módulo de 3.1.2.0 se denominaría 'xWebAdministration_3.2.1.0.zip'. Cada versión de un módulo debe incluirse en un solo archivo ZIP. Dado que solo hay una versión de un recurso en cada archivo ZIP, no se admite el formato de módulo que se agrega en WMF 5.0 con compatibilidad con varias versiones de módulo en un único directorio. Esto significa que antes de empaquetar los módulos de recursos de DSC para su uso con el servidor de incorporación de cambios, deberá realizar un pequeño cambio en la estructura de directorios. El formato predeterminado de los módulos que contienen recursos de DSC en WMF 5.0 es '{Module Folder}\{Module Version}\DscResources\{DSC Resource Folder}\'. Antes de empaquetar el servidor de extracción, quite la carpeta **{Module version}** para que la ruta de acceso se convierta en '{Module Folder}\DscResources\{DSC Resource Folder}\'. Con este cambio, comprima la carpeta según lo descrito anteriormente y coloque estos archivos ZIP en la carpeta **ModulePath**.

### Formato de archivo MOF de configuración 
Un archivo de configuración MOF debe emparejarse con un archivo de suma de comprobación para que un LCM de un nodo de destino pueda validar la configuración. Para crear una suma de comprobación, llame al cmdlet [New-DSCCheckSum](https://technet.microsoft.com/en-us/library/dn521622.aspx). El cmdlet toma un parámetro **Path** que especifica la carpeta donde se encuentra el MOF de configuración. El cmdlet crea un archivo de suma de comprobación denominado `ConfigurationMOFName.mof.checksum`, donde `ConfigurationMOFName` es el nombre del archivo mof de configuración. Si hay más de un archivo MOF de configuración en la carpeta especificada, se crea una suma de comprobación para cada una de las configuraciones de la carpeta. Coloque los archivos MOF y sus archivos asociados de suma de comprobación en la carpeta **ConfigurationPath**.

>**Nota**: Si cambia el archivo MOF de configuración de cualquier manera, también deberá volver a crear el archivo de suma de comprobación.

## Herramientas
Con el fin de facilitar la configuración, la validación y la administración del servidor de incorporación de cambios, se incluyen las siguientes herramientas como ejemplos en la versión más reciente del módulo xPSDesiredStateConfiguration:
1. Un módulo que le ayudará a empaquetar los archivos de configuración y los módulos de recursos de DSC para su uso en el servidor de incorporación de cambios. [PublishModulesAndMofsToPullServer.psm1](https://github.com/PowerShell/xPSDesiredStateConfiguration/blob/dev/DSCPullServerSetup/PublishModulesAndMofsToPullServer.psm1). Vea los ejemplos siguientes:

```powershell
    # Example 1 - Package all versions of given modules installed locally and MOF files are in c:\LocalDepot
     $moduleList = @("xWebAdministration", "xPhp") 
     Publish-DSCModuleAndMof -Source C:\LocalDepot -ModuleNameList $moduleList 
     
     # Example 2 - Package modules and mof documents from c:\LocalDepot
     Publish-DSCModuleAndMof -Source C:\LocalDepot -Force
```

1. Un script que valida que el servidor de incorporación de cambios se configura correctamente. [PullServerSetupTests.ps1](https://github.com/PowerShell/xPSDesiredStateConfiguration/blob/dev/Examples/PullServerDeploymentVerificationTest/PullServerSetupTests.ps1).


## Configuración del cliente de incorporación de cambios 
En los temas siguientes se describe en detalle cómo configurar los clientes de extracción:

* [Configuración de un cliente de incorporación de cambios de DSC mediante un id. de configuración](pullClientConfigID.md)
* [Configuración de un cliente de incorporación de cambios de DSC mediante nombres de configuración](pullClientConfigNames.md)
* [Configuraciones parciales](partialConfigs.md)


## Vea también
* [Información general sobre la configuración de estado deseado de Windows PowerShell](overview.md)
* [Establecer configuraciones](enactingConfigurations.md)
* [Uso de un servidor de informes de DSC](reportServer.md)




<!--HONumber=Jun16_HO4-->


