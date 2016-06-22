---
title:   Especificación de dependencias entre nodos
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Especificación de dependencias entre nodos

> Se aplica a: Windows PowerShell 5.0

DSC proporciona recursos especiales, **WaitForAll**, **WaitForAny** y **WaitForSome**, que se pueden usar en configuraciones para especificar las dependencias en configuraciones de otros nodos. El comportamiento de estos recursos es el siguiente:

* **WaitForAll**: se ejecuta si el recurso especificado está en el estado deseado en todos los nodos de destino definidos en la propiedad **NodeName**.
* **WaitForAny**: se ejecuta si el recurso especificado está en el estado deseado en al menos uno de los nodos de destino definidos en la propiedad **NodeName**.
* **WaitForSome**: especifica una propiedad **NodeCount** además de **NodeName**. El recurso se ejecuta si está en el estado deseado en un número mínimo de nodos (especificado por **NodeCount**) definido por la propiedad **NodeName**. 

## Uso de recursos WaitForXXXX

Para usar los recursos **WaitForXXXX**, cree un bloque de recursos de ese tipo de recurso que especifique los nodos y el recurso de DSC que se esperará. Después, use la propiedad **DependsOn** en otros bloques de recursos de la configuración para esperar que las condiciones especificadas del nodo **WaitForXXXX** sean correctas.

Por ejemplo, en la configuración siguiente, el nodo de destino está esperando que el recurso **xADDomain** finalice en el nodo **MyDC** con un número máximo de 30 reintentos, a intervalos de 15 segundos, antes de que el nodo de destino pueda unirse al dominio.

```PowerShell
Configuration JoinDomain

{
    Import-DscResource -Module xComputerManagement

    Node myDomainJoinedServer
    {

        WaitForAll DC
        {
            ResourceName      = '[xADDomain]NewDomain'
            NodeName          = 'MyDC'
            RetryIntervalSec  = 15
            RetryCount        = 30
        }

        xComputer JoinDomain
        {
            Name             = 'MyPC'
            DomainName       = 'Contoso.com'
            Credential       = (Get-Credential)
            DependsOn        ='[WaitForAll]DC'
        }
    }
}
```

>**Nota:** De manera predeterminada, los recursos de WaitForXXX lo intentan una vez y después se produce un error. Aunque no es necesario, normalmente la intención será especificar un recuento y un intervalo de reintentos.

## Consulte también
* [Configuraciones DSC](configurations.md)
* [Recursos de DSC](resources.md)
* [Configuración del administrador de configuración local](metaConfig.md)



<!--HONumber=Jun16_HO3-->


