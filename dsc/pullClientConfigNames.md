# Configuración de un cliente de extracción mediante nombres de configuración

> Se aplica a: Windows PowerShell 5.0

Es necesario indicar a cada nodo de destino que debe usar el modo de extracción y se le debe facilitar la dirección URL donde puede establecer contacto con el servidor de extracción para obtener las configuraciones. Para ello, tendrá que configurar el administrador de configuración local (LCM) con la información necesaria. Para configurar el LCM, cree un tipo especial de configuración, decorada con el atributo **DSCLocalConfigurationManager**. Para más información sobre la configuración del LCM, consulte [Configuración del administrador de configuración local](metaConfig.md).

> **Nota**: Este tema se aplica a PowerShell 5.0. Para obtener información sobre cómo configurar un cliente de incorporación de cambios en PowerShell 4.0, consulte [Configuración de un cliente de incorporación de cambios con el id. de configuración de PowerShell 4.0](pullClientConfigID4.md).

El script siguiente configura el LCM para que extraiga configuraciones de un servidor denominado "CONTOSO-PullSrv":

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
            ConfigurationNames = @('ClientConfig')
            
        }      
    }
}
PullClientConfigID
```

En el script, el bloque **ConfigurationRepositoryWeb** define el servidor de incorporación de cambios. La propiedad **ServerURL** especifica el punto de conexión del servidor de incorporación de cambios.

La propiedad **RegistrationKey** es una clave compartida entre todos los nodos de cliente de un servidor de incorporación de cambios y ese servidor de incorporación de cambios. El mismo valor se almacena en un archivo en el servidor de extracción. La propiedad **ConfigurationNames** especifica el nombre de la configuración prevista para el nodo de cliente. En el servidor de incorporación de cambios, el archivo MOF de configuración de este nodo de cliente debe denominarse *ConfigurationNames*.mof, donde *ConfigurationNames* coincida con el valor de la propiedad **ConfigurationNames** establecida en este metaconfiguración.

Después de que se ejecute este script, se crea una nueva carpeta de salida denominada **PullClientConfigID** y se coloca un archivo MOF de metaconfiguración en ella. En este caso, el nombre del archivo MOF de metaconfiguración será `localhost.meta.mof`.

Para aplicar la configuración, llame al cmdlet **Set-DscLocalConfigurationManager**, con el valor de **Path** establecido en la ubicación del archivo MOF de metaconfiguración. Por ejemplo: `Set-DSCLocalConfigurationManager localhost –Path .\PullClientConfigID –Verbose.`

> **Nota**: Las claves de registro solo funcionan con servidores de incorporación de cambios web. Deberá seguir usando el elemento **ConfigurationID** con un servidor de incorporación de cambios SMB. Para obtener información sobre cómo configurar un servidor de incorporación de cambios mediante **ConfigurationID**, consulte [Configuración de un cliente de incorporación de cambios con el id. de configuración](pullClientConfigID.md)

## Servidores de informes y recursos

Si solo especifica un bloque **ConfigurationRepositoryWeb** o **ConfigurationRepositoryShare** en su configuración del LCM (como en el ejemplo anterior), el cliente de incorporación de cambios extraerá 
recursos del servidor especificado, pero no le enviará informes. Puede utilizar un solo servidor de incorporación de cambios para configuraciones, recursos e informes, pero debe crear un 
bloque **ReportRepositoryWeb** para configurar los informes. En el ejemplo siguiente se muestra una metaconfiguration que configura un cliente para que extraiga configuraciones y recursos, y envíe informes de datos, a un único
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
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = 'fbc6ef09-ad98-4aad-a062-92b0e0327562'
        }
        
        

        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```


También puede especificar servidores de incorporación de cambios diferentes para los recursos y los informes. Para especificar un servidor de recursos, utilice un bloque **ResourceRepositoryWeb** (para un servidor de incorporación de cambios web) o un 
bloque **ResourceRepositoryShare** (para un servidor de incorporación de cambios SMB).
Para especificar un servidor de informes, utilice un bloque **ReportRepositoryWeb**. Un servidor de informes no puede ser un servidor SMB.
La metaconfiguración siguiente configura un cliente de incorporación de cambios para que obtenga sus configuraciones de **CONTOSO-PullSrv** y sus recursos de **CONTOSO-ResourceSrv**, y para que envíe los informes de estado a **CONTOSO-ReportSrv**:

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30 
            RebootNodeIfNeeded = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = 'fbc6ef09-ad98-4aad-a062-92b0e0327562'
        }
        
        ResourceRepositoryWeb CONTOSO-ResourceSrv
        {
            ServerURL = 'https://CONTOSO-ResourceSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '30ef9bd8-9acf-4e01-8374-4dc35710fc90'
        }

        ReportServerWeb CONTOSO-ReportSrv
        {
            ServerURL = 'https://CONTOSO-ReportSrv:8080/PSDSCPullServer.svc'
        }
    }
}
PullClientConfigID
```

## Consulte también

* [Configuración de un cliente de incorporación de cambios con el id. de configuración](pullClientConfigID.md)
* [Configuración de un servidor de extracción web de DSC](pullServer.md)


<!--HONumber=Mar16_HO4-->


