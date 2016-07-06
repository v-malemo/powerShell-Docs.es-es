# Extracción a petición de configuraciones de DSC

El nuevo cmdlet Update-DscConfiguration desencadena una extracción de los servidores de extracción definidos en la metaconfiguración. El comportamiento suele denominarse "extraer ahora". 


Una vez desencadenada, la extracción se comporta exactamente igual que si se desencadenara durante la frecuencia regular:

1. La suma de comprobación de la configuración actual se compara con la suma de comprobación de la configuración en el servidor de extracción. 
2. Si son iguales, se completa correctamente sin aplicar la configuración. 
3. Si son diferentes, la configuración se extrae del servidor de extracción y se aplica.

**Nota:** Si RefreshMode = 'Push' en la metaconfiguración, este cmdlet devuelve un error. Por consiguiente, este cmdlet nunca hará nada cuando un nodo de destino esté en modo de "inserción".

```PowerShell
Update-DscConfiguration     [[-ComputerName] <string[]>] 
                            [-Wait]
                            [-Force] 
                            [-JobName <string>] 
                            [-Credential<pscredential>] 
                            [-ThrottleLimit <int>] 
                            [-WhatIf] 
                            [-Confirm] 
                            [<CommonParameters>]

Update-DscConfiguration     -CimSession <CimSession[]> 
                            [-Wait] 
                            [-Force] 
                            [-JobName <string>] 
                            [-ThrottleLimit <int>]
                            [-WhatIf] 
                            [-Confirm] 
                            [<CommonParameters>]
```

<!--HONumber=Jun16_HO4-->


