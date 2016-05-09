---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Inicia la comprobación de coherencia.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_performrequiredconfigurationchecks'
MSHAttr: 'PreferredLib:/library'
title: 'Método PerformRequiredConfigurationChecks de la clase MSFT_DSCLocalConfigurationManager'
---

# Método PerformRequiredConfigurationChecks de la clase MSFT_DSCLocalConfigurationManager

Inicia una comprobación de coherencia mediante el Programador de tareas.

Sintaxis
------

```mof
uint32 PerformRequiredConfigurationChecks(
  [in] uint32 Flags
);
```

Parámetros
----------

*Flags* \[in\]  
Máscara de bits que especifica el tipo de comprobación de coherencia que se ejecutará. Los valores siguientes son válidos y se pueden combinar mediante el uso de una operación **O** bit a bit:

|Value |Descripción |
|:--- |:---|
|**1** | Comprobación de coherencia normal. |
|**2** | Continuación de una comprobación de coherencia después de reiniciar el equipo. Este valor no debe combinarse con otros valores. |
|**4** | Debe extraerse la configuración del servidor de extracción especificado en la metaconfiguración del nodo. Este valor siempre debe combinarse con **1**, para un valor de **5**. |
|**8** | Envía el estado al servidor de informes. |

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


