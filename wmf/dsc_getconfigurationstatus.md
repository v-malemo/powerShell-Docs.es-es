# Detalles sobre el estado de configuración

El cmdlet `Get-DscConfigurationStatus` obtiene información sobre el estado de configuración de un nodo de destino. Se devuelve un objeto enriquecido que incluye información detallada sobre si la configuración de ejecución era correcta o no. Ya puede adentrarse en el objeto para descubrir los detalles sobre la configuración de ejecución como:

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
```<!--HONumber=Mar16_HO2-->
