---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Recupera la salida del agente de configuración relacionada con un trabajo específico.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_getconfigurationresultoutput'
MSHAttr: 'PreferredLib:/library'
title: 'Método GetConfigurationResultOutput de la clase MSFT_DSCLocalConfigurationManager'
---

# Método GetConfigurationResultOutput de la clase MSFT_DSCLocalConfigurationManager

Obtiene la salida del agente de configuración asociada con un trabajo específico.

Sintaxis
------

```mof
uint32 GetConfigurationResultOutput(
  [in]  string                      jobId,
  [in]  uint8                       resumeOutputBookmark[],
  [out] MSFT_DSCConfigurationOutput output[]
);
```

Parámetros
----------

*jobId* \[in\]  
Identificador del trabajo del que quiere obtener datos de salida.

*resumeOutputBookmark* \[in\]  
Especifica que la salida debe tener una continuación de un marcador anterior.

*output* \[out\]  
Salida del trabajo especificado.

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


