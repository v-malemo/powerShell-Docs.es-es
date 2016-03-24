# Configurar el nodo con varios fragmentos de configuración (configuraciones parciales)

WMF 5.0 le ayuda a entregar documentos de configuración a un nodo en fragmentos. Para que un nodo reciba varios fragmentos de un documento de configuración, su Administrador de configuración local (LCM) debe estar establecido para especificar los fragmentos esperados, como se muestra en este ejemplo.

```powershell
[DSCLocalConfigurationManager()]
configuration SQLServerDSCSettings
{
    Node localhost
    {
        Settings
        {
            ConfigurationModeFrequencyMins = 30
        }

        ConfigurationRepositoryWeb OSConfigServer
        {
            ServerURL = "https://corp.contoso.com/OSConfigServer/PSDSCPullServer.svc"
        }

        ConfigurationRepositoryWeb SQLConfigServer
        {
            ServerURL = "https://corp.contoso.com/SQLConfigServer/PSDSCPullServer.svc"
        }

        PartialConfiguration OSConfig
        {
            Description = 'Configuration for the Base OS'
            ExclusiveResources = 'PSDesiredStateConfiguration\*'
            ConfigurationSource = '[ConfigurationRepositoryWeb]OSConfigServer'
        }

        PartialConfiguration SQLConfig
        {
            Description = 'Configuration for the SQL Server'
            ConfigurationSource = '[ConfigurationRepositoryWeb]SQLConfigServer'
            DependsOn = '[PartialConfiguration]OSConfig'
        }
    }
}
```

En una configuración parcial, el nombre de configuración debe coincidir con el definido en el LCM. En el ejemplo anterior, las configuraciones deben denominarse `OSConfig` y `SQLConfig`.

Configurar el LCM para configuraciones parciales permite la coordinación de la configuración, pero no proporciona la funcionalidad de seguridad.<!--HONumber=Mar16_HO2-->
