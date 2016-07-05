# Las frecuencias de RefreshMode y ConfigurationMode no tienen que ser múltiplos una de la otra

En la versión anterior de DSC, LCM trataría `RefreshFrequencyMins` y `ConfigurationModeFrequencyMins` como múltiplos una de la otra. En WMF 5.0 RTM, estas propiedades se procesan con independencia entre sí. 

Para más información, consulte [Configuración del administrador de configuración local](../dsc/metaConfig.md).

<!--HONumber=Jun16_HO4-->


