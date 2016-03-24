# Valor adicional de la propiedad RefreshMode

Esta versión introduce un nuevo valor `RefreshMode`, **Disabled**. Cuando se establece este modo, LCM no documenta la administración. Este modo se puede usar cuando se emplea una herramienta de administración de configuración de terceros en lugar de DSC, mientras se siguen usando los recursos de DSC con el cmdlet `Invoke-DscResource` o si solo necesita detener el procesamiento de DSC por cualquier motivo.

```powershell
Configuration LCMSettings
{
    Node localhost
    {
        LocalConfigurationManager
        {
           RefreshMode = 'Disabled'
        }
    }
}

LCMSettings -OutputPath .\LCMSettings
Set-DscLocalConfigurationManager -Path .\LCMSettings -Verbose -ComputerName localhost
```
<!--HONumber=Mar16_HO2-->
