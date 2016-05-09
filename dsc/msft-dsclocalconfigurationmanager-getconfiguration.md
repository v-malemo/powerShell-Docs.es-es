---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Envía el documento de configuración al nodo administrado y usa al agente de configuración para aplicar la configuración mediante el método Get.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_getconfiguration'
MSHAttr: 'PreferredLib:/library'
title: 'Método GetConfiguration de la clase MSFT_DSCLocalConfigurationManager'
---

# Método GetConfiguration de la clase MSFT_DSCLocalConfigurationManager

Envía el documento de configuración al nodo administrado y usa el método **Get** del agente de configuración para aplicar la configuración.

Sintaxis
------

```mof
uint32 GetConfiguration(
  [in]  uint8            configurationData[],
  [out] OMI_BaseResource configurations[]
);
```

Parámetros
----------

*configurationData* \[in\]  
Especifica los datos de configuración que se enviarán.

*configurations* \[out\]  
En la devolución, contiene una instancia incrustada de las configuraciones.

## Valor devuelto
------------

Devuelve cero si se ejecuta correctamente; de lo contrario, devuelve un código de error.

## Observaciones

Se trata de un método estático.

## Requisitos
------------
>**MOF:** DscCore.mof

>**Espacio de nombres**: Root\Microsoft\Windows\DesiredStateConfiguration


## Vea también


[**MSFT_DSCLocalConfigurationManager**](msft-dsclocalconfigurationmanager.md)
 

 





<!--HONumber=Apr16_HO2-->


