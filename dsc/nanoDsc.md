---
title:   Uso de DSC on Nano Server
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Uso de DSC on Nano Server

> Se aplica a: Windows PowerShell 5.0

**DSC on Nano Server** es un paquete opcional de la carpeta `NanoServer\Packages` de los medios de Windows Server 2016. Se puede instalar el paquete cuando se crea un VHD para un Nano Server mediante la especificación de **Microsoft-NanoServer-DSC-Package** como valor del parámetro **Packages** de la función **New-NanoServerImage**. Por ejemplo, si está creando un VHD para una máquina virtual, el comando sería similar al siguiente:

```powershell
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1 -Packages Microsoft-NanoServer-DSC-Package
```

Para obtener más información sobre cómo instalar y usar Nano Server, y también cómo administrar Nano Server con comunicación remota de PowerShell, vea [Getting Started with Nano Server](https://technet.microsoft.com/en-us/library/mt126167.aspx) (Introducción a Nano Server).


## Características de DSC disponibles en Nano Server

 Dado que Nano Server solo admite un conjunto limitado de API en comparación con una versión completa de Windows Server, DSC on Nano Server no tiene paridad completa funcional con DSC en ejecución en SKU completas por el momento. DSC on Nano Server está en desarrollo activo y todavía no es una característica completa.
 
 Las siguientes características de DSC están disponibles actualmente en Nano Server: 


* Modos de inserción y extracción

* Todos los cmdlets de DSC que existen en una versión completa de Windows Server, incluidos los siguientes: 
  * [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx)
  * [Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx)   
  * [Enable-DscDebug](https://technet.microsoft.com/en-us/library/mt517870.aspx)
  * [Disable-DscDebug](https://technet.microsoft.com/en-us/library/mt517872.aspx)       
  * [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx)
  * [Stop-DscConfiguration](https://technet.microsoft.com/en-us/library/mt143542.aspx)
  * [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407379.aspx)
  * [Test-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx)      
  * [Publish-DscConfiguraiton](https://technet.microsoft.com/en-us/library/mt517875.aspx) 
  * [Update-DscConfiguration](https://technet.microsoft.com/en-us/library/mt143541.aspx)
  * [Restore-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407383.aspx)
  * [Remove-DscConfigurationDocument](https://technet.microsoft.com/en-us/library/mt143544.aspx)
  * [Get-DscConfigurationStatus](https://technet.microsoft.com/en-us/library/mt517868.aspx)
  * [Invoke-DscResource](https://technet.microsoft.com/en-us/library/mt517869.aspx)
  * [Find-DscResource](https://technet.microsoft.com/en-us/library/mt517874.aspx)
  * [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx)
  * [New-DscChecksum](https://technet.microsoft.com/en-us/library/dn521622.aspx)    

* Compilación de configuraciones (vea [Configuraciones DSC](configurations.md))

  **Problema:** no funciona el cifrado de contraseña (vea [Proteger el archivo MOF](securemof.md)) durante la compilación de la configuración.

* Compilación de metaconfiguraciones (vea [Configuración del administrador de configuración local](metaConfig.md))

* Ejecutar un recurso en el contexto de usuario (vea [DSC de ejecución con las credenciales de usuario (RunAs)](runAsUser.md))

* Recursos basados en clases (vea [Escribir un recurso de DSC personalizado con clases de PowerShell](authoringResourceClass.md))

* Depuración de recursos de DSC (vea [Depuración de recursos de DSC](debugresource.md))
  
  **Problema:** no funciona si un recurso usa PsDscRunAsCredential (vea [DSC de ejecución con las credenciales de usuario](runAsUser.md))

* [Especificación de dependencias entre nodos](crossNodeDependencies.md) 

* [Control de versiones de recursos](sxsResource.md)

* Cliente de extracción (configuraciones y recursos) (vea [Configuración de un cliente de extracción mediante nombres de configuración](pullClientConfigNames.md))

* [Configuraciones parciales (extracción e inserción)](partialConfigs.md)

* [Generación de informes en el servidor de extracción](reportServer.md) 

* Cifrado de MOF

* Registro de eventos

* Informes de DSC de automatización de Azure

* Recursos que son totalmente funcionales
  * [Archive](archiveResource.md)
  * [Entorno](environmentResource.md)
  * [Archivo](fileResource.md)
  * [Registro](logResource.md)
  * ProcessSet
  * [Registro](registryResource.md)
  * [Script](scriptResource.md)
  * WindowsPackageCab
  * [WindowsProcess](windowsProcessResource.md)
  * WaitForAll (vea [Especificación de dependencias entre nodos](crossNodeDependencies.md))
  * WaitForAny (vea [Especificación de dependencias entre nodos](crossNodeDependencies.md))
  * WaitForSome (vea [Especificación de dependencias entre nodos](crossNodeDependencies.md))

* Recursos que son parcialmente funcionales
  * [Grupo](groupResource.md)
  * GroupSet
  
  **Problema:** los recursos anteriores producirán un error si se llama dos veces a una instancia específica (ejecutando dos veces la misma configuración)
  
  * [Service](serviceResource.md)
  * ServiceSet
  
  **Problema:** solo funciona para iniciar y detener el servicio (estado). Produce un error si intenta cambiar otros atributos del servicio, como startuptype, credenciales, descripción, etc. El error que se produce es similar a:
  
  *No se puede encontrar el tipo [management.managementobject]. Compruebe que está cargado el ensamblado que lo contiene.*
  
* Recursos que no son funcionales
  * [Usuario](userResource.md)
  

## Características de DSC no disponibles en Nano Server

Las siguientes características de DSC no están disponibles actualmente en Nano Server:

* Descifrar el documento MOF con contraseñas cifradas 
* Servidor de extracción: actualmente no se puede establecer un servidor de extracción en Nano Server
* Todo lo que no está en la lista de trabajos funciona

## Uso de recursos personalizados de DSC en Nano Server
 
Debido a un conjunto limitado de bibliotecas CLR y API de Windows disponibles en Nano Server, los recursos de DSC que funcionan en la versión CLR completa de Windows no funcionan necesariamente en Nano Server. 
Pruebas de un extremo a otro antes de implementar los recursos personalizados de DSC en un entorno de producción.

## Consulte también
- [Getting Started with Nano Server (Introducción a Nano Server)](https://technet.microsoft.com/en-us/library/mt126167.aspx)



<!--HONumber=May16_HO3-->


