---
title:  Método StopConfiguration de la clase MSFT_DSCLocalConfigurationManager
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Método StopConfiguration de la clase MSFT_DSCLocalConfigurationManager

Detiene el cambio de configuración que está en curso.

Sintaxis
------

```mof
uint32 StopConfiguration(
  [in] boolean force
);
```

Parámetros
----------

*force* \[in\]  
**true** para forzar la configuración que se detendrá.

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


