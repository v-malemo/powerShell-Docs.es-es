---
title: "Método RollBack de la clase MSFT_DSCLocalConfigurationManager"
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: c915ebd021ed20209bc491505d45cff2ac89f21d
ms.openlocfilehash: 771a9c7b50aba26f89dbf6b24eb3df67bafeac0a

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


 

 






<!--HONumber=Jun16_HO4-->


