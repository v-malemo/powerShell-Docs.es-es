---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Quita los archivos de configuración.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_removeconfiguration'
MSHAttr: 'PreferredLib:/library'
title: 'Método RemoveConfiguration de la clase MSFT_DSCLocalConfigurationManager'
---

# Método RemoveConfiguration de la clase MSFT_DSCLocalConfigurationManager

Quita los archivos de configuración.

Sintaxis
------

```mof
uint32 RemoveConfiguration(
  [in] uint32  Stage,
  [in] boolean Force
);
```

Parámetros
----------

*Stage* \[in\]  
Especifica qué documento de configuración quiere quitar. Los valores siguientes son válidos:

|Value |Descripción |
|:--- |:---|
|**1** | Documento de configuración **Current** (current.mof). |
|**2** | Documento de configuración **Pending** (pending.mof).  |
|**4** | Documento de configuración **Previous** (previous.mof). |

*Force* \[in\]  
**true** para forzar la eliminación de la configuración.

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


