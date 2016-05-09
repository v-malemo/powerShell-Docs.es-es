

# Clase MSFT_DSCLocalConfigurationManager

Administrador de configuración Local (LCM) que controla los estados de los archivos de configuración y usa al agente de configuración para aplicar las configuraciones.

La siguiente sintaxis se simplifica desde el código de Managed Object Format (MOF) e incluye todas las propiedades heredadas.

## Sintaxis
------

``` syntax
[ClassVersion("1.0.0"), dynamic, provider("dsccore"), AMENDMENT]
class MSFT_DSCLocalConfigurationManager
{
};
```

## Miembros
-------

La clase **MSFT_DSCLocalConfigurationManager** tiene los miembros siguientes:

-   [Métodos][]

### Métodos

La clase **MSFT_DSCLocalConfigurationManager** tiene estos métodos.

|Método |Descripción |
|:--- |:---|
| [ApplyConfiguration](msft-dsclocalconfigurationmanager-applyconfiguration.md)| Usa el agente de configuración para aplicar la configuración que está pendiente.| 
| [DisableDebugConfiguration](msft-dsclocalconfigurationmanager-disabledebugconfiguration.md)| Deshabilita la depuración de recursos de DSC.| 
| [EnableDebugConfiguration](msft-dsclocalconfigurationmanager-enabledebugconfiguration.md)| Habilita la depuración de recursos de DSC.| 
| [GetConfiguration](msft-dsclocalconfigurationmanager-getconfiguration.md)| Envía el documento de configuración al nodo administrado y usa el método **Get** del agente de configuración para aplicar la configuración.| 
| [GetConfigurationResultOutput](msft-dsclocalconfigurationmanager-getconfigurationresultoutput.md)| Obtiene la salida del agente de configuración relacionada con un trabajo específico.| 
| [GetConfigurationStatus](msft-dsclocalconfigurationmanager-getconfigurationstatus.md)| Obtiene el historial de estado de la configuración.| 
| [GetMetaConfiguration](msft-dsclocalconfigurationmanager-getmetaconfiguration.md)| Obtiene la configuración del LCM que se usa para controlar el agente de configuración.| 
| [PerformRequiredConfigurationChecks](msft-dsclocalconfigurationmanager-performrequiredconfigurationchecks.md)| Inicia la comprobación de coherencia.| 
| [RemoveConfiguration](msft-dsclocalconfigurationmanager-removeconfiguration.md)| Quita los archivos de configuración.| 
| [ResourceGet](msft-dsclocalconfigurationmanager-resourceget.md)| Llama directamente al método **Get** de un recurso de DSC.| 
| [ResourceSet](msft-dsclocalconfigurationmanager-resourceset.md)| Llama directamente al método **Set** de un recurso de DSC.| 
| [ResourceTest](msft-dsclocalconfigurationmanager-resourcetest.md)| Llama directamente al método **Test** de un recurso de DSC.| 
| [RollBack](msft-dsclocalconfigurationmanager-rollback.md)| Revierte una configuración anterior.| 
| [SendConfiguration](msft-dsclocalconfigurationmanager-sendconfiguration.md)| Envía el documento de configuración al nodo administrado y lo guarda como cambio pendiente.| 
| [SendConfigurationApply](msft-dsclocalconfigurationmanager-sendconfigurationapply.md)| Envía el documento de configuración al nodo administrado y usa al agente de configuración para aplicar la configuración.| 
| [SendConfigurationApplyAsync](msft-dsclocalconfigurationmanager-sendconfigurationapplyasync.md)| Envía el documento de configuración al nodo administrado y empieza a usar el agente de configuración para aplicar la configuración. Usa GetConfigurationResultOutput para recuperar la salida de resultados.| 
| [SendMetaConfigurationApply](msft-dsclocalconfigurationmanager-sendmetaconfigurationapply.md)| Establece la configuración del LCM que se usa para controlar el agente de configuración.| 
| [StopConfiguration](msft-dsclocalconfigurationmanager-stopconfiguration.md)| Detiene la configuración que está en curso.| 
| [TestConfiguration](msft-dsclocalconfigurationmanager-testconfiguration.md)| Envía el documento de configuración al nodo administrado y prueba la configuración actual frente al documento.| 



 

## Requisitos
------------
>**MOF:** DscCore.mof

>**Espacio de nombres**: Root\Microsoft\Windows\DesiredStateConfiguration



 

 





<!--HONumber=Apr16_HO2-->


