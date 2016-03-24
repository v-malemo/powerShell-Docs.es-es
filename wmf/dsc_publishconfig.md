# Entregar un documento de configuración sin aplicar

El cmdlet **Publish-DscConfiguration** copia un archivo MOF de configuración en un nodo de destino, pero no aplica la configuración. Esta configuración se aplica durante el siguiente paso de coherencia o cuando se ejecuta el cmdlet `Update-DscConfiguration`.

```powershell
Publish-DscConfiguration [-Path] <string> [[-ComputerName] <string[]>] [-Force] [-Credential <pscredential>] [-ThrottleLimit <int>] [-WhatIf] [-Confirm] [<CommonParameters>]

Publish-DscConfiguration [-Path] <string> -CimSession <CimSession[]> [-Force] [-ThrottleLimit <int>] [-WhatIf] [-Confirm] [<CommonParameters>]
```
<!--HONumber=Mar16_HO2-->
