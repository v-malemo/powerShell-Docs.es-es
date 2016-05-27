---
title:  Método EnableDebugConfiguration de la clase MSFT_DSCLocalConfigurationManager
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---


# Método EnableDebugConfiguration de la clase MSFT_DSCLocalConfigurationManager

Habilita la depuración de recursos de DSC.

Sintaxis
------

```mof
uint32 EnableDebugConfiguration(
  [in] boolean BreakAll
);
```

Parámetros
----------

*BreakAll* \[in\]  
Establece un punto de interrupción en cada línea del script de recursos.

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
 

 





<!--HONumber=May16_HO3-->


