---
title:  Trabajar con claves del Registro
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  91bfaecd-8684-48b4-ad86-065dfe6dc90a
---

# Trabajar con claves del Registro
Dado que las claves del Registro son elementos en unidades de Windows PowerShell, trabajar con ellas es muy similar a trabajar con archivos y carpetas. Una diferencia fundamental es que todos los elementos en una unidad de Windows PowerShell basada en el Registro es un contenedor, como una carpeta en una unidad del sistema de archivos. Sin embargo, las entradas del Registro y sus valores asociados son propiedades de los elementos, no elementos distintos.

### Enumerar a todas las subclaves de una clave del Registro
Puede mostrar todos los elementos directamente dentro de una clave del Registro mediante **Get-ChildItem**. Agregue el parámetro **Force** opcional para mostrar elementos ocultos o del sistema. Por ejemplo, este comando muestra los elementos directamente en la unidad HKCU: de Windows PowerShell, que se corresponde con el subárbol del Registro HKEY_CURRENT_USER:

```
PS> Get-ChildItem -Path hkcu:\

   Hive: Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER

SKC  VC Name                           Property
---  -- ----                           --------
  2   0 AppEvents                      {}
  7  33 Console                        {ColorTable00, ColorTable01, ColorTab...
 25   1 Control Panel                  {Opened}
  0   5 Environment                    {APR_ICONV_PATH, INCLUDE, LIB, TEMP...}
  1   7 Identities                     {Last Username, Last User ...
  4   0 Keyboard Layout                {}
...
```

Estas son las claves de nivel superior visibles en HKEY_CURRENT_USER en el Editor del Registro (Regedit.exe).

Para especificar esta ruta de acceso del Registro también puede especificar el nombre del proveedor del Registro, seguido de "**::**". El nombre completo del proveedor del Registro es **Microsoft.PowerShell.Core\Registry**, pero esto se puede abreviar simplemente a **Registry**. Cualquiera de los siguientes comandos enumerará el contenido directamente en HKCU:

```
Get-ChildItem -Path Registry::HKEY_CURRENT_USER
Get-ChildItem -Path Microsoft.PowerShell.Core\Registry::HKEY_CURRENT_USER
Get-ChildItem -Path Registry::HKCU
Get-ChildItem -Path Microsoft.PowerShell.Core\Registry::HKCU
Get-ChildItem HKCU:
```

Estos comandos muestran solo los elementos contenidos directamente, lo que se parece a usar el comando **DIR** de Cmd.exe o **ls** en un shell de UNIX. Para mostrar los elementos contenidos, debe especificar el parámetro **Recurse**. Para enumerar todas las claves del Registro en HKCU, use el comando siguiente (esta operación puede tardar una gran cantidad de tiempo):

```
Get-ChildItem -Path hkcu:\ -Recurse
```

**Get-ChildItem** puede realizar funciones complejas de filtrado a través de sus parámetros **Path**, **Filter**, **Include** y **Exclude**, pero estos parámetros suelen basarse solo en el nombre. Puede realizar un filtrado complejo basado en otras propiedades de elementos mediante el cmdlet **Where-Object**. El siguiente comando busca todas las claves de HKCU:\Software que no tienen más de una subclave y que tienen exactamente cuatro valores:

```
Get-ChildItem -Path HKCU:\Software -Recurse | Where-Object -FilterScript {($_.SubKeyCount -le 1) -and ($_.ValueCount -eq 4) }
```

### Copiar claves
La copia se realiza con **Copy-Item**. El siguiente comando copia HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion y todas sus propiedades en HKCU:\, y crea una nueva clave denominada "CurrentVersion":

```
Copy-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion' -Destination hkcu:
```

Si examina esta nueva clave en el Editor del Registro o mediante **Get-ChildItem**, observará que no tiene copias de las subclaves contenidas en la nueva ubicación. Para copiar todo el contenido de un contenedor, debe especificar el parámetro **Recurse**. Para hacer que el comando de copia anterior sea recursivo, debe usar este comando:

```
Copy-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion' -Destination hkcu: -Recurse
```

Puede seguir usando otras herramientas que tiene a su disposición para realizar copias del sistema de archivos. Todas las herramientas de edición del Registro, incluidas reg.exe, regini.exe y regedit.exe, y los objetos COM que admiten la edición del Registro (como WScript.Shell y la clase WMI StdRegProv) pueden usarse desde Windows PowerShell.

### Crear claves
Crear nuevas claves en el Registro es más sencillo que crear un nuevo elemento en un sistema de archivos. Dado que todas las claves del Registro son contenedores, no es necesario especificar el tipo de elemento; solo tiene que proporcionar una ruta de acceso explícita, como:

```
New-Item -Path hkcu:\software\_DeleteMe
```

También puede usar una ruta de acceso basada en el proveedor para especificar una clave:

```
New-Item -Path Registry::HKCU\_DeleteMe
```

### Eliminar claves
El proceso de eliminación de elementos es básicamente igual para todos los proveedores. Los siguientes comandos quitarán elementos de forma automática:

```
Remove-Item -Path hkcu:\Software\_DeleteMe
Remove-Item -Path 'hkcu:\key with spaces in the name'
```

### Quitar todas las claves bajo una clave específica
Puede quitar los elementos contenidos mediante **Remove-Item**, pero se le pedirá que confirme la eliminación si el elemento contiene algo más. Por ejemplo, si se intenta eliminar la subclave HKCU:\CurrentVersion creada, se muestra lo siguiente:

```
Remove-Item -Path hkcu:\CurrentVersion

Confirm
The item at HKCU:\CurrentVersion\AdminDebug has children and the -recurse
parameter was not specified. If you continue, all children will be removed with
 the item. Are you sure you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):
```

Para eliminar los elementos contenidos sin preguntar, especifique el parámetro **-Recurse**:

```
Remove-Item -Path HKCU:\CurrentVersion -Recurse
```

Para quitar todos los elementos de HKCU:\CurrentVersion pero no de HKCU:\CurrentVersion, podría usar en su lugar:

```
Remove-Item -Path HKCU:\CurrentVersion\* -Recurse
```



<!--HONumber=May16_HO2-->


