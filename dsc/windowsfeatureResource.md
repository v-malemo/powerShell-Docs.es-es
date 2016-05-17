---
title:   Recurso de DSC WindowsFeature
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Recurso de DSC WindowsFeature

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso **WindowsFeature** de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para asegurarse de que los roles y las características se agreguen o quiten en un nodo de destino.

## Sintaxis

```
WindowsFeature [string] #ResourceName
{
    Name = [string]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ IncludeAllSubFeature = [bool] ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ Source = [string] ]
}
```

## Propiedades

|  Propiedad  |  Descripción   | 
|---|---| 
| Nombre| Indica el nombre del rol o la característica que quiere garantizar que se agregue o se quite. Esto es el mismo que la propiedad __Name__ del cmdlet [Get-WindowsFeature](https://technet.microsoft.com/en-us/library/jj205469.aspx) y no el nombre para mostrar del rol o la característica.| 
| Credential| Indica las credenciales que se usarán para agregar o quitar el rol o la característica.| 
| Ensure| Indica si se agrega el rol o la característica. Para asegurarse de que el rol o la característica se agregue, establezca esta propiedad en "Present"; para asegurarse de que se quite el rol o característica, establezca la propiedad a "Absent".| 
| IncludeAllSubFeature| Establezca esta propiedad en __$true__ para garantizar el estado de todas las subcaracterísticas requeridas con el estado de la característica que se especifique con la propiedad __Name__.| 
| LogPath| Indica la ruta de acceso a un archivo de registro donde quiera que el proveedor de recursos registre la operación.| 
| DependsOn| Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 
| Origen| Indica la ubicación del archivo de origen que se utilizará para la instalación, si es necesario.| 

## Ejemplo
```powershell
WindowsFeature RoleExample
{
    Ensure = "Present" 
    # Alternatively, to ensure the role is uninstalled, set Ensure to "Absent"
    Name = "Web-Server" # Use the Name property from Get-WindowsFeature  
}
```



<!--HONumber=May16_HO3-->


