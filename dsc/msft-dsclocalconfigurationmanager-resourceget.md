---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Ejecuta Get en un proveedor directamente.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_resourceget'
MSHAttr: 'PreferredLib:/library'
title: 'Método ResourceGet de la clase MSFT_DSCLocalConfigurationManager'
---

# Método ResourceGet de la clase MSFT_DSCLocalConfigurationManager

Llama directamente al método **Get** de un recurso de DSC.

Sintaxis
------

```mof
uint32 ResourceGet(
  [in]  string           ResourceType,
  [in]  string           ModuleName,
  [in]  uint8            resourceProperty[],
  [out] OMI_BaseResource configurations
);
```

Parámetros
----------

*ResourceType* \[in\]  
Nombre del recurso al que se llamará.

*ModuleName* \[in\]  
Nombre del módulo que contiene el recurso al que se llamará.

*resourceProperty* \[in\]  
Especifica el nombre de la propiedad del recurso y su valor en una tabla hash como clave y valor, respectivamente. Use la
cmdlet [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) para descubrir las propiedades del recurso y sus tipos.

*configurations* \[out\]  
En la devolución, contiene una instancia incrustada de las configuraciones.

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


 

 





<!--HONumber=Apr16_HO2-->


