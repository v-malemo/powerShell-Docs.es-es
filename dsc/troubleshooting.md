---
title: "Solución de problemas de DSC"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: d367048eab0ba3fd67baed2ee27332ce0827d5ac
ms.openlocfilehash: a09f228cf232ff9d7cf2ba20c73808fd92c9d560

---

# Solución de problemas de DSC

>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

Este tema describen las distintas formas de solucionar problemas de DSC cuando surgen.

## Uso de Get-DscConfigurationStatus

El cmdlet [Get-DscConfigurationStatus](https://technet.microsoft.com/en-us/library/mt517868.aspx) obtiene información acerca del estado de la configuración de un nodo de destino. Se devuelve un objeto enriquecido que incluye información detallada sobre si la configuración de ejecución era correcta o no. Ya puede adentrarse en el objeto para descubrir los detalles sobre la configuración de ejecución como:

* Todos los recursos con errores.
* Cualquier recurso que solicitó un reinicio.
* Las opciones de metaconfiguración en tiempo de ejecución de la configuración.
* Etc.

El siguiente conjunto de parámetros devuelve la información de estado de la última ejecución de la configuración:

```powershell
Get-DscConfigurationStatus  [-CimSession <CimSession[]>] 
                            [-ThrottleLimit <int>] 
                            [-AsJob] 
                            [<CommonParameters>]
```
El siguiente conjunto de parámetros devuelve la información de estado de todas las ejecuciones anteriores de la configuración:

```powershell
Get-DscConfigurationStatus  -All 
                            [-CimSession <CimSession[]>] 
                            [-ThrottleLimit <int>] 
                            [-AsJob] 
                            [<CommonParameters>]
```

## Ejemplo

```powershell
PS C:\> $Status = Get-DscConfigurationStatus 

PS C:\> $Status

Status      StartDate               Type            Mode    RebootRequested     NumberOfResources
------      ---------               ----            ----    ---------------     -----------------
Failure     11/24/2015  3:44:56     Consistency     Push    True                36

PS C:\> $Status.ResourcesNotInDesiredState

ConfigurationName       :   MyService
DependsOn               :   
ModuleName              :   PSDesiredStateConfiguration
ModuleVersion           :   1.1
PsDscRunAsCredential    :   
ResourceID              :   [File]ServiceDll
SourceInfo              :   c:\git\CustomerService\Configs\MyCustomService.ps1::5::34::File
DurationInSeconds       :   0.19
Error                   :   SourcePath must be accessible for current configuration. The related file/directory is:
                            \\Server93\Shared\contosoApp.dll. The related ResourceID is [File]ServiceDll
FinalState              :   
InDesiredState          :   False
InitialState            :   
InstanceName            :   ServiceDll
RebootRequested         :   False
ReosurceName            :   File
StartDate               :   11/24/2015  3:44:56
PSComputerName          :
```

## El script no se ejecuta: uso de registros de DSC para diagnosticar errores de scripts

Como sucede con todo el software de Windows, DSC registra los errores y eventos en [registros](https://msdn.microsoft.com/library/windows/desktop/aa363632.aspx) que se pueden ver en el [Visor de eventos](http://windows.microsoft.com/windows/what-information-event-logs-event-viewer). Examinar estos registros puede ayudarle a entender el motivo del error de una operación determinada y cómo evitar los errores en el futuro. La escritura de scripts de configuración puede ser complicada, por lo que para que sea más fácil realizar un seguimiento de los errores a medida que se crea, utilice el recurso de registro de DSC para hacer un seguimiento del progreso de la configuración en el registro de eventos DSC Analytic.

## ¿Dónde se encuentran los registros de eventos de DSC?

En el Visor de eventos, los eventos de DSC se encuentran en: **Registros de aplicaciones y servicios/Microsoft/Windows/Configuración de estado deseado**.

El cmdlet de PowerShell correspondiente, [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx), también se puede ejecutar para ver los registros de eventos:

```
PS C:\> Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                                                                                  
-----------                     -- ---------------- -------                                                                                                  
11/17/2014 10:27:23 PM        4102 Information      Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
```

Como se muestra arriba, el nombre del registro primario de DSC es **Microsoft->Windows->DSC** (no se muestran otros nombres de registros de Windows por razones de brevedad). El nombre principal se anexa al nombre del canal para crear el nombre del registro completo. El motor de DSC escribe principalmente en tres tipos de registros: [registros operativos, analíticos y de depuración](https://technet.microsoft.com/library/cc722404.aspx). Como los registros analíticos y de depuración están desactivados de forma predeterminada, debe habilitarlos en el Visor de eventos. Para ello, abra el Visor de eventos escribiendo Show-EventLog en Windows PowerShell, o bien, haga clic en el botón **Inicio**, **Panel de Control**, **Herramientas administrativas** y luego haga clic en **Visor de eventos**. En el menú **Vista** del Visor de eventos, haga clic en **Mostrar registros analíticos y de depuración**. El nombre del registro del canal analítico **Microsoft-Windows-Dsc/Analytic** y el del canal de depuración **Microsoft-Windows-Dsc/Debug**. También puede usar la utilidad [wevtutil](https://technet.microsoft.com/library/cc732848.aspx) para habilitar los registros, como se muestra en el ejemplo siguiente.

```powershell
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
```

## ¿Qué contienen los registros de DSC?

Los registros de DSC se dividen en los tres canales de registro según la importancia del mensaje. El registro operativo de DSC contiene todos los mensajes de error y puede utilizarse para identificar un problema. El registro analítico tiene un mayor volumen de eventos y puede identificar dónde se han producido errores. Este canal también contiene los mensajes detallados (si existen). El registro de depuración contiene registros que pueden ayudarle a entender cómo se produjeron los errores. Los mensajes de los eventos de DSC se estructuran de forma que cada mensaje de evento comienza por un identificador de trabajo que representa de forma única una operación de DSC. En el ejemplo siguiente intenta obtener el mensaje desde el primer evento registrado en el registro operativo de DSC.

```powershell
PS C:\> $AllDscOpEvents = Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
PS C:\> $FirstOperationalEvent = $AllDscOpEvents[0]
PS C:\> $FirstOperationalEvent.Message
Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
Consistency engine was run successfully. 
```

Los eventos de DSC se registran en una estructura determinada que permite al usuario agrupar los eventos de un trabajo de DSC. La estructura es la siguiente:

**Id. de trabajo: <Guid>**
**<Event Message>**

## Recopilación de eventos de una operación de DSC única

Los registros de eventos de DSC contienen los eventos que han generado diferentes operaciones de DSC. Sin embargo, normalmente solo le interesarán los detalles sobre una operación determinada. Todos los registros de DSC se pueden agrupar según la propiedad de identificador de trabajo, que es única para cada operación de DSC. El identificador de trabajo se muestra como el primer valor de propiedad de todos los eventos de DSC. En los siguientes pasos se explica cómo acumular todos los eventos en una estructura de matriz agrupada.

```powershell
<##########################################################################
 Step 1 : Enable analytic and debug DSC channels (Operational channel is enabled by default)
###########################################################################>
 
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
wevtutil.exe set-log “Microsoft-Windows-Dsc/Debug” /q:True /e:true
 
<##########################################################################
 Step 2 : Perform the required DSC operation (Below is an example, you could run any DSC operation instead)
###########################################################################>
 
Get-DscLocalConfigurationManager
 
<##########################################################################
Step 3 : Collect all DSC Logs, from the Analytic, Debug and Operational channels
###########################################################################>
 
$DscEvents=[System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Operational") `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Analytic" -Oldest) `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Debug" -Oldest)
 
 
<##########################################################################
 Step 4 : Group all logs based on the job ID
###########################################################################>
$SeparateDscOperations = $DscEvents | Group {$_.Properties[0].value}  
```

Aquí, la variable `$SeparateDscOperations` contiene registros agrupados por los identificadores de trabajo. Cada elemento de la matriz de esta variable representa un grupo de eventos que ha registrado otra operación de DSC, lo que permite el acceso a más información sobre los registros.

```
PS C:\> $SeparateDscOperations
 
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   48 {1A776B6A-5BAC-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
   40 {E557E999-5BA8-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
PS C:\> $SeparateDscOperations[0].Group
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                               
-----------                     -- ---------------- -------                                               
12/2/2013 3:47:29 PM          4115 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4198 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4114 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4102 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4098 Warning          Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4098 Warning          Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4176 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...      
12/2/2013 3:47:29 PM          4182 Information      Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : ...       
```

Puede extraer los datos en la variable `$SeparateDscOperations` con [Where-Object](https://technet.microsoft.com/library/ee177028.aspx). A continuación se muestran cinco escenarios en los que podría querer extraer los datos para la solucionar problemas de DSC:

### 1: Errores de operaciones

Todos los eventos tienen [niveles de gravedad](https://msdn.microsoft.com/library/dd996917(v=vs.85)). Esta información puede usarse para identificar los eventos de error:

```
PS C:\> $SeparateDscOperations | Where-Object {$_.Group.LevelDisplayName -contains "Error"}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   38 {5BCA8BE7-5BB6-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
```

### 2: Detalles de las operaciones que se ejecutaron en última media hora

`TimeCreated`, una propiedad de todos los eventos de Windows, indica la hora en que se ha creado el evento. Se puede hacer una comparación de esta propiedad con un objeto de fecha y hora determinado para filtrar todos los eventos:

```powershell
PS C:\> $DateLatest = (Get-Date).AddMinutes(-30)
PS C:\> $SeparateDscOperations | Where-Object {$_.Group.TimeCreated -gt $DateLatest}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
    1 {6CEC5B09-5BB0-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord}   
```

### 3: Mensajes de la operación más reciente

La operación más reciente se almacena en el primer índice del grupo de matrices `$SeparateDscOperations`. Si se consultan los mensajes del grupo del índice 0, se devuelven todos los mensajes para la operación más reciente:

```powershelll
PS C:\> $SeparateDscOperations[0].Group.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
Running consistency engine.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Configuration is sent from computer NULL by user sid S-1-5-18.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Displaying messages from built-in DSC resources:
 WMI channel 1 
 ResourceID:  
 Message : [INCH-VM]:                            [] Starting consistency engine.
Job {1A776B6A-5BAC-11E3-BF41-00155D553612} : 
Displaying messages from built-in DSC resources:
 WMI channel 1 
 ResourceID:  
 Message : [INCH-VM]:                            [] Consistency check completed. 
```

### 4: Mensajes de error registrados para operaciones con errores recientes

`$SeparateDscOperations[0].Group` contiene un conjunto de eventos para la operación más reciente. Ejecute el cmdlet `Where-Object` para filtrar los eventos según su nombre para mostrar del nivel. Los resultados se almacenan en la variable `$myFailedEvent`, que se puede analizar en más profundidad para obtener el mensaje de evento:

```powershell
PS C:\> $myFailedEvent = ($SeparateDscOperations[0].Group | Where-Object {$_.LevelDisplayName -eq "Error"})
 
PS C:\> $myFailedEvent.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
DSC Engine Error : 
 Error Message Current configuration does not exist. Execute Start-DscConfiguration command with -Path pa
rameter to specify a configuration file and create a current configuration first. 
Error Code : 1 
```

### 5: Todos los eventos que ha generado un identificador de trabajo determinado.

`$SeparateDscOperations` es una matriz de grupos, cada uno de los cuales tiene como nombre el id. de trabajo único. Al ejecutar el cmdlet `Where-Object`, puede extraer esos grupos de eventos que tienen un identificador de trabajo determinado:

```powershell
PS C:\> ($SeparateDscOperations | Where-Object {$_.Name -eq $jobX} ).Group

   ProviderName: Microsoft-Windows-DSC
 
TimeCreated                     Id LevelDisplayName Message                                               
-----------                     -- ---------------- -------                                               
12/2/2013 4:33:24 PM          4102 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4168 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4146 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...      
12/2/2013 4:33:24 PM          4120 Information      Job {847A5619-5BB2-11E3-BF41-00155D553612} : ...  
```

## Uso de xDscDiagnostics para analizar registros de DSC

**xDscDiagnostics** es un módulo de PowerShell que consta de varias funciones que pueden ayudar a analizar los errores de DSC en la máquina. Estas funciones pueden ayudar a identificar todos los eventos locales de las operaciones de DSC anteriores o los eventos de DSC de los equipos remotos (con credenciales válidas). Aquí, el término operación de DSC se utiliza para definir una única ejecución de DSC desde el inicio hasta el final. Por ejemplo, `Test-DscConfiguration` sería una operación de DSC independiente. De forma similar, todos los demás cmdlets de DSC (como `Get-DscConfiguration`, `Start-DscConfiguration`, etc.) podrían identificarse uno a uno como operaciones de DSC independientes. Las funciones se explican en [xDscDiagnostics](https://github.com/PowerShell/xDscDiagnostics). Puede obtener ayuda si ejecuta `Get-Help <cmdlet name>`.

### Obtención de detalles de las operaciones de DSC 

La función `Get-xDscOperation` permite buscar los resultados de las operaciones de DSC que se ejecutan en uno o varios equipos, y devuelve un objeto que contiene la colección de eventos que ha producido cada operación de DSC. Por ejemplo, en la salida siguiente, se ejecutaron tres comandos. El primero se realizó correctamente y los otros dos no. Estos resultados se resumen en la salida de `Get-xDscOperation`.

```powershell
PS C:\DiagnosticsTest> Get-xDscOperation

ComputerName   SequenceId TimeCreated           Result   JobID                                 AllEvents            
------------   ---------- -----------           ------   -----                                 ---------            
SRV1   1          6/23/2016 9:37:52 AM  Failure  9701aadf-395e-11e6-9165-00155d390509  {@{Message=; TimeC...
SRV1   2          6/23/2016 9:36:54 AM  Failure  7e8e2d6e-395c-11e6-9165-00155d390509  {@{Message=; TimeC...
SRV1   3          6/23/2016 9:36:54 AM  Success  af72c6aa-3960-11e6-9165-00155d390509  {@{Message=Operati...

```

También puede especificar que solo desea los resultados de las últimas operaciones, para lo que debe usar el parámetro `Newest`:

```powershell
PS C:\DiagnosticsTest> Get-xDscOperation -Newest 5
ComputerName   SequenceId TimeCreated           Result   JobID                                 AllEvents            
------------   ---------- -----------           ------   -----                                 ---------            
SRV1   1          6/23/2016 4:36:54 PM  Success                                        {@{Message=; TimeC...
SRV1   2          6/23/2016 4:36:54 PM  Success  5c06402b-399b-11e6-9165-00155d390509  {@{Message=Operati...
SRV1   3          6/23/2016 4:36:54 PM  Success                                        {@{Message=; TimeC...
SRV1   4          6/23/2016 4:36:54 PM  Success  5c06402a-399b-11e6-9165-00155d390509  {@{Message=Operati...
SRV1   5          6/23/2016 4:36:51 PM  Success                                        {@{Message=; TimeC...
```

### Obtención de detalles de los eventos de DSC

El cmdlet Trace-xDscOperation1 devuelve un objeto que contiene una colección de eventos, sus tipos de eventos y la salida de mensajes que genera una operación de DSC concreta. Normalmente, si se encuentra algún error en cualquiera de las operaciones mediante Get-xDscOperation`, se realizará un seguimiento de dicha operación para averiguar cuál de los eventos provocó el error.

Utilice el parámetro `SequenceID` para obtener los eventos de una operación específica de un equipo concreto. Por ejemplo, si en `SequenceID` especifica 9, `Trace-xDscOperaion` obtiene el seguimiento de la operación de DSC que era la novena desde la última operación:

```powershell
PS C:\DiagnosticsTest> Trace-xDscOperation -SequenceID 9

ComputerName   EventType    TimeCreated           Message                                                                                             
------------   ---------    -----------           -------                                                                                             
SRV1   OPERATIONAL  6/24/2016 10:51:52 AM Operation Consistency Check or Pull started by user sid S-1-5-20 from computer NULL.                
SRV1   OPERATIONAL  6/24/2016 10:51:52 AM Running consistency engine.                                                                         
SRV1   OPERATIONAL  6/24/2016 10:51:52 AM The local configuration manager is updating the PSModulePath to WindowsPowerShell\Modules;C:\Prog...
SRV1   OPERATIONAL  6/24/2016 10:51:53 AM  Resource execution sequence :: [WindowsFeature]DSCServiceFeature, [xDSCWebService]PSDSCPullServer. 
SRV1   OPERATIONAL  6/24/2016 10:51:54 AM Consistency engine was run successfully.                                                            
SRV1   OPERATIONAL  6/24/2016 10:51:54 AM Job runs under the following LCM setting. ...                                                       
SRV1   OPERATIONAL  6/24/2016 10:51:54 AM Operation Consistency Check or Pull completed successfully. 
```

Pase el **GUID** asignado a una operación de DSC concreta (el que ha devuelto el cmdlet `Get-xDscOperation`) para obtener los detalles de los eventos de dicha operación de DSC:

```powershell
PS C:\DiagnosticsTest> Trace-xDscOperation -JobID 9e0bfb6b-3a3a-11e6-9165-00155d390509

ComputerName   EventType    TimeCreated           Message                                                                                             
------------   ---------    -----------           -------                                                                                             
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull started by user sid S-1-5-20 from computer NULL.                
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCache.mof                             
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Running consistency engine.                                                                         
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [] Starting consistency engine.                          
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Applying configuration from C:\Windows\System32\Configuration\Current.mof.                          
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Parsing the configuration to apply.                                                                 
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM  Resource execution sequence :: [WindowsFeature]DSCServiceFeature, [xDSCWebService]PSDSCPullServer. 
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Resource ]  [[WindowsFeature]DSCServiceFeature]                      
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_RoleResource with resource name [WindowsFeature]DSC...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Test     ]  [[WindowsFeature]DSCServiceFeature]                      
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[WindowsFeature]DSCServiceFeature] The operation 'Get...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[WindowsFeature]DSCServiceFeature] The operation 'Get...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Test     ]  [[WindowsFeature]DSCServiceFeature] True in 0.3130 sec...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Resource ]  [[WindowsFeature]DSCServiceFeature]                      
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Resource ]  [[xDSCWebService]PSDSCPullServer]                        
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_xDSCWebService with resource name [xDSCWebService]P...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ Start  Test     ]  [[xDSCWebService]PSDSCPullServer]                        
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check Ensure           
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check Port             
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check Physical Path ...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Check State            
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [[xDSCWebService]PSDSCPullServer] Get Full Path for We...
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Test     ]  [[xDSCWebService]PSDSCPullServer] True in 0.0160 seconds.
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]: LCM:  [ End    Resource ]  [[xDSCWebService]PSDSCPullServer]                        
SRV1   VERBOSE      6/24/2016 11:36:56 AM [SRV1]:                            [] Consistency check completed.                          
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCache.mof                             
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Consistency engine was run successfully.                                                            
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Job runs under the following LCM setting. ...                                                       
SRV1   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull completed successfully.                                         
SRV1   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCache.mof
```

Tenga en cuenta que, como `Trace-xDscOperation` agrega eventos de los registros analítico, de depuración y operativo, se le pedirá que habilite estos registros como se describió anteriormente.

Como alternativa, puede recopilar información sobre los eventos si guarda el resultado de `Trace-xDscOperation` en una variable. Puede utilizar los comandos siguientes para mostrar todos los eventos de una operación de DSC concreta.

```powershell
PS C:\DiagnosticsTest> $Trace = Trace-xDscOperation -SequenceID 4

PS C:\DiagnosticsTest> $Trace.Event
```

Esto mostrará los mismos resultados que el cmdlet `Get-WinEvent`, como se muestra en la salida siguiente:

```powershell
   ProviderName: Microsoft-Windows-DSC

TimeCreated                     Id LevelDisplayName Message                                                                                           
-----------                     -- ---------------- -------                                                                                           
6/23/2016 1:36:53 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 1:36:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 2:07:00 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 2:07:01 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 2:36:55 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 2:36:56 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 3:06:55 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 3:06:55 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 3:36:55 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 3:36:55 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 4:06:53 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 4:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 4:36:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 4:36:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 5:06:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 5:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 5:36:54 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 5:36:54 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 6:06:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 6:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 6:36:56 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 6:36:57 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 7:06:52 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 7:06:53 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 7:36:53 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.     
6/23/2016 7:36:54 AM          4343 Information      The DscTimer has successfully run LCM method PerformRequiredConfigurationChecks with flag 5.      
6/23/2016 8:06:54 AM          4312 Information      The DscTimer is running LCM method PerformRequiredConfigurationChecks with the flag set to 5.
```

Idealmente, primero deberá utilizar `Get-xDscOperation` para enumerar las últimas configuraciones DSC que se han ejecutado en las máquinas. A continuación, puede examinar cualquier operación única (mediante su valor de SequenceID o JobID) con `Trace-xDscOperation` para descubrir lo que hizo en segundo plano.

### Obtención de eventos de un equipo remoto

Utilice el parámetro `ComputerName` del cmdlet `Trace-xDscOperation` para obtener los detalles de los eventos de un equipo remoto. Para ello, antes es preciso crear una regla de firewall que permita la administración remota en el equipo remoto:

```powershell
New-NetFirewallRule -Name "Service RemoteAdmin" -DisplayName "Remote" -Action Allow
```
A partir de ahí se puede especificar dicho equipo en la llamada a `Trace-xDscOperation`:

```powershell
PS C:\DiagnosticsTest> Trace-xDscOperation -ComputerName SRV2 -Credential Get-Credential -SequenceID 5

ComputerName   EventType    TimeCreated           Message
------------   ---------    -----------           -------
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull started by user sid S-1-5-20 f...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCach...
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Running consistency engine.
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [] Starting consistency...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Applying configuration from C:\Windows\System32\Configuration\Curr...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Parsing the configuration to apply.
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM  Resource execution sequence :: [WindowsFeature]DSCServiceFeature,...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Resource ]  [[WindowsFeature]DSCSer...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_RoleResource with re...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Test     ]  [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Test     ]  [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Resource ]  [[WindowsFeature]DSCSer...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Resource ]  [[xDSCWebService]PSDSCP...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Executing operations for PS DSC resource MSFT_xDSCWebService with ...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ Start  Test     ]  [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Test     ]  [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]: LCM:  [ End    Resource ]  [[xDSCWebService]PSDSCP...
SRV2   VERBOSE      6/24/2016 11:36:56 AM [SRV2]:                            [] Consistency check co...
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCach...
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Consistency engine was run successfully.
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Job runs under the following LCM setting. ...
SRV2   OPERATIONAL  6/24/2016 11:36:56 AM Operation Consistency Check or Pull completed successfully.
SRV2   ANALYTIC     6/24/2016 11:36:56 AM Deleting file from C:\Windows\System32\Configuration\DSCEngineCach...



## My resources won’t update: How to reset the cache

The DSC engine caches resources implemented as a PowerShell module for efficiency purposes. However, this can cause problems when you are authoring a resource and testing it simultaneously because DSC will load the cached version until the process is restarted. The only way to make DSC load the newer version is to explicitly kill the process hosting the DSC engine.

Similarly, when you run `Start-DscConfiguration`, after adding and modifying a custom resource, the modification may not execute unless, or until, the computer is rebooted. This is because DSC runs in the WMI Provider Host Process (WmiPrvSE), and usually, there are many instances of WmiPrvSE running at once. When you reboot, the host process is restarted and the cache is cleared.

To successfully recycle the configuration and clear the cache without rebooting, you must stop and then restart the host process. This can be done on a per instance basis, whereby you identify the process, stop it, and restart it. Or, you can use `DebugMode`, as demonstrated below, to reload the PowerShell DSC resource.

To identify which process is hosting the DSC engine and stop it on a per instance basis, you can list the process ID of the WmiPrvSE which is hosting the DSC engine. Then, to update the provider, stop the WmiPrvSE process using the commands below, and then run **Start-DscConfiguration** again.

```powershell
###
### find the process that is hosting the DSC engine
###
$dscProcessID = Get-WmiObject msft_providers | 
Where-Object {$_.provider -like 'dsccore'} | 
Select-Object -ExpandProperty HostProcessIdentifier 

###
### Stop the process
###
Get-Process -Id $dscProcessID | Stop-Process
```

## Uso de DebugMode

Puede configurar el administrador de configuración de local (LCM) de DSC para que utilice `DebugMode` y siempre se borre la memoria caché cuando se reinicie el proceso de host. Cuando se establece en **TRUE**, provoca que el motor siempre vuelva a cargar el recurso de DSC de PowerShell. Cuando haya terminado de escribir el recurso, se puede volver a establecer en **FALSE** y el motor volverá al comportamiento de almacenamiento en caché de los módulos.

A continuación se incluye una demostración donde se aprecia cómo `DebugMode` puede actualizar automáticamente la memoria caché. En primer lugar, echemos un vistazo a la configuración predeterminada:

```
PS C:\> Get-DscLocalConfigurationManager
 
 
AllowModuleOverwrite           : False
CertificateID                  : 
ConfigurationID                : 
ConfigurationMode              : ApplyAndMonitor
ConfigurationModeFrequencyMins : 30
Credential                     : 
DebugMode                      : False
DownloadManagerCustomData      : 
DownloadManagerName            : 
LocalConfigurationManagerState : Ready
RebootNodeIfNeeded             : False
RefreshFrequencyMins           : 15
RefreshMode                    : PUSH
PSComputerName                 :  
```

Puede ver que `DebugMode` está establecido en **FALSE**.

Para configurar la demostración de `DebugMode`, utilice el siguiente recurso de PowerShell:

```powershell
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return @{onlyProperty = Get-Content -Path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    "1" | Out-File -PSPath "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"
}
function Test-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    return $false
} 
```

Ahora, cree una configuración con el recurso anterior denominado `TestProviderDebugMode`:

```powershell
Configuration ConfigTestDebugMode
{
    Import-DscResource -Name TestProviderDebugMode
    Node localhost
    {
        TestProviderDebugMode test
        {
            onlyProperty = "blah"
        }
    }
}
ConfigTestDebugMode
```

Verá que el contenido del archivo "**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**" es **1**.

Ahora, actualice el código del proveedor mediante el siguiente script:

```powershell
$newResourceOutput = Get-Random -Minimum 5 -Maximum 30
$content = @"
function Get-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    return @{onlyProperty = Get-Content -Path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    "$newResourceOutput" | Out-File -PSPath C:\OutputFromTestProviderDebugMode.txt
}
function Test-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    return `$false
}
"@ | Out-File -FilePath "C:\Program Files\WindowsPowerShell\Modules\MyPowerShellModules\DSCResources\TestProviderDebugMode\TestProviderDebugMode.psm1
```

Este script genera un número aleatorio y actualiza el código del proveedor en consecuencia. Con `DebugMode` establecido en false, el contenido del archivo "**$env:SystemDrive\OutputFromTestProviderDebugMode.txt**" nunca cambia.

Ahora, establezca `DebugMode` en **TRUE** en el script de configuración:

```powershell
LocalConfigurationManager
{
    DebugMode = $true
} 
```

Cuando vuelva a ejecutar el script anterior, verá que el contenido del archivo es cada vez distinto. (Puede ejecutar `Get-DscConfiguration` para comprobarlo). A continuación se muestra el resultado de dos ejecuciones adicionales (los resultados pueden ser diferentes cuando ejecute el script):

```powershell
PS C:\> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
20                                      localhost                              
 
PS C:\> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
14                                      localhost
```

## Consulte también

### Referencia
* [Recurso de DSC Log](logResource.md)

### Conceptos
* [Crear recursos de configuración de estado deseado de Windows PowerShell personalizados](authoringResource.md)

### Otros recursos
* [Cmdlets de configuración de estado deseado de Windows PowerShell](https://technet.microsoft.com/en-us/library/dn521624(v=wps.630).aspx)




<!--HONumber=Jun16_HO4-->


