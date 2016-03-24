# Estado coherente unificado y representación de estado

En este versión se incluye una serie de mejoras para el estado de LCM y el estado de DSC creados por las automatizaciones. Incluyen representaciones de estado y de estado unificado coherente, la propiedad datatime administrable de los objetos de estado devueltos por el cmdlet Get-DscConfigurationStatus y la propiedad state details de LCM mejorada devuelta por el cmdlet Get-DscLocalConfigurationManager.

La representación del estado de las operaciones de DSC y LCM se revisa y unifica de acuerdo con las reglas siguientes:
1.  El recurso Notprocessed no afecta al estado de DSC y LCM.
2.  LCM deja de procesar más recursos cuando encuentra un recurso que solicita un reinicio.
3.  Un recurso que solicita un reinicio no está en el estado deseado hasta el reinicio se produce realmente.
4.  Después de encontrar un recurso que genera un error, LCM sigue procesando más recursos, siempre que estos no dependan del que produjo el error.
5.  El estado general devuelto por el cmdlet Get-DscConfigurationStatus es el superconjunto de estado de todos los recursos.
6.  El estado PendingReboot es un superconjunto del estado PendingConfiguration.

En la siguiente tabla se muestran el estado resultante y las propiedades relacionadas con el estado en algunos escenarios típicos.

| **Escenario**                    | **LCMState\***       | **Estado** | **Reinicio solicitado**  | **ResourcesInDesiredState**  | **ResourcesNotInDesiredState** |
|---------------------------------|----------------------|------------|---------------|------------------------------|--------------------------------|
| S**^**                          | Inactivo                 | Correcto    | $false        | S                            | $null                          |
| F**^**                          | PendingConfiguration | Error    | $false        | $null                        | F                              |
| S, F                             | PendingConfiguration | Error    | $false        | S                            | F                              |
| F, S                             | PendingConfiguration | Error    | $false        | S                            | F                              |
| S<sub>1</sub>, F, S<sub>2</sub> | PendingConfiguration | Error    | $false        | S<sub>1</sub>, S<sub>2</sub> | F                              |
| F<sub>1</sub>, S, F<sub>2</sub> | PendingConfiguration | Error    | $false        | S                            | F<sub>1</sub>, F<sub>2</sub>   |
| S, r                            | PendingReboot        | Correcto    | $true         | S                            | r                              |
| F, r                            | PendingReboot        | Error    | $true         | $null                        | F, r                           |
| r, S                            | PendingReboot        | Correcto    | $true         | $null                        | r                              |
| r, F                            | PendingReboot        | Correcto    | $true         | $null                        | r                              |

^
S<sub>i</sub>: serie de recursos que se aplicaron correctamente
S<sub>i</sub>: serie de recursos que no se aplicaron correctamente
r: recurso que requiere un reinicio
\*

```powershell
$LCMState = (Get-DscLocalConfigurationManager).LCMState
$Status = (Get-DscConfigurationStatus).Status

$RebootRequested = (Get-DscConfigurationStatus).RebootRequested

$ResourcesInDesiredState = (Get-DscConfigurationStatus).ResourcesInDesiredState

$ResourcesNotInDesiredState = (Get-DscConfigurationStatus).ResourcesNotInDesiredState
```
## Mejora en el cmdlet Get-DscConfigurationStatus

En esta versión, se incluyen algunas mejoras en el cmdlet Get-DscConfigurationStatus. Anteriormente, la propiedad StartDate de los objetos devueltos por el cmdlet era de tipo String. Ahora, es de tipo Datetime, que facilita la selección y el filtrado complejos según las propiedades intrínsecas de un objeto Datetime.
```powershell
(Get-DscConfigurationStatus).StartDate | fl \*
DateTime : Friday, November 13, 2015 1:39:44 PM
Date : 11/13/2015 12:00:00 AM
Day : 13
DayOfWeek : Friday
DayOfYear : 317
Hour : 13
Kind : Local
Millisecond : 886
Minute : 39
Month : 11
Second : 44
Ticks : 635830187848860000
TimeOfDay : 13:39:44.8860000
Year : 2015
```

A continuación se ofrece un ejemplo que devuelve todos los registros de operaciones de DSC que se produjeron el mismo día de la semana que hoy.
```powershell
(Get-DscConfigurationStatus –All) | where { $\_.startdate.dayofweek -eq (Get-Date).DayOfWeek }
```

Se eliminaron los registros de las operaciones que no realizan cambios en la configuración del nodo (es decir, las operaciones de solo lectura). Por lo tanto, las operaciones Test-DscConfiguration, Get-DscConfiguration ya no están adulteradas en los objetos devueltos del cmdlet Get-DscConfigurationStatus.
Los registros de la operación de metaconfiguración se agregaron a la devolución del cmdlet Get-DscConfigurationStatus.

A continuación se ofrece un ejemplo de resultado devuelto del cmdlet Get-DscConfigurationStatus –All.
```powershell
All configuration operations:

Status StartDate Type RebootRequested
------ --------- ---- ---------------
Success 11/13/2015 11:38:16 AM Consistency False
Success 11/13/2015 11:23:16 AM Reboot False
Success 11/13/2015 11:21:43 AM Reboot True
Success 11/13/2015 11:20:44 AM Initial True
Success 11/13/2015 11:20:44 AM LocalConfigurationManager False
```

## Mejora en el cmdlet Get-DSCLocalConfigurationManager
Se agregó un nuevo campo de LCMStateDetail al objeto devuelto del cmdlet Get-DscLocalConfigurationManager. Este campo se rellena cuando el valor de LCMState es "Ocupado". Se puede recuperar mediante el siguiente cmdlet:
```powershell
(Get-DscLocalConfigurationManager).LCMStateDetail
```

A continuación se ofrece un ejemplo de salidas de una supervisión continua en una configuración que requiere dos reinicios en un nodo remoto.
```powershell
Start a configuration that requires two reboots

Monitor LCM State:
LCM State: Busy, LCM is applying a new configuration.
LCM State: PendingReboot,
Machine is rebooting...
LCM State: Busy, LCM is continuing applying configuration after last reboot.
LCM State: PendingReboot,
Machine is rebooting...
LCM State: Busy, LCM is continuing applying configuration after last reboot.
LCM State: Idle,
LCM State: Busy, LCM is performing a consistency check.
LCM State: Idle,
```
<!--HONumber=Mar16_HO2-->
