# Especificar dependencias entre nodos

Con los recursos integrados WaitFor\* (WaitForAll, WaitForAny y WaitForSome), ahora puede especificar las dependencias entre equipos durante las ejecuciones de configuración, sin orquestación externa. El comportamiento o cada recurso WaitFor\* es:

* **WaitForAll** Espera a que el recurso especificado esté en el estado deseado en *todos* los nodos de destino definidos en la propiedad NodeName.
* **WaitForAny** Espera a que el recurso especificado esté en el estado deseado en *cualquier* nodo de destino definido en la propiedad NodeName.
* **WaitForSome** Espera a que el recurso especificado esté en el estado deseado en el número específico, definido por la propiedad NodeCount, de nodos de destino definidos en la propiedad NodeName.

Estos recursos ofrecen sincronización de nodo a nodo a través de conexiones CIM en el protocolo WS-Man. Con estos recursos, una configuración puede esperar que el estado de recurso específico de otro equipo cambie antes de continuar con su propia configuración. 

Por ejemplo, en la configuración siguiente, el nodo de destino está esperando que el recurso **xADDomain** finalice en el nodo **MyDC** con unos cuantos reintentos, antes de que el nodo de destino pueda unirse al dominio.

```PowerShell
Configuration JoinDomain

{
    Import-DscResource -Module xComputerManagement

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
        Credential       = (get-credential)
        DependsOn        ='[WaitForAll]DC'
    }
}
```
**Sugerencia:** De forma predeterminada, los recursos WaitFor\* realizan un intento y luego generan un error. Por tanto, aunque no sea necesario, normalmente deseará especificar un intervalo y un número de reintentos.
<!--HONumber=Mar16_HO2-->
