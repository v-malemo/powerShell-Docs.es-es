---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Envía el documento de configuración al nodo administrado y usa al agente de configuración para aplicar la configuración.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_sendconfigurationapply'
MSHAttr: 'PreferredLib:/library'
title: 'Método SendConfigurationApply de la clase MSFT_DSCLocalConfigurationManager'
---

# Método SendConfigurationApply de la clase MSFT_DSCLocalConfigurationManager

Envía el documento de configuración al nodo administrado y usa al agente de configuración para aplicar la configuración.

Sintaxis
------

```mof
uint32 SendConfigurationApply(
  [in] uint8   ConfigurationData[],
  [in] boolean force
);
```

Parámetros
----------

*ConfigurationData* \[in\]  
Datos del entorno para la configuración.

*force* \[in\]  
**true** para forzar la configuración que se detendrá.

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


