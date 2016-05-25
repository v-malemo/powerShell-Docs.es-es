---
title:  Obtener información de ayuda detallada
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  6fb4daf7-8607-4a3e-b692-f77631adc1b9
---

# Obtener información de ayuda detallada
Windows PowerShell incluye temas de ayuda detallados que explican conceptos de Windows PowerShell y el lenguaje de Windows PowerShell. También hay temas de Ayuda para cada cmdlet y proveedor y temas de Ayuda para muchas funciones y scripts.

Puede ver estos temas de Ayuda en el símbolo del sistema o ver las versiones actualizadas más recientemente de estos temas en la biblioteca de Microsoft Technet. Muchos programas que hospedan Windows PowerShell, como el Entorno de scripting integrado de Windows PowerShell, proporcionan características adicionales de ayuda, como la ayuda contextual y el archivo de ayuda compilado (.chm).

## Obtener ayuda para cmdlets
Para obtener ayuda acerca de los cmdlets de Windows PowerShell, use el cmdlet [Get-Help [m2]](https://technet.microsoft.com/en-us/library/2d7fe1b4-0025-4580-a911-d81922dd6cd2). Por ejemplo, para obtener ayuda para el cmdlet [Get-ChildItem [m2]](https://technet.microsoft.com/en-us/library/4b270d63-c995-45b8-b5b4-3f8887efbfcc), escriba:

```
get-help get-childitem
```

o

```
get-childitem -?
```

También puede obtener ayuda sobre el cmdlet Get-Help. Por ejemplo:

```
get-help get-help
```

Para obtener una lista de todos los temas de Ayuda de cmdlets de la sesión, escriba:

```
get-help -category cmdlet
```

Para mostrar una página de cada tema de Ayuda cada vez, use la función **help** o su alias **man**. Por ejemplo, para mostrar la Ayuda del cmdlet Get-ChildItem, escriba:

```
man get-childitem
```

o

```
help get-childitem
```

Para mostrar información detallada acerca de un cmdlet, una función o un script, incluidas las descripciones de sus parámetros y ejemplos de su uso, emplee el parámetro *Detailed* del cmdlet Get-Help. Por ejemplo, para obtener información detallada acerca del cmdlet Get-ChildItem, escriba:

```
get-help get-childitem -detailed
```

Para mostrar todo el contenido del tema de Ayuda, use el parámetro *Full* del cmdlet Get-Help. Por ejemplo, para mostrar todo el contenido del tema de Ayuda para el cmdlet Get-ChildItem, escriba:

```
get-help get-childitem -full
```

Para obtener Ayuda detallada acerca de los parámetros de un cmdlet, use el parámetro *Parameter* del cmdlet Get-Help. Por ejemplo, para obtener Ayuda detallada de todos los parámetros del cmdlet Get-ChildItem, escriba:

```
get-help get-childitem -parameter *
```

Para mostrar solo los ejemplos de un tema de Ayuda, use el parámetro *Example* de Get-Help. Por ejemplo, para mostrar solo los ejemplos del tema de Ayuda para el cmdlet Get-ChildItem, escriba:

```
get-help get-childitem -examples
```

Para obtener información sobre cómo escribir temas de Ayuda para los cmdlets que escribe, vea el tema sobre cómo escribir ayuda de cmdlets ("How to Write Cmdlet Help") en MSDN.

## Obtener ayuda conceptual
El cmdlet Get-Help también muestra información acerca de temas conceptuales en Windows PowerShell, incluidos los temas sobre el lenguaje de Windows PowerShell. Los temas de Ayuda conceptual comienzan por el prefijo "about_", como about_line_editing. (Los nombres de temas conceptuales deben especificarse en inglés, incluso en versiones no inglesas de Windows PowerShell).

Para mostrar una lista de temas conceptuales, escriba:

```
get-help about_*
```

Para mostrar un tema de Ayuda determinado, escriba el nombre del tema, por ejemplo:

```
get-help about_command_syntax
```

Los parámetros de Get-Help, como *Detailed*, *Parameter* y *Examples*, no tienen ningún efecto en la presentación de temas de Ayuda conceptuales.

## Obtener ayuda acerca de los proveedores
El cmdlet Get-Help muestra información sobre los proveedores de Windows PowerShell. Para obtener ayuda sobre un proveedor, escriba "Get-Help" seguido del nombre del proveedor. Por ejemplo, para obtener ayuda sobre el proveedor del Registro, escriba:

```
get-help registry
```

Para obtener una lista de todos los temas de Ayuda de la sesión, escriba:

```
get-help -category provider
```

Los parámetros de Get\-Help, como *Detailed*, *Parameter* y *Examples*, no tienen ningún efecto en la presentación de temas de Ayuda de proveedor.

## Obtener ayuda acerca de scripts y funciones
Muchos scripts y funciones de Windows PowerShell tienen temas de Ayuda. Use el cmdlet Get-Help para visualizar los temas de Ayuda de scripts y funciones.

Para visualizar la Ayuda de una función, escriba "get-help" seguido del nombre de función. Por ejemplo, para obtener Ayuda para la función Disable-PSRemoting, escriba:

```
get-help disable-psremoting
```

Para visualizar la Ayuda de un script, escriba la ruta de acceso completa al archivo de script. Si el script está en una ruta de acceso que aparece en la variable de entorno Path, puede omitir la ruta de acceso del comando.

Por ejemplo, si tiene un script denominado "TestScript.ps1" en el directorio C:\PS-Test, para visualizar el tema de Ayuda del script, escriba:

```
get-help c:\ps-test\TestScript.ps1
```

Los parámetros diseñados para mostrar la Ayuda de cmdlets, como *Detailed*, *Full*, *Examples* y *Parameter*, también funcionan para la Ayuda de scripts y funciones. Sin embargo, al escribir "get-help *" para que se muestre toda la Ayuda, la Ayuda de funciones y scripts no aparece.

Para obtener información sobre cómo escribir la Ayuda de scripts y funciones, vea [about_Functions [m2]](https://technet.microsoft.com/en-us/library/61d40692-5300-4de9-a9b5-bae31815e105), [about_Scripts](https://technet.microsoft.com/en-us/library/7dc08334-dcfe-450b-b949-0554855623af) y [about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/99a81ccc-21a0-49ec-a1b3-9efe2b4c0bbf).

## Obtener ayuda en línea
Si está conectado a Internet, una de las mejores formas de obtener Ayuda es ver los temas de Ayuda en línea. Dado que los temas en línea son fáciles de actualizar, es probable que proporcionen el contenido más actualizado.

Para obtener Ayuda en línea, pruebe el parámetro *Online* del cmdlet Get-Help. El parámetro *Online* del cmdlet Get-Help funciona solo para la Ayuda de cmdlets, funciones y scripts. No se puede usar el parámetro *Online* con temas conceptuales (Acerca de) ni temas de Ayuda de proveedor. Además, dado que esta característica es opcional, no funciona con todos los temas de Ayuda de cmdlets, funciones o scripts.

Sin embargo, todos los temas de Ayuda que se suministran con Windows PowerShell, incluidos los temas de Ayuda de proveedor y los temas de Ayuda conceptuales (Acerca de), están disponibles en línea en la sección [Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=107116) de la biblioteca de Microsoft TechNet.

Para usar el parámetro *Online* del cmdlet Get-Help, emplee el siguiente formato de comando.

```
get-help <command-name> -online
```

Por ejemplo, para obtener la versión en línea del tema de Ayuda sobre el cmdlet Get-ChildItem, escriba:

```
get-help get-childitem -online
```

Si está disponible una versión en línea del tema de Ayuda, se abrirá en el explorador predeterminado.

Si se admite la Ayuda en línea para un tema de Ayuda, también puede ver la dirección de Internet (URL) del tema de Ayuda. La dirección de Internet aparece en la sección Vínculos relacionados de un tema de Ayuda.

Por ejemplo, para ver la dirección URL de la versión en línea del cmdlet Add-Computer, escriba:

```
get-help add-computer
```

A continuación, se muestra la primera línea de la sección Vínculos relacionados del tema.

```
Online version: http://go.microsoft.com/fwlink/?LinkID=135194
```

Para obtener más información sobre cómo proporcionar soporte técnico en línea para sus temas de ayuda, vea [about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/99a81ccc-21a0-49ec-a1b3-9efe2b4c0bbf) y el tema sobre cómo escribir ayuda de cmdlets ("How to Write Cmdlet Help", [http://go.microsoft.com/fwlink/?LinkID=123415](http://go.microsoft.com/fwlink/?LinkID=123415)) en la biblioteca de MSDN (Microsoft Developer Network).

## Consulte también
[about_Functions [m2]](https://technet.microsoft.com/en-us/library/61d40692-5300-4de9-a9b5-bae31815e105)
[about_Scripts](https://technet.microsoft.com/en-us/library/7dc08334-dcfe-450b-b949-0554855623af)
[about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/99a81ccc-21a0-49ec-a1b3-9efe2b4c0bbf)
[Get-Help [m2]](https://technet.microsoft.com/en-us/library/2d7fe1b4-0025-4580-a911-d81922dd6cd2)



<!--HONumber=May16_HO2-->


