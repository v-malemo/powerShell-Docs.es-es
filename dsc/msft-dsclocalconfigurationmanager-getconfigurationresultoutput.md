---
title: "Método GetConfigurationResultOutput de la clase MSFT_DSCLocalConfigurationManager"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: c915ebd021ed20209bc491505d45cff2ac89f21d
ms.openlocfilehash: 8f13964dfbbe1cd827c58232a35d1cbacddeed1b

---

# Método GetConfigurationResultOutput de la clase MSFT_DSCLocalConfigurationManager

Obtiene la salida del agente de configuración asociada con un trabajo específico.

Sintaxis
------

```mof
uint32 GetConfigurationResultOutput(
  [in]  string                      jobId,
  [in]  uint8                       resumeOutputBookmark[],
  [out] MSFT_DSCConfigurationOutput output[]
);
```

Parámetros
----------

*jobId* \[in\]  
Identificador del trabajo del que quiere obtener datos de salida.

*resumeOutputBookmark* \[in\]  
Especifica que la salida debe tener una continuación de un marcador anterior.

*output* \[out\]  
Salida del trabajo especificado.

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


