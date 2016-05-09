---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Detiene la configuración en curso.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_stopconfiguration'
MSHAttr: 'PreferredLib:/library'
title: 'Método StopConfiguration de la clase MSFT_DSCLocalConfigurationManager'
---

# Método StopConfiguration de la clase MSFT_DSCLocalConfigurationManager

Detiene el cambio de configuración que está en curso.

Sintaxis
------

```mof
uint32 StopConfiguration(
  [in] boolean force
);
```

Parámetros
----------

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


