---
title: "Método GetMetaConfiguration de la clase MSFT_DSCLocalConfigurationManager"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 919438862ca9786447b690d2db10e905da0a7c42
ms.openlocfilehash: 4662bfed62fce47be7d42a083ad5a7be801e6ff1

---


# Método GetMetaConfiguration de la clase MSFT_DSCLocalConfigurationManager

Obtiene la configuración del Administrador de configuración local que se usa para controlar el agente de configuración.

Sintaxis
------

```mof
uint32 GetMetaConfiguration(
  [out] MSFT_DSCMetaConfiguration MetaConfiguration
);
```

Parámetros
----------

*MetaConfiguration* \[out\]  
En la devolución, contiene una instancia incrustada de la clase **MSFT_DSCMetaConfiguration** que define la configuración.

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


