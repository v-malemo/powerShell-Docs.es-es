---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Envía el documento de configuración al nodo administrado y la prueba frente a la configuración actual.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_testconfiguration'
MSHAttr: 'PreferredLib:/library'
title: 'Método TestConfiguration de la clase MSFT_DSCLocalConfigurationManager'
---

# Método TestConfiguration de la clase MSFT_DSCLocalConfigurationManager

Envía el documento de configuración al nodo administrado y prueba la configuración actual frente al documento.

Sintaxis
------

```mof
uint32 TestConfiguration(
  [in]  uint8                          configurationData[],
  [out] boolean                        InDesiredState,
  [out] MSFT_ResourceInDesiredState    ResourcesInDesiredState[],
  [out] MSFT_ResourceNotInDesiredState ResourcesNotInDesiredState[]
);
```

Parámetros
----------

*configurationData* \[in\]  
Datos del entorno para la configuración.

*InDesiredState* \[out\]  
En la devolución, especifica si el nodo administrado está en el estado especificado por el documento de configuración.

*ResourcesInDesiredState* \[out\]  
En la devolución, contiene una instancia incrustada de la clase **MSFT_ResourceInDesiredState**, que especifica los recursos que están en el estado deseado.

*ResourcesNotInDesiredState* \[out\]  
En la devolución, contiene una instancia incrustada de la clase **MSFT_ResourceNotInDesiredState**, que especifica los recursos que no están en el estado deseado.

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


