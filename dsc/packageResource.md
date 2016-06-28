---
title: Recursos de DSC Package
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 6477ae8575c83fc24150f9502515ff5b82bc8198
ms.openlocfilehash: bcaf82cbafe67cc309765e16b3c9cd6eff0a982a

---

# Recursos de DSC Package

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso **Package** de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para instalar o desinstalar paquetes, como los paquetes de Windows Installer y setup.exe, en un nodo de destino.

## Sintaxis

```
Package [string] #ResourceName
{
    Name = [string]
    Path = [string]
    ProductId = [string]
    [ Arguments = [string] ]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ LogPath = [string] ]
    [ DependsOn = [string[]] ]
    [ ReturnCode = [UInt32[]] ]
}
```

## Propiedades
|  Propiedad  |  Descripción   | 
|---|---| 
| Nombre| Indica el nombre del paquete para el que quiere garantizar un estado específico.| 
| Ruta| Indica la ruta de acceso donde reside el paquete.| 
| ProductId| Indica el id. de producto que identifica el paquete.| 
| Argumentos| Muestra una cadena de argumentos que se pasarán al paquete tal y como se faciliten.| 
| Credential| Proporciona acceso al paquete en un origen remoto. Esta propiedad no se utiliza para instalar el paquete. El paquete siempre se instala en el sistema local.| 
| Ensure| Indica si el paquete está instalado. Establezca esta propiedad en "Absent" para asegurarse de que el paquete no esté instalado (o se desinstale el paquete si está instalado). Establézcala en "Present" (el valor predeterminado) para asegurarse de que el paquete esté instalado.| 
| LogPath| Indica la ruta de acceso completa donde quiere que el proveedor guarde un archivo de registro para instalar o desinstalar el paquete.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es **ResourceName** y su tipo es **ResourceType**, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"``.| 
| ReturnCode| Indica el código de retorno esperado. Si el código de retorno real no a coincide con el valor esperado facilitado aquí, la configuración devolverá un error.| 

## Ejemplo

En este ejemplo se ejecuta al instalador .msi que se encuentra en la ruta de acceso especificada y tiene el identificador de producto especificado.

```powershell
Package PackageExample
{
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Path  = "$Env:SystemDrive\TestFolder\TestProject.msi"
    Name = "TestPackage"
    ProductId = "ACDDCDAF-80C6-41E6-A1B9-8ABD8A05027E"
} 
```




<!--HONumber=Jun16_HO4-->


