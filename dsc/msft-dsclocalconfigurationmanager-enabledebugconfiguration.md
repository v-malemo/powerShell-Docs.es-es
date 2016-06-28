---
title: "Método EnableDebugConfiguration de la clase MSFT_DSCLocalConfigurationManager"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 919438862ca9786447b690d2db10e905da0a7c42
ms.openlocfilehash: f74e9941180c00a1aae1bd1d7b48fa4de0c8790d

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
 

 






<!--HONumber=Jun16_HO4-->


