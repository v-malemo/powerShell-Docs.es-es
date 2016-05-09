---
DCS.appliesToProduct: 'WindowsServer\_Dev'
Description: 'Ejecuta Set en un proveedor directamente.'
MS-HAID: 'cimwin32a.MSFT_DSCLocalConfigurationManager\_resourceset'
MSHAttr: 'PreferredLib:/library'
title: 'Método ResourceSet de la clase MSFT_DSCLocalConfigurationManager'
---

# Método ResourceSet de la clase MSFT_DSCLocalConfigurationManager

Llama directamente al método **Set** de un recurso de DSC.

Sintaxis
------

```mof
uint32 ResourceSet(
  [in]  string  ResourceType,
  [in]  string  ModuleName,
  [in]  uint8   resourceProperty[],
  [out] boolean RebootRequired
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

*RebootRequired* \[out\]  
En la devolución, esta propiedad se establece en **true** si el nodo de destino debe reiniciarse.

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


