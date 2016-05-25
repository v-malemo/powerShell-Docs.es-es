# Configuración de un cliente de extracción mediante id. de configuración

> Se aplica a: Windows PowerShell 5.0

Es necesario indicar a cada nodo de destino que debe usar el modo de extracción y se le debe facilitar la dirección URL donde puede establecer contacto con el servidor de extracción para obtener las configuraciones. Para ello, tendrá que configurar el administrador de configuración local (LCM) con la información necesaria. Para configurar el LCM, debe crear un tipo especial de configuración, haya sido decorado con el atributo **DSCLocalConfigurationManager**. Para más información sobre la configuración del LCM, consulte [Configuración del administrador de configuración local](metaConfig.md)..

> **Nota**: Este tema se aplica a PowerShell 5.0. Para obtener información sobre cómo configurar un cliente de incorporación de cambios en PowerShell 4.0, consulte [Configuración de un cliente de incorporación de cambios con el id. de configuración de PowerShell 4.0](pullClientConfigID4.md).

El script siguiente configura el LCM para que extraiga configuraciones de un servidor denominado "CONTOSO-PullSrv".

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            
        }      
    }
}
PullClientConfigID
```

En el script, el bloque **ConfigurationRepositoryWeb** define el servidor de incorporación de cambios. El elemento **ServerURL**

Después de que se ejecute este script, se crea una nueva carpeta de salida denominada **PullClientConfigID** y se coloca un archivo MOF de metaconfiguración en ella. En este caso, el nombre del archivo MOF de metaconfiguración será `localhost.meta.mof`.

Para aplicar la configuración, llame al cmdlet **Set-DscLocalConfigurationManager**, con el valor de **Path** establecido en la ubicación del archivo MOF de metaconfiguración. Por ejemplo: `Set-DSCLocalConfigurationManager localhost –Path .\PullClientConfigID –Verbose.`

## Id. de configuración

El script establece la propiedad **ConfigurationID** del LCM en un GUID que se había creado anteriormente para este fin (puede crear un GUID mediante el cmdlet **New-Guid**). La propiedad **ConfigurationID** es lo que usa el LCM para buscar la configuración adecuada en el servidor de incorporación de cambios. El archivo MOF de configuración del servidor de incorporación de cambios debe denominarse _ConfigurationID_.mof, donde _ConfigurationID_ es el valor de la propiedad **ConfigurationID** del LCM del nodo de destino.

## Servidor de extracción SMB

Para configurar un cliente que extraiga configuraciones de un servidor SMB, use un bloque **ConfigurationRepositoryShare**. En un bloque **ConfigurationRepositoryShare**, especifique la ruta de acceso al servidor mediante el establecimiento de la propiedad **SourcePath**. La metaconfiguración siguiente configura el nodo de destino para que se extraiga de un servidor de incorporación de cambios SMB denominado **SMBPullServer**..

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb SMBPullServer
        {
            SourcePath = '\\SMBPullServer\PullSource'
            
        }     
    }
}
PullClientConfigID
```

## Servidores de informes y recursos

Si solo especifica un bloque **ConfigurationRepositoryWeb** o **ConfigurationRepositoryShare** en su configuración del LCM (como en el ejemplo anterior), el cliente de incorporación de cambios extraerá 
recursos del servidor especificado, pero no le enviará informes. Puede utilizar un solo servidor de incorporación de cambios para configuraciones, recursos e informes, pero debe crear un 
bloque **ReportRepositoryWeb** para configurar los informes. 

En el ejemplo siguiente se muestra una metaconfiguration que configura un cliente para que extraiga configuraciones y recursos, y envíe informes de datos, a un único
servidor de incorporación de cambios.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            
        }
        
        
        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

También puede especificar servidores de incorporación de cambios diferentes para los recursos y los informes. Para especificar un servidor de recursos, use un bloque **ResourceRepositoryWeb** (para un servidor de incorporación de cambios web) o un 
bloque **ResourceRepositoryShare** (para un servidor de incorporación de cambios SMB).
Para especificar un servidor de informes, utilice un bloque **ReportRepositoryWeb**. Un servidor de informes no puede ser un servidor SMB.
La metaconfiguración siguiente configura un cliente de extracción para que obtenga sus configuraciones de **CONTOSO-PullSrv** y sus recursos de **CONTOSO-ResourceSrv**, y para que envíe los informes a **CONTOSO-ReportSrv**:

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            ConfigurationID = '1d545e3b-60c3-47a0-bf65-5afc05182fd0'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            
        }
        
        ResourceRepositoryWeb CONTOSO-ResourceSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL = 'https://CONTOSO-REsourceSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

## Consulte también

* [Configuración de un cliente de incorporación de cambios con nombres de configuración](pullClientConfigNames.md)


<!--HONumber=May16_HO2-->


