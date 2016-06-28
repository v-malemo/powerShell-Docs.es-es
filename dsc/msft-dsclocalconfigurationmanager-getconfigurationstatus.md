---
title: "Método GetConfigurationStatus de la clase MSFT_DSCLocalConfigurationManager"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: c915ebd021ed20209bc491505d45cff2ac89f21d
ms.openlocfilehash: b430e98c7ec287c0efcf2c2e2736253797242904

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
**true** si este método debe devolver información sobre toda la configuración que se ejecuta en el equipo, incluidas la aplicación de configuración y la comprobación de coherencia.

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


 

 






<!--HONumber=Jun16_HO4-->


