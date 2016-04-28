# Especificar dependencias entre nodos

Con los recursos integrados WaitFor\* (WaitForAll, WaitForAny y WaitForSome), ahora puede especificar las dependencias entre equipos durante las ejecuciones de configuración, sin orquestación externa. El comportamiento o cada recurso WaitFor\* es:

* <ctype="x-NOTFOUND" mdpre="**" mdpost="**">WaitForAll</ctype="x-NOTFOUND"> Espera a que el recurso especificado esté en el estado deseado en <ctype="x-NOTFOUND" mdpre="*" mdpost="*">todos</ctype="x-NOTFOUND"> los nodos de destino definidos en la propiedad NodeName.
* <ctype="x-NOTFOUND" mdpre="**" mdpost="**">WaitForAny</ctype="x-NOTFOUND"> Espera a que el recurso especificado esté en el estado deseado en <ctype="x-NOTFOUND" mdpre="*" mdpost="*">cualquier</ctype="x-NOTFOUND"> nodo de destino definido en la propiedad NodeName.
* <ctype="x-NOTFOUND" mdpre="**" mdpost="**">WaitForSome</ctype="x-NOTFOUND"> Espera a que el recurso especificado esté en el estado deseado en el número específico, definido por la propiedad NodeCount, de nodos de destino definidos en la propiedad NodeName.

Estos recursos ofrecen sincronización de nodo a nodo a través de conexiones CIM en el protocolo WS-Man. Con estos recursos, una configuración puede esperar que el estado de recurso específico de otro equipo cambie antes de continuar con su propia configuración. 

Por ejemplo, en la configuración siguiente, el nodo de destino está esperando que el recurso <ctype="x-NOTFOUND" mdpre="**" mdpost="**">xADDomain</ctype="x-NOTFOUND"> finalice en el nodo <ctype="x-NOTFOUND" mdpre="**" mdpost="**">MyDC</ctype="x-NOTFOUND"> con unos cuantos reintentos, antes de que el nodo de destino pueda unirse al dominio.

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
<ctype="x-NOTFOUND" mdpre="**" mdpost="**">Sugerencia:</ctype="x-NOTFOUND"> De manera predeterminada, los recursos WaitFor\* realizan un intento y luego generan un error. Por tanto, aunque no sea necesario, normalmente querrá especificar un intervalo y un número de reintentos.


<!--HONumber=Mar16_HO3-->


