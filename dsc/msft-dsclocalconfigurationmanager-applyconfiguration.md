---
title: "Método ApplyConfiguration de la clase MSFT_DSCLocalConfigurationManager"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 919438862ca9786447b690d2db10e905da0a7c42
ms.openlocfilehash: 6f9c6a8851732574ac72bc4f3a3db1a73fbbecf2

---

# Método ApplyConfiguration de la clase MSFT_DSCLocalConfigurationManager

Usa el agente de configuración para aplicar la configuración que está pendiente. 

Si no hay ninguna configuración pendiente, este método vuelve a aplicar la configuración actual.


## Sintaxis
------

```mof
uint32 ApplyConfiguration(
  [in] boolean force
);
```

## Parámetros
----------

*force* \[in\]  
Si es **true**, se vuelve a aplicar la configuración actual, incluso si hay una configuración pendiente.

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


