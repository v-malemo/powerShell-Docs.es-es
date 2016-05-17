---
title:   Recurso nxService de DSC para Linux
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Recurso nxService de DSC para Linux

El recurso **nxService** de la configuración de estado deseado (DSC) de PowerShell ofrece un mecanismo para administrar servicios en un nodo de Linux.

## Sintaxis

```
nxService <string> #ResourceName
{
    Name = <string>
    [ Controller = <string> { init | upstart | system }  ]
    [ Enabled = <bool> ]
    [ State = <string> { Running | Stopped } ]
    [ DependsOn = <string[]> ]

}
```

## Propiedades
|  Propiedad |  Descripción | 
|---|---|
| Nombre| El nombre del servicio o el demonio que se configurará.| 
| Controlador| El tipo de controlador de servicio que se debe usar al configurar el servicio.| 
| Habilitada| Indica si el servicio se inicia en el arranque.| 
| Estado| Indica si el servicio se está ejecutando. Establezca esta propiedad en "Stopped" para garantizar que el servicio no se esté ejecutando. Establézcala en "Running" para garantizar que el servicio no se esté ejecutando.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento **ID** del bloque del script de configuración del recurso que quiere ejecutar primero es **ResourceName** y su tipo es **ResourceType**, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 


## Información adicional

El recurso **nxService** no creará una definición de servicio ni un script para el servicio si no existe. Puede usar el recurso **nxFile** de la configuración de estado deseado de PowerShell para administrar la existencia o el contenido del script o el archivo de definición del servicio.

## Ejemplo

En el ejemplo siguiente se muestra la configuración del servicio "httpd" (para el servidor HTTP Apache), registrado con el controlador de servicio **SystemD**.

```
Import-DSCResource -Module nx 

Node $node {
#Apache Service
nxService ApacheService 
{
Name = "httpd"
State = "running"
Enabled = $true
Controller = "systemd"
}
}
```



<!--HONumber=May16_HO3-->


