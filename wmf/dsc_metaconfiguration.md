# Configurar el LCM de DSC con el nuevo atributo de metaconfiguración

El atributo `DscLocalConfigurationManager` designa un bloque de configuración como una metaconfiguración, que se usa para configurar el Administrador de configuración local de DSC. Este atributo restringe una configuración para que únicamente contenga elementos que configuren el Administrador de configuración local de DSC. Durante el procesamiento, esta configuración genera un archivo `*.meta.mof` que se envía a los nodos de destino apropiados mediante el cmdlet `Set-DscLocalConfigurationManager`.

```powershell
[DscLocalConfigurationManager()]
configuration meta
{
    Node localhost
    {
        Settings
        {
            ConfigurationMode = "ApplyAndAutocorrect"
            ConfigurationID = "5603f952-d6c6-4971-88c4-948636bf5c3f"
            RefreshMode = "Pull"
        }
        ConfigurationRepositoryWeb PullServer
        {
            ServerURL = "https://corp.contoso.com/PSDSCPullServer/PSDSCPullServer.svc"
        }
    }
}
```

En el ejemplo anterior se configura el modo de actualización de LCM para usar el modo de extracción, se cambia el modo de configuración a ApplyAndAutocorrect, y se definen el tipo y la ubicación del servidor de extracción.

Este nuevo atributo de configuración reemplaza y amplía la funcionalidad del recurso LocalConfigurationManager de PowerShell 4.0. Este recurso LocalConfigurationManager todavía se admite en configuraciones sin este atributo para ofrecer compatibilidad con versiones anteriores.

Metarecursos:

| **Nombre del recurso**            | **Descripción**                                |
|------------------------------|------------------------------------------------|
| LocalConfigurationManager    | Distintas configuraciones para la ejecución del motor de DSC      |
| PartialConfiguration         | Parámetros de configuración parcial                 |
| ConfigurationRepositoryWeb   | Repositorio de configuración basado en Web             |
| ConfigurationRepositoryShare | Repositorio de configuración basado en el recurso compartido de archivos      |
| ResourceRepositoryWeb        | Repositorio de recursos basado en Web                  |
| ResourceRepositoryShare      | Repositorio de recursos basado en archivos                 |
| ReportServerWeb              | Punto de conexión de informes basado en Web para el escenario de extracción |
<!--HONumber=Mar16_HO2-->
