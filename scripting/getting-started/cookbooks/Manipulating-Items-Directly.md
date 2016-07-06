---
title: Manipular elementos directamente
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 8cbd4867-917d-41ea-9ff0-b8e765509735
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: b7e752a1615da4540106ec32754f873c5d7aa5d9

---

# Manipular elementos directamente
Los elementos que se ven en las unidades de Windows PowerShell (como los archivos y carpetas en las unidades del sistema de archivos y las claves del Registro en las unidades de Registro de Windows PowerShell) se denominan *elementos* en Windows PowerShell. Los cmdlets que funcionan con los elementos contienen el término **Item** en sus nombres.

La salida del comando **Get\-Command \-Noun Item** indica que hay nueve cmdlets de elemento en Windows PowerShell.

```
PS> Get-Command -Noun Item

CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Clear-Item                      Clear-Item [-Path] <String[]...
Cmdlet          Copy-Item                       Copy-Item [-Path] <String[]>...
Cmdlet          Get-Item                        Get-Item [-Path] <String[]> ...
Cmdlet          Invoke-Item                     Invoke-Item [-Path] <String[...
Cmdlet          Move-Item                       Move-Item [-Path] <String[]>...
Cmdlet          New-Item                        New-Item [-Path] <String[]> ...
Cmdlet          Remove-Item                     Remove-Item [-Path] <String[...
Cmdlet          Rename-Item                     Rename-Item [-Path] <String>...
Cmdlet          Set-Item                        Set-Item [-Path] <String[]> ...
```

### Creación de elementos (New\-Item)
Para crear un elemento en el sistema de archivos, use el cmdlet **New\-Item**. Incluya el parámetro **Path** con la ruta de acceso al elemento y el parámetro **ItemType** con un valor "file" o "directory".

Por ejemplo, para crear un directorio denominado "New.Directory" en el directorio C:\\Temp, escriba:

```
PS> New-Item -Path c:\temp\New.Directory -ItemType Directory

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2006-05-18  11:29 AM            New.Directory
```

Para crear un archivo, cambie el valor de **ItemType** a "file". Por ejemplo, para crear un archivo denominado "file1.txt" en el directorio New.Directory, escriba:

```
PS> New-Item -Path C:\temp\New.Directory\file1.txt -ItemType file

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp\New.Directory

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2006-05-18  11:44 AM          0 file1
```

Esta misma técnica se puede usar para crear una clave del Registro. De hecho, una clave del Registro es más fácil de crear porque se trata del único tipo de elemento que hay en el Registro de Windows. (Las entradas del Registro son *propiedades* de los elementos). Por ejemplo, para crear una clave denominada "\_Test" en la subclave CurrentVersion, escriba:

```
PS> New-Item -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\_Test

   Hive: Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Micros
oft\Windows\CurrentVersion

SKC  VC Name                           Property
---  -- ----                           --------
  0   0 _Test                          {}
```

Cuando escriba una ruta de acceso del Registro, no olvide incluir dos puntos (**:**) en los nombres de unidad de Windows PowerShell, HKLM: y HKCU:. Sin esos dos puntos, Windows PowerShell no reconoce el nombre de la unidad en la ruta de acceso.

### ¿Por qué valores del Registro no son elementos?
Si usa el cmdlet **Get\-ChildItem** para encontrar los elementos en una clave del Registro, nunca verá las entradas del Registro reales ni sus valores.

Por ejemplo, la clave del Registro **HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Run** suele contener varias entradas del Registro que representan las aplicaciones que se ejecutan cuando el sistema se inicia.

Sin embargo, si usa **Get\-ChildItem** para buscar elementos secundarios en la clave, solamente verá la subclave **OptionalComponents** de la clave:

```
PS> Get-ChildItem HKLM:\Software\Microsoft\Windows\CurrentVersion\Run
   Hive: Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\Software\Micros
oft\Windows\CurrentVersion\Run
SKC  VC Name                           Property
---  -- ----                           --------
  3   0 OptionalComponents             {}
```

Aunque posiblemente sea conveniente tratar las entradas del Registro como elementos, no se puede especificar una ruta de acceso a una entrada del Registro de una manera que garantice que sea única. La notación de ruta de acceso no distingue entre la subclave del Registro **Run** y la entrada del Registro **(Default)** en la subclave **Run**. Es más, dado que los nombres de las entradas del Registro pueden contener el carácter de barra diagonal inversa (**\\**), si las entradas de Registro fueran elementos, no podría usar la notación de ruta de acceso para distinguir una entrada del Registro llamada **Windows\\CurrentVersion\\Run** de la subclave que se encuentra en esa ruta de acceso.

### Cambio del nombre de los elementos existentes (Rename\-Item)
Para cambiar el nombre de un archivo o una carpeta, use el cmdlet **Rename\-Item**. El siguiente comando cambia el nombre del archivo **file1.txt** a **fileOne.txt**.

```
PS> Rename-Item -Path C:\temp\New.Directory\file1.txt fileOne.txt
```

El cmdlet **Rename\-Item** puede cambiar el nombre de un archivo o una carpeta, pero no puede mover un elemento. El siguiente comando genera un error porque intenta mover el archivo del directorio New.Directory al directorio Temp.

```
PS> Rename-Item -Path C:\temp\New.Directory\fileOne.txt c:\temp\fileOne.txt
Rename-Item : Cannot rename because the target specified is not a path.
At line:1 char:12
+ Rename-Item  <<<< -Path C:\temp\New.Directory\fileOne c:\temp\fileOne.txt
```

### Traslado de elementos (Move\-Item)
Para mover un archivo o una carpeta, use el cmdlet **Move\-Item**.

Por ejemplo, el siguiente comando mueve el directorio New.Directory del directorio C:\\temp a la raíz de la unidad C:. Para confirmar que el elemento se ha movido, incluya el parámetro **PassThru** del cmdlet **Move\-Item**. Sin **Passthru**, el cmdlet **Move\-Item** no muestra ningún resultado.

```
PS> Move-Item -Path C:\temp\New.Directory -Destination C:\ -PassThru

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2006-05-18  12:14 PM            New.Directory
```

### Copia de elementos (Copy\-Item)
Si está familiarizado con las operaciones de copia de otros shells, el comportamiento del cmdlet **Copy\-Item** de Windows PowerShell le puede parecer inusual. Al copiar un elemento de una ubicación en otra, Copy\-Item no copia el contenido de forma predeterminada.

Por ejemplo, si copia el directorio **New.Directory** de la unidad C: en el directorio C:\\temp, el comando se ejecuta correctamente, pero los archivos del directorio New.Directory no se copian.

```
PS> Copy-Item -Path C:\New.Directory -Destination C:\temp
```

Si muestra el contenido de **C:\\temp\\New.Directory**, verá que no hay archivos:

```
PS> Get-ChildItem -Path C:\temp\New.Directory
PS>
```

¿Por qué el cmdlet **Copy\-Item** no copia el contenido en la nueva ubicación?

El cmdlet **Copy\-Item** está diseñado para ser genérico; no sirve solo para copiar archivos y carpetas. Además, incluso cuando se copian archivos y carpetas, conviene copiar solo el contenedor y no los elementos que hay en él.

Para copiar todo el contenido de una carpeta, incluya el parámetro **Recurse** del cmdlet **Copy\-Item** en el comando. Si ya ha copiado el directorio sin su contenido, agregue el parámetro **Force**, que permite sobrescribir la carpeta vacía.

```
PS> Copy-Item -Path C:\New.Directory -Destination C:\temp -Recurse -Force -Passthru
    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        2006-05-18   1:53 PM            New.Directory

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\temp\New.Directory

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2006-05-18  11:44 AM          0 file1
```

### Eliminación de elementos (Remove\-Item)
Use el cmdlet **Remove\-Item** para eliminar archivos y carpetas. A menudo, los cmdlets de Windows PowerShell como **Remove\-Item**, que pueden realizar cambios importantes e irreversibles, pedirán confirmación al escribir sus comandos. Así, si intenta quitar la carpeta **New.Directory**, se le pedirá que confirme el comando, ya que la carpeta contiene archivos:

```
PS> Remove-Item C:\New.Directory

Confirm
The item at C:\temp\New.Directory has children and the -recurse parameter was not
specified. If you continue, all children will be removed with the item. Are you
 sure you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):
```

Dado que **Sí** es la respuesta predeterminada, presione la tecla **Entrar** para eliminar la carpeta y sus archivos. Para quitar la carpeta sin confirmar, use el parámetro **\-Recurse**.

```
PS> Remove-Item C:\temp\New.Directory -Recurse
```

### Ejecución de elementos (Invoke\-Item)
Windows PowerShell usa el cmdlet **Invoke\-Item** para realizar una acción predeterminada relativa a un archivo o una carpeta. Esta acción predeterminada viene determinada por el controlador de aplicación predeterminado en el Registro; el efecto es el mismo que si hiciera doble clic en el elemento en el Explorador de archivos.

Por ejemplo, suponga que ejecuta el siguiente comando:

```
PS> Invoke-Item C:\WINDOWS
```

Se abre una ventana del explorador en la ubicación C:\\Windows, igual que si hubiera hecho doble clic en la carpeta C:\\Windows.

Si invoca el archivo **Boot.ini** en un sistema previo a Windows Vista:

```
PS> Invoke-Item C:\boot.ini
```

Si el tipo de archivo .ini está asociado con el Bloc de notas, se abrirá en el Bloc de notas.




<!--HONumber=Jun16_HO4-->


