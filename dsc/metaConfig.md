---
title:   Configuración del administrador de configuración local
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Configuración del administrador de configuración local

> Se aplica a: Windows PowerShell 5.0

El administrador de configuración local (LCM) es el motor de la configuración de estado deseado (DSC) de Windows PowerShell. El LCM se ejecuta en cada nodo de destino y es el responsable de analizar y establecer las configuraciones que se envían al nodo. También es responsable de diversos otros aspectos de DSC, entre los que se incluyen los siguientes.

* Determinar el modo de actualización (inserción o extracción).
* Especificar la frecuencia con que un nodo extrae y aplica configuraciones.
* Asociar el nodo con los servidores de extracción.
* Especificar configuraciones parciales.

Se utiliza un tipo especial de configuración para configurar el LCM a fin de que especifique cada uno de estos comportamientos. En las siguientes secciones se describe cómo configurar el LCM.

> **Nota**: Este tema se aplica al LCM que se introdujo en Windows PowerShell 5.0. Para obtener información sobre cómo configurar el LCM en Windows PowerShell 4.0, consulte [Administrador de configuración local (LCM) de la configuración de estado deseado de Windows PowerShell 4.0](metaconfig4.md).

## Escritura y aplicación de una configuración de LCM

Para configurar el LCM, debe crear y ejecutar un tipo especial de configuración. Para especificar una configuración de LCM, utilice el atributo DscLocalConfigurationManager. A continuación se muestra una configuración sencilla que establece el LCM en el modo de inserción.

```powershell
[DSCLocalConfigurationManager()]
configuration LCMConfig
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Push'
        }
    }
} 
```

Debe llamar y ejecutar la configuración para crear el MOF de configuración, tal y como lo haría con una configuración normal (para obtener información sobre la creación del MOF de configuración, consulte [Compilación de la configuración](configurations.md#compiling-the-configuration)). A diferencia de las configuraciones normales, no aplique una configuración LCM con una llamada al cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx). En su lugar, llame al cmdlet [Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx) y proporcione la ruta de acceso al MOF de configuración como un parámetro. Después de establecer la configuración, podrá ver las propiedades del LCM con una llamada al cmdlet [Get-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn407378.aspx).

Una configuración de LCM puede contener bloques solo para un conjunto limitado de recursos. En el ejemplo anterior, el único recurso al que se llama es **Settings**. El resto de recursos disponibles son:

* **ConfigurationRepositoryWeb**: especifica un servidor de incorporación de cambios HTTP para configuraciones. 
* **ConfigurationRepositoryShare**: especifica un servidor de incorporación de cambios SMB para configuraciones.
* **ResourceRepositoryWeb**: especifica un servidor de incorporación de cambios HTTP para módulos.
* **ResourceRepositoryShare**: especifica un servidor de incorporación de cambios SMB para módulos.
* **ReportServerWeb**: especifica un servidor de incorporación de cambios HTTP al que se envían informes.
* **PartialConfiguration**: especifica configuraciones parciales.

## Configuración básica

Aparte de especificar servidores de incorporación de cambios y configuraciones parciales, todas las propiedades del LCM se configuran en un bloque **Settings**. Las propiedades siguientes están disponibles en un bloque **Settings**.

|  Propiedad  |  Tipo  |  Descripción   | 
|----------- |------- |--------------- | 
| ConfigurationModeFrequencyMins| UInt32| La frecuencia, en minutos, con que se comprueba y aplica la configuración actual. Esta propiedad se omite si la propiedad ConfigurationMode se establece en ApplyOnly. El valor predeterminado es 15. <br> __Nota__: O bien el valor de esta propiedad debe ser un múltiplo del valor de la propiedad __RefreshFrequencyMins__, o bien el valor de la propiedad __RefreshFrequencyMins__ debe ser un múltiplo del valor de esta propiedad.| 
| RebootNodeIfNeeded| bool| Establezca esta propiedad en __$true__ para reiniciar automáticamente el nodo después de aplicar una configuración que requiera un reinicio. De lo contrario, tendrá que reiniciar manualmente el nodo de configuración que lo requiera. El valor predeterminado es __$false__.| 
| ConfigurationMode| cadena | Especifica la forma en que el LCM aplica realmente la configuración a los nodos de destino. Los valores posibles son __"ApplyOnly"__, __"ApplyandMonitior"(default)__ y __"ApplyandAutoCorrect"__. <ul><li>__"ApplyOnly"__: DSC aplica la configuración y no hace nada más, a menos que se inserte una nueva configuración en el nodo de destino o se extraiga una nueva configuración de un servidor. Después de la aplicación inicial de una nueva configuración, DSC no comprueba si hay un desplazamiento con respecto a un estado configurado previamente.</li><li> __ApplyAndMonitor__: este es el valor predeterminado. El LCM aplica las nuevas configuraciones. Después de la aplicación inicial de una nueva configuración, si el nodo de destino se desplaza del estado deseado, DSC notifica la discrepancia en los registros.</li><li>__ApplyAndAutoCorrect__: DSC aplica cualquier configuración nueva. Después de la aplicación inicial de una nueva configuración, si el nodo de destino se desplaza del estado deseado, DSC notifica la discrepancia en los registros y después vuelve a aplicar la configuración actual.</li></ul>| 
| ActionAfterReboot| cadena| Especifica lo que ocurre tras un reinicio durante la aplicación de una configuración. Los valores posibles son __"ContinueConfiguration(default)"__ y __"StopConfiguration"__. <ul><li> __ContinueConfiguration__: continúe aplicando la configuración actual después de reiniciar el equipo.</li><li>__StopConfiguration__: detenga la configuración actual después de reiniciar el equipo.</li></ul>| 
| RefreshMode| cadena| Especifica cómo obtiene el LCM las configuraciones. Los valores posibles son __"Disabled"__, __"Push(default)"__ y __"Pull"__. <ul><li>__Disabled__: las configuraciones DSC se deshabilitan para este nodo.</li><li> __Push__: las configuraciones se inician con una llamada al cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx). La configuración se aplica inmediatamente al nodo. Este es el valor predeterminado.</li><li>__Pull:__ el nodo se configura para comprobar con regularidad si existen configuraciones en un servidor de incorporación de cambios. Si esta propiedad se establece en __Pull__, se debe especificar un servidor de extracción en un bloque __ConfigurationRepositoryWeb__ o __ConfigurationRepositoryShare__. Para más información sobre los servidores de incorporación de cambios, consulte [Configuración de un servidor de incorporación de cambios de DSC](pullServer.md).</li></ul>| 
| CertificateID| cadena| Un GUID que especifique un certificado utilizado para proteger las credenciales para acceder a la configuración. Para más información, consulte [Want to secure credentials in Windows PowerShell Desired State Configuration?](http://blogs.msdn.com/b/powershell/archive/2014/01/31/want-to-secure-credentials-in-windows-powershell-desired-state-configuration.aspx) (¿Quiere proteger las credenciales de configuración de estado deseado de Windows PowerShell?).| 
| ConfigurationID| cadena| Un GUID que identifica el archivo de configuración que se obtendrá de un servidor de extracción en el modo de extracción. El nodo extraerá las configuraciones del servidor de extracción si el nombre del MOF de configuración es ConfigurationID.mof.<br> __Nota:__ Si establece esta propiedad, el registro del nodo con un servidor de extracción mediante __RegistrationKey__ no funcionará. Para más información, consulte [Configuración de un cliente de incorporación de cambios con nombres de configuración](pullClientConfigNames.md).| 
| RefreshFrequencyMins| Uint32| El intervalo de tiempo, en minutos, que emplea el LCM para comprobar un servidor de extracción en busca de configuraciones actualizadas. Este valor se omite si el LCM no está configurado en el modo de extracción. El valor predeterminado es 30.<br> __Nota:__ O bien el valor de esta propiedad debe ser un múltiplo del valor de la propiedad __ConfigurationModeFrequencyMins__, o bien el valor de la propiedad __ConfigurationModeFrequencyMins__ debe ser un múltiplo del valor de esta propiedad.| 
| AllowModuleOverwrite| bool| __$TRUE__ si se permite que las nuevas configuraciones descargadas desde el servidor de configuración sobrescriban las antiguas en el nodo de destino. En caso contrario, $FALSE.| 
| DebugMode| cadena| Los valores posibles son __None(Default)__, __ForceModuleImport__ y __All__. <ul><li>Establézcala en __None__ para utilizar los recursos almacenados en caché. Este es el valor predeterminado y debe utilizarse en escenarios de producción.</li><li>Si se establece en __ForceModuleImport__, provocará que el LCM vuelva a cargar los módulos de recursos de DSC, incluso aunque se hayan cargado y almacenado en caché previamente. Esto afecta al rendimiento de las operaciones de DSC, ya que cada módulo se recarga cuando se usa. Normalmente, este valor se usaría durante la depuración de un recurso.</li><li>En esta versión, __All __ es lo mismo que __ForceModuleImport__.</li></ul> |
| ConfigurationDownloadManagers| CimInstance[]| Obsoleto. Utilice bloques __ConfigurationRepositoryWeb__ y __ConfigurationRepositoryShare__ para definir servidores de incorporación de cambios de configuración.| 
| ResourceModuleManagers| CimInstance[]| Obsoleto. Utilice bloques __ResourceRepositoryWeb__ y __ResourceRepositoryShare__ para definir servidores de incorporación de cambios de recursos.| 
| ReportManagers| CimInstance[]| Obsoleto. Utilice bloques __ReportServerWeb__ para definir servidores de incorporación de cambios de informes.| 
| PartialConfigurations| CimInstance| Sin implementar. No usar.| 
| StatusRetentionTimeInDays | UInt32| El número de días que el LCM mantiene el estado de la configuración actual.| 

## Servidores de extracción

Un servidor de extracción es un servicio web OData o un recurso compartido SMB que se utiliza como una ubicación central para archivos de DSC. La configuración de LCM admite la definición de los siguientes tipos de servidores de extracción:

* **Servidor de configuración**: un repositorio para las configuraciones DSC. Defina servidores de configuración mediante el uso de bloques **ConfigurationRepositoryWeb** (para servidores basados en web) y **ConfigurationRepositoryShare** (para servidores basados en SMB).
* Servidor de recursos: un repositorio para recursos de DSC, empaquetado como módulos de PowerShell. Defina servidores de recursos mediante el uso de bloques **ResourceRepositoryWeb** (para servidores basados en web) y **ResourceRepositoryShare** (para servidores basados en SMB).
* Servidor de informes: un servicio al que DSC envía datos de informes. Defina servidores de informes mediante bloques **ReportServerWeb**. Un servidor de informes debe ser un servicio web.

Para obtener más información sobre la configuración y utilización de servidores de incorporación de cambios, consulte [Configuración de un servidor de incorporación de cambios de DSC](pullServer.md).

## Bloques del servidor de configuración

Para definir un servidor de configuración basado en web, cree un bloque **ConfigurationRepositoryWeb**. Un bloque **ConfigurationRepositoryWeb** define las siguientes propiedades.

|Propiedad|Tipo|Descripción|
|---|---|---| 
|AllowUnsecureConnection|bool|Establézcala en **$TRUE** para permitir conexiones desde el nodo al servidor sin autenticación. Establézcala en **$FALSE** para que se requiera autenticación.|
|CertificateID|cadena|Un GUID que representa el certificado utilizado para autenticar el servidor.|
|ConfigurationNames|String[]|Una matriz de nombres de configuraciones que el nodo de destino extraerá. Solo se utilizan si el nodo se registra con el servidor de incorporación de cambios mediante un elemento **RegistrationKey**. Para más información, consulte [Configuración de un cliente de incorporación de cambios con nombres de configuración](pullClientConfigNames.md).|
|RegistrationKey|cadena|Un GUID que registra el nodo con el servidor de extracción. Para más información, consulte [Configuración de un cliente de incorporación de cambios con nombres de configuración](pullClientConfigNames.md).|
|ServerURL|cadena|La dirección URL del servidor de configuración.|

Para definir un servidor de configuración basado en SMB, cree un bloque **ConfigurationRepositoryShare**. Un bloque **ConfigurationRepositoryShare** define las siguientes propiedades.

|Propiedad|Tipo|Descripción|
|---|---|---|
|Credential|MSFT_Credential|La credencial usada para autenticarse en el recurso compartido SMB.|
|SourcePath|cadena|La ruta de acceso del recurso compartido SMB.|

## Bloques del servidor de recursos

Para definir un servidor de recursos basado en web, cree un bloque **ResourceRepositoryWeb**. Un bloque **ResourceRepositoryWeb** define las siguientes propiedades.

|Propiedad|Tipo|Descripción|
|---|---|---|
|AllowUnsecureConnection|bool|Establézcala en **$TRUE** para permitir conexiones desde el nodo al servidor sin autenticación. Establézcala en **$FALSE** para que se requiera autenticación.|
|CertificateID|cadena|Un GUID que representa el certificado utilizado para autenticar el servidor.|
|RegistrationKey|cadena|Un GUID que identifica el nodo para el servidor de extracción. Para más información, consulte Cómo registrar un nodo con un servidor de extracción de DSC.|
|ServerURL|cadena|La dirección URL del servidor de configuración.|
 
Para definir un servidor de recursos basado en SMB, cree un bloque **ResourceRepositoryShare**. **ResourceRepositoryShare** define las siguientes propiedades.

|Propiedad|Tipo|Descripción|
|---|---|---|
|Credential|MSFT_Credential|La credencial usada para autenticarse en el recurso compartido SMB.|
|SourcePath|cadena|La ruta de acceso del recurso compartido SMB.|

## Bloques del servidor de informes

Un servidor de informes debe ser un servicio web OData. Para definir un servidor de informes, cree un bloque **ReportServerWeb**. **ReportServerWeb** define las siguientes propiedades.

|Propiedad|Tipo|Descripción|
|---|---|---| 
|AllowUnsecureConnection|bool|Establézcala en **$TRUE** para permitir conexiones desde el nodo al servidor sin autenticación. Establézcala en **$FALSE** para que se requiera autenticación.|
|CertificateID|cadena|Un GUID que representa el certificado utilizado para autenticar el servidor.|
|RegistrationKey|cadena|Un GUID que identifica el nodo para el servidor de extracción. Para más información, consulte Cómo registrar un nodo con un servidor de extracción de DSC.|
|ServerURL|cadena|La dirección URL del servidor de configuración.|

## Configuraciones parciales

Para definir una configuración parcial, cree un bloque **PartialConfiguration**. Para más información sobre configuraciones parciales, consulte [Configuraciones parciales de DSC](partialConfigs.md). **PartialConfiguration** define las siguientes propiedades.

|Propiedad|Tipo|Descripción|
|---|---|---| 
|ConfigurationSource|string[]|Una matriz de nombres de servidores de configuración, definidos previamente en bloques **ConfiguratoinRepositoryWeb** y **ConfigurationRepositoryShare**, desde donde se extrae la configuración parcial.|
|DependsOn|string{}|Una lista de nombres de otras configuraciones que se deben completar antes de que se aplique esta configuración parcial.|
|Descripción|cadena|Texto utilizado para describir la configuración parcial.|
|ExclusiveResources|string[]|Una matriz de recursos exclusivos de esta configuración parcial.|
|RefreshMode|cadena|Especifica cómo obtiene el LCM esta configuración parcial. Los valores posibles son __"Disabled"__, __"Push(default)"__ y __"Pull"__. <ul><li>__Disabled__: esta configuración parcial está deshabilitada.</li><li> __Push__: la configuración parcial se inserta en el nodo con una llamada al cmdlet [Publish-DscConfiguration](https://technet.microsoft.com/en-us/library/mt517875.aspx). Cuando todas las configuraciones parciales del nodo se han insertado o extraído de un servidor, es posible iniciar la configuración con una llamada a `Start-DscConfiguration –UseExisting`. Este es el valor predeterminado.</li><li>__Pull__: el nodo se configura para comprobar con regularidad si existe una configuración parcial en un servidor de extracción. Si esta propiedad se establece en __Pull__, debe especificar un servidor de extracción en una propiedad __ConfigurationSource__. Para más información sobre los servidores de incorporación de cambios, consulte [Configuración de un servidor de incorporación de cambios de DSC](pullServer.md).</li></ul>|
|ResourceModuleSource|string[]|Una matriz de los nombres de los servidores de recursos desde los que se descargarán los recursos necesarios para esta configuración parcial. Estos nombres deben hacer referencia a los servidores de recursos definidos previamente en bloques **ResourceRepositoryWeb** y **ResourceRepositoryShare**.|

## Consulte también 

### Conceptos
[Información general sobre la configuración de estado deseado de Windows PowerShell](overview.md)
 
[Configuración de un servidor de incorporación de cambios de DSC](pullServer.md) 

[Administrador de configuración local de la configuración de estado deseado de Windows PowerShell 4.0](metaConfig4.md) 

### Otros recursos
[Set-DscLocalConfigurationManager](https://technet.microsoft.com/en-us/library/dn521621.aspx) 

[Configuración de un cliente de incorporación de cambios con nombres de configuración](pullClientConfigNames.md) 



<!--HONumber=Jun16_HO3-->


