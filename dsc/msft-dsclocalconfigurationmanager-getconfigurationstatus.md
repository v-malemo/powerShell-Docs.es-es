---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Obtiene el historial de estado de la configuración.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_getconfigurationstatus'
MSHAttr: 'PreferredLib:/library'
title: 'Método GetConfigurationStatus de la clase MSFT_DSCLocalConfigurationManager'
---

# Método GetConfigurationStatus de la clase MSFT_DSCLocalConfigurationManager

Obtiene el historial de estado de la configuración.

Sintaxis
------

```mof
uint32 GetConfigurationStatus(
  [in]  boolean                     All,
  [out] MSFT_DSCConfigurationStatus configurationStatus[]
);
```

Parámetros
----------

*All* \[in\]  
**true** si este método debe devolver información acerca de toda la configuración que se ejecuta en el equipo, incluida
la aplicación de la configuración y la comprobación de coherencia.

*configurationStatus* \[out\]  
En la devolución, contiene una instancia incrustada de la clase **MSFT_DSCConfigurationStatus** que define la configuración.

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


