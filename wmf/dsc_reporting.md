# Notificar el estado de configuración a una ubicación central

Se puede enviar información detallada sobre el estado de configuración de DSC a un servidor que ejecute el servicio DSC. La misma información que devuelve el cmdlet Get-DscConfigurationStatus se envía al servicio DSC. Al definir el servidor de informes en una metaconfiguración, este estado se envía al servidor cuando se produce un error o se completa correctamente la configuración. Esta información se envía cuando un nodo está configurado en modo de inserción o de extracción. La información de estado se almacena en la base de datos del servicio DSC.

## Metaconfiguración de ejemplo para notificar el estado
```PowerShell
[DscLocalConfigurationManager()]
Configuration ReportingClientMetaConfig
{
    Settings
        {
            RefreshFrequencyMins = 30
            RefreshMode = "PUSH"
            ConfigurationModeFrequencyMins = 15
            AllowModuleOverwrite = $true
        }

        ReportServerWeb ReportManager
        {
            ServerUrL = "http://localhost:8080/PSDSCPullServer/PSDSCPullserver.svc"
            AllowUnsecureConnection  = $true
        }           
}
```
Se crea un nuevo punto de conexión de OData con el servicio DSC que expone esta información de estado. Al pasar un identificador de configuración o de agente {$guid} al punto de conexión, se puede recopilar y analizar todo el estado de un nodo.

## Solicitud web de ejemplo para recopilar el estado de configuración 
```PowerShell
$statusReports = Invoke-WebRequest -Uri "[http://localhost:8080/PSDSCPullserver/PSDSCPullserver.svc/Node(ConfigurationId='$guid')/StatusReport](http://localhost:8080/PSDSCPullserver/psdscpullserver.svc/Node(ConfigurationId='$guid')/StatusReport)s" -UseBasicParsing -UseDefaultCredentials -ContentType "application/json;odata=minimalmetadata;streaming=true;charset=utf-8" -Headers @{Accept = "application/json"; ProtocolVersion = “1.1”}
```<!--HONumber=Mar16_HO2-->
