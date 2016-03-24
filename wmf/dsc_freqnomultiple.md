# Las frecuencias de RefreshMode y ConfigurationMode no tienen que ser múltiplos una de la otra

En la versión anterior de DSC, LCM trataría `RefreshFrequencyMins` y `ConfigurationModeFrequencyMins` como múltiplos una de la otra, tal como se muestra [aquí](http://blogs.msdn.com/b/powershell/archive/2013/12/09/understanding-meta-configuration-in-windows-powershell-desired-state-configuration.aspx) en el blog. En WMF 5.0 RTM, estas propiedades se procesan con independencia entre sí. Las tablas siguientes muestran este comportamiento:

Comportamiento en el modo de **extracción**: 

|                                  |**Valor en la metaconfiguración**|**Valor después de aplicar la metaconfiguración**|**Frecuencia con la que se produce la extracción (en minutos)**|**Frecuencia de aplicación de la configuración (en minutos)**|
|----------------------------------|-------------------------------|---------------------------------------------|------------------------------------|------------------------------------------------|
|**ConfigurationModeFrequencyMins**|70                             |70                                           |                                    |70                                              |
|**RefreshFrequencyMins**          |40                             |40                                           |40                                  |                                                |

Comportamiento en el modo de **inserción**:

|                                  |**Valor en la metaconfiguración**|**Valor después de aplicar la metaconfiguración**|**Frecuencia de aplicación de la configuración (en minutos)**|
|----------------------------------|-------------------------------|---------------------------------------------|------------------------------------------------|
|**ConfigurationModeFrequencyMins**|70                             |70                                           |70                                              |
|**RefreshFrequencyMins**          |40                             |40                                           |                                                |
<!--HONumber=Mar16_HO2-->
