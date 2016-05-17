---
title:   Separación de los datos de entorno y configuración
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Separación de los datos de entorno y configuración

>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

En la configuración de estado deseado (DSC) de Windows PowerShell, es posible separar los datos de configuración de la lógica de la configuración. Otra manera de enfocar todo esto es considerar que la configuración estructural (por ejemplo, una configuración que podría requerir que IIS esté instalado) es independiente de la configuración del entorno (es decir, si la situación es un entorno de pruebas frente a uno de producción, los nombres de los nodos serían distintos, pero la configuración que se les aplicaría sería la misma). Esto tiene las ventajas siguientes:

* Permite reutilizar los datos de configuración para los diferentes recursos, nodos y configuraciones.
* La lógica de configuración es más reutilizable si no contiene datos codificados de forma rígida. Esto es similar a unas buenas directrices de creación de scripts para funciones.
* Si algunos de los datos deben cambiar, puede realizar los cambios en una ubicación, con lo que se ahorraría tiempo y se reducirían los errores.

## Conceptos básicos y ejemplos

Para especificar la parte del entorno de la configuración, DSC utiliza el parámetro **ConfigurationData**, que es una tabla hash (o puede tomar un archivo .psd1 que contiene la tabla hash). Esta tabla hash debe tener al menos una clave **AllNodes**, que tiene un valor estructurado. Por ejemplo:

```powershell
$MyData = 
@{
    AllNodes = @();
    NonNodeData = ""   
}
```

Observe la clave AllNodes, cuyo valor es una matriz. Cada elemento de esta matriz también es una tabla hash, que requiere NodeName como una clave:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName = "VM-1"
        },

 
        @{
            NodeName = "VM-2"
        },

 
        @{
            NodeName = "VM-3"
        }
    );

    NonNodeData = ""   
}
```

Cada entrada de la tabla hash de AllNodes corresponde a los datos de configuración de un nodo de la configuración. Además de la clave NodeName necesaria, puede agregar otras claves a la tabla hash; por ejemplo:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName = "VM-1";
            Role     = "WebServer"
        },

 
        @{
            NodeName = "VM-2";
            Role     = "SQLServer"
        },

 
        @{
            NodeName = "VM-3";
            Role     = "WebServer"
        }
    );

    NonNodeData = ""   
}
```

DSC ofrece tres variables especiales para usarlas en el script de configuración:

* **$AllNodes**: hace referencia a todos los nodos. Puede usar el filtrado **.Where()** y **.ForEach()**, de forma que en lugar de codificar de forma rígida los nombres de nodos para seleccionar nodos concretos para la acción, podría escribir algo parecido a esto para seleccionar VM-1 y VM-3 en el ejemplo anterior:

```powershell
configuration MyConfiguration
{
    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
    }
}
```

* **$Node**: cuando se filtra el conjunto de nodos, puede usar $Node para hacer referencia a la entrada determinada. Por ejemplo:

```powershell
configuration MyConfiguration
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite
    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = "Present"
        }
    }
}
```

Para aplicar una propiedad a todos los nodos, puede establecer NodeName = "*". Por ejemplo, para aplicar a todos los nodos la propiedad LogPath, se podría hacer lo siguiente:

```
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName           = "*"
            LogPath            = "C:\Logs"
        },

 
        @{
            NodeName = "VM-1";
            Role     = "WebServer"
            SiteContents = "C:\Site1"
            SiteName = "Website1"
        },

 
        @{
            NodeName = "VM-2";
            Role     = "SQLServer"
        },

 
        @{
            NodeName = "VM-3";
            Role     = "WebServer";
            SiteContents = "C:\Site2"
            SiteName = "Website3"
        }
    );
}
```

* **$ConfigurationData**: puede usar esta variable dentro de una configuración para acceder a la tabla hash de los datos de configuración pasada como un parámetro. Por ejemplo:

```powershell
$MyData = 
@{
    AllNodes = 
    @(
        @{
            NodeName           = "*"
            LogPath            = "C:\Logs"
        },

 
        @{
            NodeName = "VM-1";
            Role     = "WebServer"
            SiteContents = "C:\Site1"
            SiteName = "Website1"
        },

 
        @{
            NodeName = "VM-2";
            Role     = "SQLServer"
        },
 

        @{
            NodeName = "VM-3";
            Role     = "WebServer";
            SiteContents = "C:\Site2"
            SiteName = "Website3"
        }
    );

    NonNodeData = 
    @{
        ConfigFileContents = (Get-Content C:\Template\Config.xml)
     }   
} 

configuration MyConfiguration
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = "Present"
        }

        File ConfigFile
        {
            DestinationPath = $Node.SiteContents + "\\config.xml"
            Contents = $ConfigurationData.NonNodeData.ConfigFileContents
        }
    }
}
```

Puede encontrar un ejemplo completo incluido en el [módulo xWebAdministration](https://powershellgallery.com/packages/xWebAdministration).



<!--HONumber=May16_HO3-->


