---
title: Método ApplyConfiguration de la clase MSFT_DSCLocalConfigurationManager 
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
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

 

 





<!--HONumber=May16_HO3-->


