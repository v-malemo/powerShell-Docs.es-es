---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Revierte la configuración anterior.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_rollback'
MSHAttr: 'PreferredLib:/library'
title: 'Método RollBack de la clase MSFT_DSCLocalConfigurationManager'
---

# Método RollBack de la clase MSFT_DSCLocalConfigurationManager

Revierte la configuración a una versión anterior.

Sintaxis
------

```mof
uint32 RollBack(
  [in] uint8 configurationNumber
);
```

Parámetros
----------

*configurationNumber* \[in\]  
Especifica la configuración solicitada. 

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


