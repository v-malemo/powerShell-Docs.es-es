---
title: Seleccionar partes de objetos (Select-Object)
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 72e64b1a-d351-4500-9da3-24d8a71d7a92
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: c5ec8b902096f85e4d5f5575587434f2adf7c6a1

---

# Seleccionar partes de objetos (Select-Object)
Puede usar el cmdlet **Select\-Object** para crear objetos de Windows PowerShell personalizados que contengan las propiedades seleccionadas de los objetos que usa para crearlos. Escriba el siguiente comando para crear un objeto que incluya solamente las propiedades Name y FreeSpace de la clase WMI Win32\_LogicalDisk:

```
PS> Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace

Name                                    FreeSpace
----                                    ---------
C:                                      50664845312
```

No puede ver el tipo de datos después de emitir este comando, pero si canaliza la salida para Get\-Member después de Select\-Object, puede indicar que tiene un nuevo tipo de objeto, PSCustomObject:

```
PS> Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace| Get-Member

   TypeName: System.Management.Automation.PSCustomObject

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       System.Boolean Equals(Object obj)
GetHashCode Method       System.Int32 GetHashCode()
GetType     Method       System.Type GetType()
ToString    Method       System.String ToString()
FreeSpace   NoteProperty  FreeSpace=...
Name        NoteProperty System.String Name=C:
```

Select\-Object tiene muchos usos. Uno de ellos es replicar datos que se puedan modificar posteriormente. Ahora podemos gestionar el problema detectado a través de la sección anterior. Podemos actualizar el valor de FreeSpace en nuestros objetos recién creados y la salida incluirá la etiqueta descriptiva:

```
Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace | ForEach-Object -Process {$_.FreeSpace = ($_.FreeSpace)/1024.0/1024.0; $_}
Name                                                                  FreeSpace
----                                                                  ---------
C:                                                                48317.7265625
```




<!--HONumber=Jun16_HO4-->


