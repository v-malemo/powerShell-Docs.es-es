# Uso de un servidor de informes de DSC

> Se aplica a: Windows PowerShell 5.0

> **Nota:** El servidor de informes que se describe en este tema no está disponible en PowerShell 4.0. Para la creación de informes en PowerShell 4.0, consulte Uso de un servidor de cumplimiento de DSC.

El administrador de configuración local (LCM) de un nodo se puede configurar para enviar informes sobre su estado de configuración a un servidor de extracción, que puede consultarse posteriormente para recuperar los datos. Cada vez que el nodo comprueba y aplica
una configuración, envía un informe al servidor de informes. Estos informes se almacenan en una base de datos en el servidor y se pueden recuperar mediante una llamada al servicio web de informes. Cada informe contiene
información como qué configuraciones se aplicaron y lo hicieron correctamente, los recursos usados, los errores que se produjeron y las horas de inicio y finalización.

## Configuración de un nodo para enviar informes

Puede indicar a un nodo que envíe informes a un servidor mediante un bloque **ReportServerWeb** en la configuración del LCM del nodo (para obtener información sobre cómo configurar el LCM,
 consulte [Configuración del administrador de configuración local](metaConfig.md)). El servidor al que el nodo envía los informes debe estar configurado como un servidor de extracción web (no puede enviar informes
 a un recurso compartido SMB). Para obtener información sobre la configuración de un servidor de extracción, consulte [Configuración de un servidor de extracción web de DSC](pullServer.md). El servidor de informes puede ser el mismo servicio desde el que
 el nodo extrae configuraciones y obtiene recursos, o puede ser un servicio diferente.
 
 En el bloque **ReportServerWeb**, debe especificar la dirección URL del servicio de extracción
 y una clave de registro que sea conocida para el servidor.
 
  La configuración siguiente configura un nodo para que extraiga configuraciones de un servicio y envíe informes
 a un servicio en un servidor diferente. 
 
```powershell
[DSCLocalConfigurationManager()]
configuration ReportClientConfig
{
    Node localhost
    {
        Settings
        {
            RefreshMode          = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded   = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL          = 'https://CONTOSO-PULL:8080/PSDSCPullServer.svc'
            RegistrationKey    = 'bbb9778f-43f2-47de-b61e-a0daff474c6d'
            ConfigurationNames = @('ClientConfig')
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL               = 'http://CONTOSO-REPORT:8080/PSDSCReportServer.svc'
            RegistrationKey         = 'ba39daaa-96c5-4f2f-9149-f95c46460faa'
            AllowUnsecureConnection = $true
        }
    }
}
PullClientConfigID
```

## Obtener datos de informes

Los informes enviados al servidor de extracción se introducen en una base de datos del servidor. Los informes están disponibles a través de llamadas al servicio web. Para recuperar los informes para un nodo específico, 
envíe una solicitud HTTP al servicio web de informes de la forma siguiente:
`http://CONTOSO-REPORT:8080/PSDSCReportServer.svc/Nodes(AgentID = MyNodeAgentId)/Reports` 
donde `MyNodeAgentId` es el valor AgentId del nodo para el que desea obtener informes. Puede obtener el valor de AgentId de un nodo mediante una llamada a [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx)
en ese nodo.

Los informes se devuelven como una matriz de objetos JSON.

El script siguiente devuelve los informes para el nodo en el que se ejecuta:

```powershell
function GetReport
{
    param($AgentId = "$((glcm).AgentId)", $serviceURL = "http://CONTOSO-REPORT:8080/PSDSCReportServer.svc")
    $requestUri = "$serviceURL/Nodes(AgentId= '$AgentId')/Reports"
    $request = Invoke-WebRequest -Uri $requestUri  -ContentType "application/json;odata=minimalmetadata;streaming=true;charset=utf-8" `
               -UseBasicParsing -Headers @{Accept = "application/json";ProtocolVersion = "2.0"} `
               -ErrorAction SilentlyContinue -ErrorVariable ev
    $object = ConvertFrom-Json $request.content
    return $object.value
}
```
    
## Visualización de los datos de los informes

Si establece una variable como el resultado de la función **GetReport**, puede ver los campos individuales de un elemento de la matriz que se devuelve:

```powershell
$reports = GetReport
$reports[1]

JobId                : 71515ae8-7294-40a3-8137-fc85bf4b678f
OperationType        : Consistency
RefreshMode          : 
Status               : 
ReportFormatVersion  : 1.0
ConfigurationVersion : 2.0.0
StartTime            : 02/08/2016 01:28:54
EndTime              : 02/08/2016 01:28:57
RebootRequested      : False
Errors               : {}
StatusData           : {{"NumberOfResources":"2","Locale":"en-US","ResourcesInDesiredState":[{"ResourceId":"[WindowsFeature]MyFeatureInstance","SourceI
                       nfo":"C:\\ReportTest\\ClientConfig.ps1::4::9::WindowsFeature","ModuleName":"PsDesiredStateConfiguration","ModuleVersion":"1.0","
                       ConfigurationName":"ClientConfig","ResourceName":"WindowsFeature"},{"ResourceId":"[WindowsFeature]My2ndFeatureInstance","SourceI
                       nfo":"C:\\ReportTest\\ClientConfig.ps1::8::9::WindowsFeature","ModuleName":"PsDesiredStateConfiguration","ModuleVersion":"1.0","
                       ConfigurationName":"ClientConfig","ResourceName":"WindowsFeature"}]}}
```

Observe que el campo **StatusData** es un objeto con tres propiedades: **NumberOfResources**, **Configuración regional** y **ResourcesInDesiredState**. La propiedad **ResourcesInDesiredState**
es una matriz de objetos, cada uno con un número de propiedades. El script siguiente toma un único informe como un parámetro, recorre en iteración su matriz de **ResourcesInDesiredState**
y escribe esos recursos en la consola:
 
```powershell
function GetStatusData
{
    param ($Report)
    $statusData = $Report.StatusData | ConvertFrom-Json

    $Resources = $statusData.ResourcesInDesiredState

    Foreach ($Resource in $Resources)
    {
        Write-Host 'ResourceId: ' $Resource.ResourceId
        Write-Host 'SourceInfo: ' $Resource.SourceInfo
        Write-Host 'ModuleName: ' $Resource.ModuleName
        Write-Host 'ModuleVersion: ' $Resource.ModuleVersion
        Write-Host 'ConfigurationName: ' $Resource.ConfigurationName
        Write-Host 'ResourceName: ' $Resource.ResourceName
        Write-Host
    }
}
```

A continuación se muestra un resultado de ejemplo después de llamar a la función **GetStatusData**:

```powershell
GetStatusData -Report $report[1]

ResourceId:  [WindowsFeature]MyFeatureInstance
SourceInfo:  C:\ReportTest\ClientConfig.ps1::4::9::WindowsFeature
ModuleName:  PsDesiredStateConfiguration
ModuleVersion:  1.0
ConfigurationName:  ClientConfig
ResourceName:  WindowsFeature

ResourceId:  [WindowsFeature]My2ndFeatureInstance
SourceInfo:  C:\ReportTest\ClientConfig.ps1::8::9::WindowsFeature
ModuleName:  PsDesiredStateConfiguration
ModuleVersion:  1.0
ConfigurationName:  ClientConfig
ResourceName:  WindowsFeature
```

Tenga en cuenta que estos ejemplos están diseñados para ofrecerle una idea de lo que puede hacer con los datos del informe. Para obtener una introducción sobre cómo trabajar con JSON en PowerShell, consulte
[Playing with JSON and PowerShell](https://blogs.technet.microsoft.com/heyscriptingguy/2015/10/08/playing-with-json-and-powershell/) (Experimentos con JSON y PowerShell).

## Consulte también
>[Configuración del administrador de configuración local](metaConfig.md)
>[Configuración de un servidor de extracción web de DSC](pullServer.md)
>[Configuración de un cliente de extracción mediante nombres de configuración](pullClientConfigNames.md)
<!--HONumber=Feb16_HO4-->
