---
title:  Obtener información sobre los comandos
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  56f8e5b4-d97c-4e59-abbe-bf13e464eb0d
---

# Obtener información sobre los comandos
El cmdlet **Get-Command** de Windows PowerShell obtiene todos los comandos disponibles en la sesión actual. Cuando **Get-Command** se escribe en un símbolo del sistema de Windows PowerShell, se obtiene un resultado similar al siguiente:

```
PS> Get-Command
CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Add-Content                     Add-Content [-Path] <String[...
Cmdlet          Add-History                     Add-History [[-InputObject] ...
Cmdlet          Add-Member                      Add-Member [-MemberType] <PS...
...
```

Este resultado se parece mucho al resultado de la Ayuda de Cmd.exe: es un resumen tabular de comandos internos. En el extracto de la salida del comando **Get-Command** mostrado anteriormente, la sección CommandType de cada comando es Cmdlet. Un cmdlet es el tipo de comando intrínseco de Windows PowerShell: un tipo que equivale en cierto modo a los comandos **dir** y **cd** de Cmd.exe y a elementos integrados de los shells de UNIX, como BASH.

En la salida del comando **Get-Command**, todas las definiciones terminan con puntos suspensivos (...) para indicar que PowerShell no puede mostrar todo el contenido en el espacio disponible. Cuando Windows PowerShell muestra un resultado, le aplica un formato de texto y, a continuación, lo organiza para ajustar los datos correctamente en la ventana. Hablaremos sobre esto más adelante en la sección sobre formateadores.

El cmdlet **Get-Command** tiene un parámetro **Syntax** con el que se obtiene la sintaxis de cada cmdlet. Para obtener la sintaxis del cmdlet Get-Help, use el siguiente comando:

**Get-Command Get-Help -Syntax**

```
Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Component <String[]>] [-Functionality <String[]>]
 [-Role <String[]>] [-Full] [-Online] [-Verbose] [-Debug] [-ErrorAction <ActionPreference>] [-WarningAction <ActionPreference>] [-ErrorVariable <String>] [-WarningVariable <String>] [-OutVariable <String>] [-OutBuffer <Int32>]

Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Component <String[]>] [-Functionality <String[]>]
 [-Role <String[]>] [-Detailed] [-Online] [-Verbose] [-Debug] [-ErrorAction <ActionPreference>] [-WarningAction <ActionPreference>] [-ErrorVariable <String>] [-WarningVariable <String>] [-OutVariable <String>] [-OutBuffer <Int32>]

Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Component <String[]>] [-Functionality <String[]>]
 [-Role <String[]>] [-Examples] [-Online] [-Verbose] [-Debug] [-ErrorAction <ActionPreference>] [-WarningAction <ActionPreference>] [-ErrorVariable <String>] [-WarningVariable <String>] [-OutVariable <String>] [-OutBuffer <Int32>]

Get-Help [[-Name] <String>] [-Path <String>] [-Category <String[]>] [-Component <String[]>] [-Functionality <String[]>]
 [-Role <String[]>] [-Parameter <String>] [-Online] [-Verbose] [-Debug] [-ErrorAction <ActionPreference>] [-WarningAction <ActionPreference>] [-ErrorVariable <String>] [-WarningVariable <String>] [-OutVariable <String>] [-OutBuffer <Int32>]
```

### Mostrar tipos de comandos disponibles
El comando **Get-Command** no enumera todos los comandos que están disponibles en Windows PowerShell. En su lugar, el comando **Get-Command** enumera únicamente los cmdlets de la sesión actual. En realidad, Windows PowerShell admite otros muchos tipos de comandos. Los alias, funciones y scripts también son comandos de Windows PowerShell, aunque no se describen en detalle en la guía de usuario de Windows PowerShell. Los archivos externos que son ejecutables o que tienen un controlador de tipo de archivo registrado, también son comandos.

Para obtener todos los comandos de la sesión, escriba:

```
Get-Command *
```

Esta lista incluye los archivos externos de su ruta de búsqueda, con lo cual puede contener miles de elementos. Es más útil buscar en un conjunto reducido de comandos.

Para obtener los comandos nativos de otros tipos, use el parámetro **CommandType** del cmdet **Get-Command**.

> [!NOTE]
> El asterisco (*) sirve de comodín para hallar coincidencias en los argumentos de comando de Windows PowerShell. El carácter * quiere decir "coincide con uno o varios caracteres". Así, puede escribir **Get-Command a&#42;** para encontrar todos los comandos que comienzan por la letra "a". A diferencia de las búsquedas con comodín en Cmd.exe, los comodines de Windows PowerShell también encontrarán coincidencias de período.

Para obtener los alias de comandos (que son los sobrenombres asignados de los comandos), escriba:

```
Get-Command -CommandType Alias
```

Para obtener las funciones en la sesión actual, escriba:

```
Get-Command -CommandType Function
```

Para mostrar los scripts en la ruta de búsqueda de Windows PowerShell, escriba:

```
Get-Command -CommandType Script
```



<!--HONumber=May16_HO2-->


