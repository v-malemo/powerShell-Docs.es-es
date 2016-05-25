---
title:  Trabajar con entradas del Registro
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  fd254570-27ac-4cc9-81d4-011afd29b7dc
---

# Trabajar con entradas del Registro
Las entradas del Registro son propiedades de claves y, como tales, no se pueden examinar de forma directa, de modo que es preciso adoptar un enfoque ligeramente diferente al trabajar con ellas.

### Enumerar entradas del Registro
Existen muchas formas de examinar entradas del Registro. La más sencilla consiste en obtener los nombres de propiedad asociados a una clave. Por ejemplo, para ver los nombres de las entradas en la clave del Registro **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion**, use **Get-Item**. Las claves del Registro tienen una propiedad con el nombre genérico "Property" que es una lista de entradas del Registro en la clave. Con el siguiente comando se selecciona la propiedad Property y se expanden los elementos de forma que se muestran en una lista:

```
PS> Get-Item -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion | Select-Object -ExpandProperty Property
DevicePath
MediaPathUnexpanded
ProgramFilesDir
CommonFilesDir
ProductId
```

Para ver las entradas del Registro en un formato más legible, use **Get-ItemProperty**:

```
PS> Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion

PSPath              : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SO
                      FTWARE\Microsoft\Windows\CurrentVersion
PSParentPath        : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SO
                      FTWARE\Microsoft\Windows
PSChildName         : CurrentVersion
PSDrive             : HKLM
PSProvider          : Microsoft.PowerShell.Core\Registry
DevicePath          : C:\WINDOWS\inf
MediaPathUnexpanded : C:\WINDOWS\Media
ProgramFilesDir     : C:\Program Files
CommonFilesDir      : C:\Program Files\Common Files
ProductId           : 76487-338-1167776-22465
WallPaperDir        : C:\WINDOWS\Web\Wallpaper
MediaPath           : C:\WINDOWS\Media
ProgramFilesPath    : C:\Program Files
PF_AccessoriesName  : Accessories
(default)           :
```

Las propiedades relativas a Windows PowerShell de la clave tienen antepuesto el prefijo "PS", como **PSPath**, **PSParentPath**, **PSChildName** y **PSProvider**.

Puede usar la notación "**.**" para hacer referencia a la ubicación actual. Puede usar **Set-Location** para cambiar al contenedor del Registro **CurrentVersion** en primer lugar:

```
Set-Location -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
```

Otra opción consiste en usar el elemento integrado HKLM PSDrive con **Set-Location**:

```
Set-Location -Path hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion
```

Luego, puede usar la notación "**.**" de la ubicación actual para enumerar las propiedades sin especificar una ruta de acceso completa:

```
PS> Get-ItemProperty -Path .
...
DevicePath          : C:\WINDOWS\inf
MediaPathUnexpanded : C:\WINDOWS\Media
ProgramFilesDir     : C:\Program Files
...
```

La expansión de la ruta de acceso funciona igual a como lo hace en el sistema de archivos, de modo que desde esta ubicación se puede obtener la lista de **ItemProperty** de **HKLM:\SOFTWARE\Microsoft\Windows\Help** usando **Get-ItemProperty -Path ..\Help**.

### Obtener una sola entrada del Registro
Si quiere recuperar una entrada específica de una clave del Registro, puede usar uno de los diversos métodos posibles existentes. En este ejemplo, se encuentra el valor de **DevicePath** en **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion**.

Con **Get-ItemProperty**, use el parámetro **Path** para especificar el nombre de la clave y el parámetro **Name** para especificar el nombre de la entrada **DevicePath**.

```
PS> Get-ItemProperty -Path HKLM:\Software\Microsoft\Windows\CurrentVersion -Name DevicePath

PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\Software\
               Microsoft\Windows\CurrentVersion
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\Software\
               Microsoft\Windows
PSChildName  : CurrentVersion
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
DevicePath   : C:\WINDOWS\inf
```

Este comando devuelve las propiedades estándar de Windows PowerShell, así como la propiedad **DevicePath**.

> [!NOTE]
> Aunque **Get-ItemProperty** tiene los parámetros **Filter**, **Include** y **Exclude**, no se pueden usar para filtrar por nombre de propiedad. Estos parámetros hacen referencia a claves del Registro (que son rutas de acceso de los elementos) y no a entradas del Registro (que son propiedades de los elementos).

Otra opción sería usar la herramienta de línea de comandos Reg.exe. Para obtener ayuda con reg.exe, escriba **reg.exe /?** en un símbolo del sistema. Para buscar la entrada DevicePath, use reg.exe tal y como se muestra en el siguiente comando:

```
PS> reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion /v DevicePath

! REG.EXE VERSION 3.0

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
    DevicePath  REG_EXPAND_SZ   %SystemRoot%\inf
```

También puede usar el objeto **WshShell COM** para encontrar algunas entradas del Registro, si bien este método no funciona con datos binarios de gran tamaño o con nombres de entradas del Registro que contengan caracteres como "\". Anexe el nombre de propiedad a la ruta de acceso de elemento con una separador \:

```
PS> (New-Object -ComObject WScript.Shell).RegRead("HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\DevicePath")
%SystemRoot%\inf
```

### Crear entradas del Registro
Para agregar una nueva entrada denominada "PowerShellPath" a la clave **CurrentVersion**, use **New-ItemProperty** con la ruta de acceso a la clave, el nombre de la entrada y el valor de la entrada. En este ejemplo, tomaremos el valor de la variable de Windows PowerShell **$PSHome**, que almacena la ruta de acceso al directorio de instalación de Windows PowerShell.

Puede agregar la nueva entrada a la clave usando el siguiente comando; este comando también devolverá información sobre la nueva entrada:

```
PS> New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -PropertyType String -Value $PSHome

PSPath         : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWAR
                 E\Microsoft\Windows\CurrentVersion
PSParentPath   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWAR
                 E\Microsoft\Windows
PSChildName    : CurrentVersion
PSDrive        : HKLM
PSProvider     : Microsoft.PowerShell.Core\Registry
PowerShellPath : C:\Program Files\Windows PowerShell\v1.0
```

**PropertyType** debe ser el nombre de un miembro de la enumeración **Microsoft.Win32.RegistryValueKind** de la siguiente tabla:

|Valor de PropertyType|Significado|
|----------------------|-----------|
|Binary|Datos binarios|
|DWord|Un número que es un UInt32 válido|
|ExpandString|Una cadena que puede contener variables de entorno que se expanden dinámicamente|
|MultiString|Una cadena de varias líneas|
|Cadena|Cualquier valor de cadena|
|QWord|8 bytes de datos binarios|

> [!NOTE] Una entrada del Registro se puede agregar a varias ubicaciones si se especifica una matriz de valores para el parámetro **Path**:

```
New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion, HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -PropertyType String -Value $PSHome
```

Un valor de entrada del Registro preexistente también se puede sobrescribir si se agrega el parámetro **Force** a cualquier comando **New-ItemProperty**.

### Cambiar el nombre de entradas del Registro
Para cambiar el nombre de la entrada **PowerShellPath** a "PSHome," use **Rename-ItemProperty**:

```
Rename-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -NewName PSHome
```

Para mostrar el valor cuyo nombre ha cambiado, agregue el parámetro **PassThru** al comando.

```
Rename-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath -NewName PSHome -passthru
```

### Eliminar entradas del Registro
Para eliminar las entradas del Registro PSHome y PowerShellPath, use **Remove-ItemProperty**:

```
Remove-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PSHome
Remove-ItemProperty -Path HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion -Name PowerShellPath
```



<!--HONumber=May16_HO2-->


