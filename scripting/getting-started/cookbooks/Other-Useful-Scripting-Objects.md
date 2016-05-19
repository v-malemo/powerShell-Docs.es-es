---
title: Otros objetos de scripting útiles
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d781196-720b-4ccc-90d2-c570e5e719f5
---
# Otros objetos de scripting útiles
  Los objetos siguientes proporcionan funcionalidad adicional de scripting de Windows PowerShell ISE. No forman parte de la jerarquía de **$psISE**.

## Objetos de scripting útiles

### $psUnsupportedConsoleApplications
 Existen algunas limitaciones sobre cómo Windows PowerShell ISE interactúa con las aplicaciones de consola. Un comando o un script de automatización que requiere la intervención de usuario podrían no funcionar de la misma forma que desde la consola de Windows PowerShell. Puede bloquear la ejecución de estos comandos o scripts en el panel de comandos de Windows PowerShell ISE. El objeto **$psUnsupportedConsoleApplications** mantiene una lista de estos comandos. Si intenta ejecutar los comandos de esta lista, recibirá un mensaje donde se indica que no son compatibles. El siguiente script agrega una entrada a la lista.

```
# List the unsupported commands
psUnsupportedConsoleApplications
# Add a command to this list
psUnsupportedConsoleApplications.Add(“Mycommand”)
#Show the augmented list of commands
psUnsupportedConsoleApplications

```

### $psLocalHelp
 Se trata de un objeto de diccionario que mantiene una asignación contextual entre los temas de Ayuda y sus vínculos asociados en el archivo de Ayuda HTML compilado local. Se utiliza para buscar la Ayuda local para un tema determinado. Puede agregar o eliminar los temas de esta lista. En el ejemplo de código siguiente se muestran algunos pares clave-valor de ejemplo que se encuentran en **$psLocalHelp**..

```
# See the local help map
$psLocalHelp |Format-List

```

### Salida de muestra

|||
|-|-|
|Clave: Add-Computer|Valor: WindowsPowerShellHelp.chm::/html/093f660c-b8d5-43cf-aa0c-54e5e54e76f9.htm|
|Clave: Add-Content|Valor: WindowsPowerShellHelp.chm::/html/0c836a1b-f389-4e9a-9325-0f415686d194.htm|

 El siguiente script agrega una entrada a la lista.

```
$psLocalHelp.Add("get-myNoun","c:\MyFolder\MyHelpChm.chm::/html/0198854a-1298-57ae-aa0c-87b5e5a84712.htm")
```

### $psOnlineHelp
 Se trata de un objeto de diccionario que mantiene una asignación contextual entre los títulos de los temas de Ayuda y sus direcciones URL externas asociadas. Se utiliza para buscar la Ayuda para un tema determinado en la Web. Puede agregar o eliminar los temas de esta lista.

```
$psOnlineHelp |format-list

```

### Salida de muestra

|||
|-|-|
|Clave: Add-Computer|Valor: http:\/\/go.microsoft.com\/fwlink\/p\/?LinkID\=135194|
|Clave: Add-Content|Valor: http://go.microsoft.com/fwlink/p/?LinkID=113278|

 El siguiente script agrega una entrada a la lista.

```
$psOnlineHelp.Add("get-myNoun","http://www.mydomain.com/MyNoun.html")
```

## Consulte también
 [El modelo de objetos de scripting de ISE de Windows PowerShell](../../core-powershell/ise/The-Windows-PowerShell-ISE-Scripting-Object-Model.md)

  


<!--HONumber=May16_HO2-->


