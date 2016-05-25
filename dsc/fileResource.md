---
title:   Recurso de DSC File
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Recurso de DSC File

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso File de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para administrar archivos y carpetas en el nodo de destino.

## Sintaxis
```
File [string] #ResourceName
{
    DestinationPath = [string]
    [ Attributes = [string[]] { Archive | Hidden | ReadOnly | System }]
    [ Checksum = [string] { CreatedDate | ModifiedDate | SHA-1 | SHA-256 | SHA-512 } ]
    [ Contents = [string] ]
    [ Credential = [PSCredential] ]
    [ Ensure = [string] { Absent | Present } ] 
    [ Force = [bool] ]
    [ Recurse = [bool] ]
    [ DependsOn = [string[]] ]
    [ SourcePath = [string] ]
    [ Type = [string] { Directory | File } ] 
    [ MatchSource = [bool] ]
}
```

## Propiedades

|  Propiedad  |  Descripción   | 
|---|---| 
| DestinationPath| Indica la ubicación donde desea garantizar el estado de un archivo o directorio.| 
| Atributos| Especifica el estado deseado de los atributos del archivo o directorio de destino.| 
| Checksum| Indica el tipo de suma de comprobación para determinar si dos archivos son iguales. Si no se especifica __Checksum__, solo se usa el nombre del archivo o directorio para la comparación. Los valores válidos son: SHA-1, SHA-256, SHA-512, createdDate, modifiedDate.| 
| Contenido| Especifica el contenido de un archivo, como una cadena determinada.| 
| Credential| Indica las credenciales necesarias para acceder a recursos, como archivos de origen, si es necesario dicho acceso.| 
| Ensure| Indica si el archivo o directorio existe. Establezca esta propiedad en "Absent" para asegurarse de que el archivo o directorio no exista. Establézcala en "Present" para asegurarse de que el archivo o directorio exista. El valor predeterminado es "Present".| 
| Force| Determinadas operaciones de archivos (por ejemplo, sobrescribir un archivo o eliminar un directorio que no está vacío) provocarán un error. Si se usa la propiedad Force, se invalidan estos errores. El valor predeterminado es __$false__.| 
| Recurse| Indica si se incluyen los subdirectorios. Establezca esta propiedad en __$true__ para indicar que quiere que los subdirectorios se incluyan. El valor predeterminado es __$false__. **Nota**: Esta propiedad solo es válida cuando la propiedad Type está establecida en Directory.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 
| SourcePath| Indica la ruta de acceso de la que se copiará el recurso de archivo o carpeta.| 
| Tipo| Indica si el recurso que se está configurando es un directorio o un archivo. Establezca esta propiedad en "Directory" para indicar que el recurso es un directorio. Establézcala en "File" para indicar que el recurso es un archivo. El valor predeterminado es "File".| 
| MatchSource| Si se establece en el valor predeterminado de __$false__, los archivos en el origen (por ejemplo, los archivos A, B y C) se agregarán al destino la primera vez que se aplique la configuración. Si se agrega un nuevo archivo (D) al origen, no se agregará al destino, incluso aunque la configuración se vuelva a aplicar más adelante. Si el valor es __$true__, cada vez que se aplique la configuración, se agregan los nuevos archivos encontrados posteriormente en el origen (como el archivo D de este ejemplo) al destino.| 

## Ejemplo

En el ejemplo siguiente se muestra cómo usar el recurso File para asegurarse de que un directorio con la ruta de acceso `C:\Users\Public\Documents\DSCDemo\DemoSource` en un equipo de origen (por ejemplo, el servidor "pull") también está presente (junto con todos los subdirectorios) en el nodo de destino. También escribe un mensaje de confirmación en el registro cuando se complete e incluye una instrucción para asegurarse de que se ejecuta la operación de comprobación de archivos antes de la operación de registro.

```powershell
Configuration FileResourceDemo
{
    Node "localhost"
    {
        File DirectoryCopy
        {
            Ensure = "Present"  # You can also set Ensure to "Absent"
            Type = "Directory" # Default is "File".
            Recurse = $true # Ensure presence of subdirectories, too
            SourcePath = "C:\Users\Public\Documents\DSCDemo\DemoSource"
            DestinationPath = "C:\Users\Public\Documents\DSCDemo\DemoDestination"    
        }

        Log AfterDirectoryCopy
        {
            # The message below gets written to the Microsoft-Windows-Desired State Configuration/Analytic log
            Message = "Finished running the file resource with ID DirectoryCopy"
            DependsOn = "[File]DirectoryCopy" # This means run "DirectoryCopy" first.
        }
    }
}
```



<!--HONumber=May16_HO3-->


