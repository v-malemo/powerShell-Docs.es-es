---
title:   Recursos de DSC Archive
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Recursos de DSC Archive

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso Archive de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para desempaquetar archivos de almacenamiento (.zip) en una ruta de acceso específica.

## Sintaxis 
```MOF
Archive [string] #ResourceName
{
    Destination = [string]
    Path = [string]
    [ Checksum = [string] { CreatedDate | ModifiedDate | SHA-1 | SHA-256 | SHA-512 } ]
    [ DependsOn = [string[]] ]
    [ Ensure = [string] { Absent | Present } ]
    [ Force = [bool] ]
    [ Validate = [bool] ]
}
```

## Propiedades

|  Propiedad  |  Descripción   | 
|---|---| 
| Destination| Especifica la ubicación donde quiere garantizar que se extraiga el contenido del archivo.| 
| Ruta| Especifica la ruta de acceso de origen del archivo.| 
| __Checksum__| Define el tipo que se usará cuando se determine si dos archivos son iguales. Si no se especifica __Checksum__, solo se usa el nombre del archivo o directorio para la comparación. Los valores válidos son: SHA-1, SHA-256, SHA-512, createdDate, modifiedDate, none (valor predeterminado). Si especifica __Checksum__ sin __Validate__, se producirá un error de configuración.| 
| Ensure| Determina si se debe comprobar si existe el contenido del archivo en __Destination__. Establezca esta propiedad en __Present__ para asegurarse de que el contenido exista. Establézcala en __Absent__ para asegurarse de que no exista. El valor predeterminado es __Present__.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es ResourceName y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 
| Validar| Utiliza la propiedad Checksum para determinar si el archivo coincide con la firma. Si especifica Checksum sin Validate, se producirá un error de configuración. Si especifica Validate sin Checksum, se usará una suma de comprobación SHA-256 de forma predeterminada.| 
| Force| Determinadas operaciones de archivos (por ejemplo, sobrescribir un archivo o eliminar un directorio que no está vacío) provocarán un error. Si se usa la propiedad Force, se invalidan estos errores. El valor predeterminado es False.| 

## Ejemplo

En el ejemplo siguiente se muestra cómo usar el recurso Archive para asegurarse de que el contenido de un archivo llamado Test.zip exista y se extraiga en un destino determinado.

```
Archive ArchiveExample {
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Path = "C:\Users\Public\Documents\Test.zip"
    Destination = "C:\Users\Public\Documents\ExtractionPath"
} 
```



<!--HONumber=May16_HO3-->


