---
title:  Método GetMetaConfiguration de la clase MSFT_DSCLocalConfigurationManager
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
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


 

 





<!--HONumber=May16_HO3-->


