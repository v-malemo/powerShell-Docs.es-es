---
title: Recurso de DSC Service
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 6477ae8575c83fc24150f9502515ff5b82bc8198
ms.openlocfilehash: ce8d6c1c9e8005c5c4792ae0fa4c5030bb9a92ed

---

# Recurso de DSC Service

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0


El recurso **Service** de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para administrar servicios en el nodo de destino.

## Sintaxis

```
Service [string] #ResourceName
{
    Name = [string]
    [ BuiltInAccount = [string] { LocalService | LocalSystem | NetworkService }  ]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
    [ StartupType = [string] { Automatic | Disabled | Manual }  ]
    [ State = [string] { Running | Stopped }  ]
}
```

## Propiedades

|  Propiedad  |  Descripción   | 
|---|---| 
| Nombre| Indica el nombre del servicio. Tenga en cuenta que a veces es distinto del nombre para mostrar. Puede obtener una lista de los servicios y sus estados actuales con el cmdlet Get-Service.| 
| BuiltInAccount| Indica la cuenta de inicio de sesión que se utilizará para el servicio. Los valores permitidos para esta propiedad son: **LocalService**, **LocalSystem** y **NetworkService**.| 
| Credential| Indica las credenciales de la cuenta en la que se ejecutará el servicio. Esta propiedad no se puede utilizar junto con la propiedad __BuiltinAccount__.| 
| DependsOn| Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 
| StartupType| Indica el tipo de inicio del servicio. Los valores permitidos para esta propiedad son: **Automatic**, **Disabled** y **Manual**.| 
| Estado| Indica el estado que quiere garantizar para el servicio.| 

## Ejemplo

```powershell
Service ServiceExample
{
    Name = "TermService"
    StartupType = "Manual"
    State = "Running"
} 
```




<!--HONumber=Jun16_HO4-->


