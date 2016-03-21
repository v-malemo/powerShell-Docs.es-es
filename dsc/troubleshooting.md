# Solución de problemas de DSC

>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

En este tema se describen los métodos para conseguir que los scripts de configuración de estado deseado (DSC) se ejecuten sin errores. Si se usan registros para localizar errores y se comprende cómo se recicla la memoria caché para ver los resultados inmediatos de los cambios de recursos, se podrán solucionar los problemas de DSC de forma más eficaz. Estas técnicas se explican en dos secciones:

* El script no se ejecuta: **uso de registros de DSC para diagnosticar errores de scripts**.
* Los recursos no se actualizan: **restablecer la memoria caché**.

## El script no se ejecuta: uso de registros de DSC para diagnosticar errores de scripts

Como sucede con todo el software de Windows, DSC registra los errores y eventos en [registros](https://msdn.microsoft.com/library/windows/desktop/aa363632.aspx) que se pueden ver en el [Visor de eventos](http://windows.microsoft.com/windows/what-information-event-logs-event-viewer). Examinar estos registros puede ayudarle a entender el motivo del error de una operación determinada y cómo evitar los errores en el futuro. La escritura de scripts de configuración puede ser complicada, por lo que para que sea más fácil realizar un seguimiento de los errores a medida que se crea, utilice el recurso de registro de DSC para hacer un seguimiento del progreso de la configuración en el registro de eventos DSC Analytic.

## ¿Dónde se encuentran los registros de eventos de DSC?

En el Visor de eventos, los eventos de DSC se encuentran en: **Registros de aplicaciones y servicios/Microsoft/Windows/Configuración de estado deseado**.

El cmdlet de PowerShell correspondiente, [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx), también se puede ejecutar para ver los registros de eventos:

```
PS C:\Users> Get-WinEvent -LogName "Microsoft-Windows-Dsc/Operational"
   ProviderName: Microsoft-Windows-DSC
TimeCreated                     Id LevelDisplayName Message                                                                                                  
-----------                     -- ---------------- -------                                                                                                  
11/17/2014 10:27:23 PM        4102 Information      Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
```

Como se muestra arriba, el nombre del registro primario de DSC es **Microsoft->Windows->DSC** (no se muestran otros nombres de registros de Windows por razones de brevedad). El nombre principal se anexa al nombre del canal para crear el nombre del registro completo. El motor de DSC escribe principalmente en tres tipos de registros: [registros operativos, analíticos y de depuración](https://technet.microsoft.com/library/cc722404.aspx). Como los registros analíticos y de depuración están desactivados de forma predeterminada, debe habilitarlos en el Visor de eventos. Para ello, abra el Visor de eventos escribiendo eventvwr en Windows PowerShell, o bien, haga clic en el botón **Inicio**, **Panel de Control**, **Herramientas administrativas** y luego haga clic en **Visor de eventos**. En el menú **Vista** del Visor de eventos, haga clic en **Mostrar registros analíticos y de depuración**. El nombre del registro del canal analítico **Microsoft-Windows-Dsc/Analytic** y el del canal de depuración **Microsoft-Windows-Dsc/Debug**. También puede usar la utilidad [wevtutil](https://technet.microsoft.com/library/cc732848.aspx) para habilitar los registros, como se muestra en el ejemplo siguiente.

```powershell
wevtutil.exe set-log “Microsoft-Windows-Dsc/Analytic” /q:true /e:true
```

## ¿Qué contienen los registros de DSC?

Los registros de DSC se dividen en los tres canales de registro según la importancia del mensaje. El registro operativo de DSC contiene todos los mensajes de error y puede utilizarse para identificar un problema. El registro analítico tiene un mayor volumen de eventos y puede identificar dónde se han producido errores. Este canal también contiene los mensajes detallados (si existen). El registro de depuración contiene registros que pueden ayudarle a entender cómo se produjeron los errores. Los mensajes de los eventos de DSC se estructuran de forma que cada mensaje de evento comienza por un identificador de trabajo que representa de forma única una operación de DSC. En el ejemplo siguiente intenta obtener el mensaje desde el primer evento registrado en el registro operativo de DSC.

```powershell
PS C:\Users> $AllDscOpEvents=get-winevent -LogName "Microsoft-Windows-Dsc/Operational"
PS C:\Users> $FirstOperationalEvent=$AllDscOpEvents[0]
PS C:\Users> $FirstOperationalEvent.Message
Job {02C38626-D95A-47F1-9DA2-C1D44A7128E7} : 
Consistency engine was run successfully. 
```

Los eventos de DSC se registran en una estructura determinada que permite al usuario agrupar los eventos de un trabajo de DSC. La estructura es la siguiente:

**Id. de trabajo : \<Guid\>**
**\<Mensaje de evento\>**

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
 
$DscEvents=[System.Array](Get-WinEvent "Microsoft-windows-dsc/operational") `
         + [System.Array](Get-WinEvent "Microsoft-Windows-Dsc/Analytic" -Oldest) `
         + [System.Array](Get-Winevent "Microsoft-Windows-Dsc/Debug" -Oldest)
 
 
<##########################################################################
 Step 4 : Group all logs based on the job ID
###########################################################################>
$SeparateDscOperations=$DscEvents | Group {$_.Properties[0].value}  
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
PS C:\> $SeparateDscOperations  | Where-Object {$_.Group.LevelDisplayName -contains "Error"}
Count Name                      Group                                                                     
----- ----                      -----                                                                     
   38 {5BCA8BE7-5BB6-11E3-BF... {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics....
```

### 2: Detalles de las operaciones que se ejecutaron en última media hora

`TimeCreated`, una propiedad de todos los eventos de Windows, indica la hora en que se creó el evento. Se puede hacer una comparación de esta propiedad con un objeto de fecha y hora determinado para filtrar todos los eventos:

```powershell
PS C:\> $DateLatest=(Get-date).AddMinutes(-30)
PS C:\> $SeparateDscOperations  | Where-Object {$_.Group.TimeCreated -gt $DateLatest}
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
PS C:\> $myFailedEvent=($SeparateDscOperations[0].Group | Where-Object {$_.LevelDisplayName -eq "Error"})
 
PS C:\> $myFailedEvent.Message
Job {5BCA8BE7-5BB6-11E3-BF41-00155D553612} : 
DSC Engine Error : 
 Error Message Current configuration does not exist. Execute Start-DscConfiguration command with -Path pa
rameter to specify a configuration file and create a current configuration first. 
Error Code : 1 
```

### 5: Todos los eventos que ha generado un identificador de trabajo determinado.

`$SeparateDscOperations` es una matriz de grupos, cada uno de los cuales tiene como nombre el identificador de trabajo único. Al ejecutar el cmdlet `Where-Object`, puede extraer esos grupos de eventos que tienen un identificador de trabajo determinado:

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

**xDscDiagnostics** es un módulo de PowerShell que consta de dos operaciones sencillas que pueden ayudar a analizar los errores de DSC de la máquina: `Get-xDscOperation` y `Trace-xDscOperation`. Estas funciones pueden ayudar a identificar todos los eventos locales de las operaciones de DSC anteriores o los eventos de DSC de los equipos remotos (con credenciales válidas). Aquí, el término operación de DSC se utiliza para definir una única ejecución de DSC desde el inicio hasta el final. Por ejemplo, `Test-DscConfiguration` sería una operación de DSC independiente. De forma similar, todos los demás cmdlets de DSC (como `Get-DscConfiguration`, `Start-DscConfiguration`, etc.) podrían identificarse uno a uno como operaciones de DSC independientes. Los dos cmdlets se explican en el módulo de PowerShell [xDscDiagnostics](https://powershellgallery.com/packages/xDscDiagnostics) (Kit de recursos de DSC) y con más detalle a continuación. Puede obtener ayuda si ejecuta `Get-Help <cmdlet name>`.

## Get-xDscOperation

Este cmdlet permite buscar los resultados de las operaciones de DSC que se ejecutan en uno o varios equipos, y devuelve un objeto que contiene la colección de eventos que ha producido cada operación de DSC. Por ejemplo, en la salida siguiente, se ejecutaron tres comandos. El primero se realizó correctamente y los otros dos no. Estos resultados se resumen en la salida de `Get-xDscOperation`.

TAREA: Reemplazar esta imagen que muestra la salida de Get-xDscOperation

### Parámetros

* **Newest**: acepta un valor entero para indicar el número de operaciones que se mostrarán. De forma predeterminada, devuelve las 10 operaciones más recientes. Por ejemplo:
  TAREA: Mostrar Get-xDscOperation -Newest 5
* **ComputerName**: parámetro que acepta una matriz de cadenas, cada una de las cuales debe contener el nombre de un equipo del que quiera recopilar datos de registros de eventos de DSC. De forma predeterminada, recopila los datos del equipo host. Para habilitar esta característica, debe ejecutar el comando siguiente en las máquinas remotas, en modo elevado para que se permita la recopilación de eventos.
```powershell
  New-NetFirewallRule -Name "Service RemoteAdmin" -Action Allow
```
* **Credential**: parámetro de tipo PSCredential, que puede ayudar a acceder a los equipos especificados en el parámetro ComputerName.

### Objeto devuelto

El cmdlet devuelve una matriz de objetos de tipo **Microsoft.PowerShell.xDscDiagnostics.GroupedEvents**. Cada objeto de esta matriz pertenece a una operación de DSC diferente. La visualización predeterminada para este objeto tiene las siguientes propiedades:
* **SequenceID**: especifica el número incremental asignado a la operación de DSC en función del tiempo. Por ejemplo, la última operación ejecutada tendría un valor de SequenceID de 1, la penúltima operación de DSC tendría un valor de SequenceID de 2 y así sucesivamente. Este número es otro identificador para cada objeto de la matriz devuelta.
* **TimeCreated**: un valor de fecha y hora que indica cuándo se inició la operación de DSC.
* **ComputerName**: el nombre del equipo desde donde se agrupan los resultados.
* **Result**: una cadena con el valor **Failure** o **Success** que indica si en esa operación de DSC se produjo un error o se realizó correctamente, respectivamente.
* **AllEvents**: un objeto que representa una colección de eventos que ha producido la operación de DSC.

Por ejemplo, en el resultado siguiente se muestran los resultados de la última operación desde varios equipos:
  TAREA: Sustituir la imagen de Get-xDscOperation para que muestre los registros de equipos remotos

## Trace-xDscOperation

Este cmdlet devuelve un objeto que contiene una colección de eventos, sus tipos de eventos y la salida del mensaje generado a partir de una determinada operación de DSC. Normalmente, cuando encuentre un error en cualquiera de las operaciones con `Get-xDscOperation`, realizará un seguimiento de esa operación para averiguar qué evento provocó el error.

### Parámetros

* **SequenceID**: este es el valor entero asignado a cualquier operación, que pertenece a un equipo concreto. Al especificar un valor de SequenceID de, por ejemplo, 4, se mostrará el seguimiento de la operación de DSC que estaba en la posición cuarta desde el final.

Trace-xDscOperation con identificador de secuencia especificado
* **JobID**: este es el valor GUID que ha asignado el LCM xDscOperation para identificar de forma única una operación. Cuando se especifica un valor de JobID, se muestra el seguimiento de la operación de DSC correspondiente.
  TAREA: Reemplazar la imagen de Trace-xDscOperation que acepta un elemento JobID como parámetro
* **ComputerName** y **Credential**: estos parámetros permiten que el seguimiento se recopile de los equipos remotos:
```powershell
New-NetFirewallRule -Name "Service RemoteAdmin" -Action Allow
```
  TAREA: Reemplazar la imagen de la ejecución de Trace-xDscOperation en un proceso distinto

Tenga en cuenta que como `Trace-xDscOperation` agrega eventos de los registros analítico, de depuración y operativo, se le pedirá que habilite estos registros como se describió anteriormente.

### Objeto devuelto

El cmdlet devuelve una matriz de objetos, todos ellos de tipo `Microsoft.PowerShell.xDscDiagnostics.TraceOutput`. Cada uno de los objetos de esta matriz contiene los siguientes campos:
* **ComputerName**: el nombre del equipo desde donde se recopilan los registros.
* **EventType**: se trata de un campo de tipo enumerador que contiene información sobre el tipo de evento. Podría ser cualquiera de los siguientes:
  - *Operational*: el evento procede del registro operativo.
  - *Analytic*: el evento procede del registro analítico.
  - *Debug*: el evento procede del registro de depuración.
  - *Verbose*: los eventos se muestran como mensajes detallados durante la ejecución. Los mensajes detallados facilitan la identificación de la secuencia de los eventos que se publican.
  - *Error*: eventos de error. Al visualizar los eventos de error, normalmente puede encontrar de forma rápida el motivo del error.
* **TimeCreated**: un valor de fecha y hora que indica cuándo registró el evento DSC.
* **Message**: el mensaje que registró DSC en los registros de eventos.

A continuación se muestran los campos de este objeto que se pueden utilizar para obtener más información sobre el evento, pero no se muestran de forma predeterminada:

* **JobID**: el identificador de trabajo (en formato GUID) específico de esa operación de DSC.
* **SequenceID**: el valor de SequenceID único para esa operación de DSC en ese equipo.
* **Evento**: este es el evento real que DSC ha registrado, de tipo `System.Diagnostics.Eventing.Reader.EventLogRecord`. También se puede obtener si ejecuta el cmdlet `Get-WinEvent`. Contiene más información, como la tarea, el valor eventID y el nivel del evento.

Como alternativa, puede recopilar información sobre los eventos si guarda el resultado de `Trace-xDscOperation` en una variable. Puede utilizar el comando siguiente para mostrar todos los eventos de una operación de DSC determinada:

```powershell
(Trace-xDscOperation -SequenceID 3).Event
```

Esto mostrará los mismos resultados que el cmdlet `Get-WinEvent`, como se muestra en la salida siguiente:
  TAREA: ¿Qué resultado?

Idealmente, primero deberá utilizar `Get-xDscOperations` para enumerar las últimas configuraciones DSC que se han ejecutado en las máquinas. A continuación, puede examinar cualquier operación única (mediante su valor de sequenceID o JobID) con `Trace-xDscOperation` para descubrir lo que hizo en segundo plano.

## Los recursos no se actualizan: restablecer la memoria caché

El motor de DSC almacena en caché los recursos implementados como un módulo de PowerShell por motivos de eficiencia. Sin embargo, esto puede provocar problemas cuando está creando un recurso y probándolo al mismo tiempo, ya que DSC cargará la versión en caché hasta que se reinicie el proceso. La única manera de conseguir que DSC cargue la versión más reciente es detener explícitamente el proceso que hospeda el motor de DSC.

De forma similar, cuando se ejecuta `Start-DSCConfiguration`, después de agregar y modificar un recurso personalizado, es posible que la modificación no se ejecute a menos que (o hasta que) se reinicie el equipo. Esto se debe a que DSC se ejecuta en el proceso host del proveedor de WMI (WmiPrvSE) y, normalmente, existen muchas instancias de WmiPrvSE que se ejecutan a la vez. Al reiniciar, el proceso de host se reinicia y la memoria caché se borra.

Para reciclar la configuración de correctamente y borrar la memoria caché sin necesidad de reiniciar, debe detener y después reiniciar el proceso del host. Esto puede hacerse por instancia, en cuyo caso se identifica el proceso, se detiene y se reinicia. O bien, puede usar `DebugMode`, como se muestra a continuación, para volver a cargar el recurso de DSC de PowerShell.

Para identificar el proceso que hospeda el motor de DSC y detenerlo por instancia, puede mostrar el identificador de proceso del elemento WmiPrvSE que hospeda el motor de DSC. A continuación, para actualizar el proveedor, detenga el proceso WmiPrvSE mediante los comandos siguientes y luego ejecute de nuevo **Start-DscConfiguration**.

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
PS C:\Users\WinVMAdmin\Desktop> Get-DscLocalConfigurationManager
 
 
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
    return @{onlyProperty = Get-Content -path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        $onlyProperty
    )
    "1"|Out-File -PSPath "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"
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
    return @{onlyProperty = Get-Content -path "$env:SystemDrive\OutputFromTestProviderDebugMode.txt"}
}
function Set-TargetResource
{
    param
    (
        [Parameter(Mandatory)]
        `$onlyProperty
    )
    "$newResourceOutput"|Out-File -PSPath C:\OutputFromTestProviderDebugMode.txt
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
PS C:\Users\WinVMAdmin\Desktop> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
20                                      localhost                              
 
PS C:\Users\WinVMAdmin\Desktop> Get-DscConfiguration -CimSession (New-CimSession localhost)
 
onlyProperty                            PSComputerName                         
------------                            --------------                         
14 
```

## Consulte también

### Referencia
* [Recurso de DSC Log](logResource.md)

### Conceptos
* [Crear recursos de configuración de estado deseado de Windows PowerShell personalizados](authoringResource.md)

### Otros recursos
* [Cmdlets de configuración de estado deseado (DSC) de Windows PowerShell](https://technet.microsoft.com/en-us/library/dn521624(v=wps.630).aspx)
<!--HONumber=Feb16_HO4-->
