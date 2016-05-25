---
title:  Ver la estructura del objeto (Get-Member) 
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  a1819ed2-2ef3-453a-b2b0-f3589c550481
---

# Ver la estructura del objeto (Get-Member)
Dado que los objetos desempeñan una función esencial en Windows PowerShell, existen varios comandos nativos diseñados para trabajar con tipos de objetos arbitrarios. El más importante es el comando **Get-Member**.

Es la técnica más sencilla para analizar los objetos que devuelve un comando es canalizar la salida del comando al cmdlet **Get-Member**. El cmdlet **Get-Member** muestra el nombre formal del tipo de objeto y una lista completa de sus miembros. En ocasiones, el número de elementos devuelto puede ser excesivo. Por ejemplo, un objeto de proceso puede tener más de 100 miembros.

Para ver a todos los miembros de un objeto de proceso y paginar la salida para verla completamente, escriba:

```
PS> Get-Process | Get-Member | Out-Host -Paging
```

La salida de este comando tendrá un aspecto similar al siguiente:

```
TypeName: System.Diagnostics.Process

Name                           MemberType     Definition
----                           ----------     ----------
Handles                        AliasProperty  Handles = Handlecount
Name                           AliasProperty  Name = ProcessName
NPM                            AliasProperty  NPM = NonpagedSystemMemorySize
PM                             AliasProperty  PM = PagedMemorySize
VM                             AliasProperty  VM = VirtualMemorySize
WS                             AliasProperty  WS = WorkingSet
add_Disposed                   Method         System.Void add_Disposed(Event...
...
```

Podemos hacer que esta larga lista de información sea más fácil de usar mediante el filtrado de los elementos que quiere ver. El comando **Get-Member** permite mostrar solo los miembros que son propiedades. Existen varios formatos de propiedades. El cmdlet muestra las propiedades de cualquier tipo si se establece el parámetro **Get-MemberMemberType** en el valor **Properties**. La lista resultante todavía es muy larga, pero un poco más fácil de administrar:

```
PS> Get-Process | Get-Member -MemberType Properties

   TypeName: System.Diagnostics.Process

Name                       MemberType     Definition
----                       ----------     ----------
Handles                    AliasProperty  Handles = Handlecount
Name                       AliasProperty  Name = ProcessName
...
ExitCode                   Property       System.Int32 ExitCode {get;}
...
Handle                     Property       System.IntPtr Handle {get;}
...
CPU                        ScriptProperty System.Object CPU {get=$this.Total...
...
Path                       ScriptProperty System.Object Path {get=$this.Main...
...
```

> [!NOTE] Los valores permitidos de MemberType son AliasProperty, CodeProperty, Property, NoteProperty, ScriptProperty, Properties, PropertySet, Method, CodeMethod, ScriptMethod, Methods, ParameterizedProperty, MemberSet y All.

Existen más de 60 propiedades para un proceso. La razón por la que Windows PowerShell suele mostrar solo un conjunto de propiedades para cualquier objeto conocido, es que en caso de mostrarlos todos se generaría una cantidad de información imposible de administrar.

> [!NOTE]
> Windows PowerShell determina cómo mostrar un tipo de objeto mediante la información almacenada en archivos XML cuyos nombres terminan con .format.ps1xml. Los datos de formato de objetos de proceso, que son objetos System.Diagnostics.Process de .NET, se almacenan en PowerShellCore.format.ps1xml.

Si necesita consultar propiedades distintas de las que Windows PowerShell muestra de forma predeterminada, deberá formatear los datos de salida personalmente. Esto se puede hacer mediante los cmdlets de formato.



<!--HONumber=May16_HO2-->


