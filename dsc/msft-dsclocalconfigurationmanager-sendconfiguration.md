---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Envía el documento de configuración al nodo administrado y lo guarda como pendiente.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_sendconfiguration'
MSHAttr: 'PreferredLib:/library'
title: 'Método SendConfiguration de la clase MSFT_DSCLocalConfigurationManager'
---

# Método SendConfiguration de la clase MSFT_DSCLocalConfigurationManager

Envía el documento de configuración al nodo administrado y lo guarda como cambio pendiente.

Sintaxis
------

```mof
uint32 SendConfiguration(
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


