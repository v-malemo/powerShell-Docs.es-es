---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Envía el documento de configuración al nodo administrado y empieza a usar el agente de configuración para aplicar la configuración. Usa GetConfigurationResultOutput para recuperar la salida de resultados.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_sendconfigurationapplyasync'
MSHAttr: 'PreferredLib:/library'
title: 'Método SendConfigurationApplyAsync de la clase MSFT_DSCLocalConfigurationManager'
---

# Método SendConfigurationApplyAsync de la clase MSFT_DSCLocalConfigurationManager

Envía el documento de configuración de forma asincrónica al nodo administrado y usa el agente de configuración para aplicar la configuración.

Sintaxis
------

```mof
uint32 SendConfigurationApplyAsync(
  [in] uint8   ConfigurationData[],
  [in] boolean force,
  [in] string  jobId
);
```

Parámetros
----------

*ConfigurationData* \[in\]  
Datos del entorno para la configuración.

*force* \[in\]  
**true** para forzar la configuración que se detendrá.

*jobId* \[in\]  
Identificador del trabajo al que se enviará la configuración.

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


