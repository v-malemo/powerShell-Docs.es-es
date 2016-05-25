---
title:  Trabajar con instalaciones de software
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  51a12fe9-95f6-4ffc-81a5-4fa72a5bada9
---

# Trabajar con instalaciones de software
Puede usar la clase WMI **Win32_Product** para acceder a las aplicaciones diseñadas para usar Windows Installer, si bien no todas las aplicaciones de hoy en día usan Windows Installer. Dado que Windows Installer proporciona la gama más amplia de técnicas estándar para trabajar con aplicaciones instalables, nos centraremos principalmente en esas aplicaciones. Por lo general, las aplicaciones que usan otras rutinas de instalación no se administrarán con Windows Installer. Las técnicas concretas para trabajar con esas aplicaciones dependerán del instalador de software y de las decisiones tomadas por el desarrollador de la aplicación.

> [!NOTE]
> Las aplicaciones que se instalan copiando los archivos de la aplicación en el equipo normalmente no se suelen poder administrar con las técnicas aquí descritas. Puede administrar estas aplicaciones como archivos y carpetas recurriendo a las técnicas descritas en la sección "Trabajar con archivos y carpetas".

### Lista de aplicaciones de Windows Installer
Para confeccionar una lista de las aplicaciones instaladas con Windows Installer en un sistema local o remoto, use esta sencilla consulta WMI:

```
PS> Get-WmiObject -Class Win32_Product -ComputerName .
IdentifyingNumber : {7131646D-CD3C-40F4-97B9-CD9E4E6262EF}
Name              : Microsoft .NET Framework 2.0
Vendor            : Microsoft Corporation
Version           : 2.0.50727
Caption           : Microsoft .NET Framework 2.0
```

Para mostrar todas las propiedades del objeto Win32_Product, use el parámetro Properties de los cmdlets de formato, como el cmdlet Format-List, con un valor de * (todos).

```
PS> Get-WmiObject -Class Win32_Product -ComputerName . | Where-Object -FilterScript {$_.Name -eq "Microsoft .NET Framework 2.0"} | Format-List -Property *
Name              : Microsoft .NET Framework 2.0
Version           : 2.0.50727
InstallState      : 5
Caption           : Microsoft .NET Framework 2.0
Description       : Microsoft .NET Framework 2.0
IdentifyingNumber : {7131646D-CD3C-40F4-97B9-CD9E4E6262EF}
InstallDate       : 20060506
InstallDate2      : 20060506000000.000000-000
InstallLocation   :
PackageCache      : C:\WINDOWS\Installer\619ab2.msi
SKUNumber         :
Vendor            : Microsoft Corporation
```

También podría usar el parámetro **Get-WmiObject Filter** para seleccionar únicamente Microsoft .NET Framework 2.0. Dado que el filtro empleado en este comando es un filtro WMI, se usa la sintaxis de lenguaje de consulta de WMI (WQL), no la sintaxis de Windows PowerShell. En su lugar:

```
Get-WmiObject -Class Win32_Product -ComputerName . -Filter "Name='Microsoft .NET Framework 2.0'"| Format-List -Property *
```

Tenga en cuenta que, a menudo, las consultas WQL usan caracteres (como espacios o signos igual) que en Windows PowerShell tienen un significado especial. Por esta razón, es recomendable incluir siempre el valor del parámetro Filter entre comillas. También puede usar el carácter de escape de Windows PowerShell, un acento grave (`), aunque puede que esto no mejore la legibilidad. El siguiente comando equivale al comando anterior y devuelve los mismos resultados, pero usa un acento grave para aplicar escape en los caracteres especiales, en lugar de entrecomillar la cadena de filtro completa.

```
Get-WmiObject -Class Win32_Product -ComputerName . -Filter Name`=`'Microsoft` .NET` Framework` 2.0`' | Format-List -Property *
```

Para mostrar solo las propiedades que le interesen, use el parámetro Property de los cmdlets.

```
Get-WmiObject -Class Win32_Product -ComputerName . | Format-List -Property Name,InstallDate,InstallLocation,PackageCache,Vendor,Version,IdentifyingNumber
...
Name              : HighMAT Extension to Microsoft Windows XP CD Writing Wizard
InstallDate       : 20051022
InstallLocation   : C:\Program Files\HighMAT CD Writing Wizard\
PackageCache      : C:\WINDOWS\Installer\113b54.msi
Vendor            : Microsoft Corporation
Version           : 1.1.1905.1
IdentifyingNumber : {FCE65C4E-B0E8-4FBD-AD16-EDCBE6CD591F}
...
```

Por último, para buscar solo los nombres de las aplicaciones instaladas, una sencilla instrucción **Format-Wide** simplifica la salida:

```
Get-WmiObject -Class Win32_Product -ComputerName .  | Format-Wide -Column 1
```

Ya tenemos varias formas de ver las aplicaciones que usan Windows Installer para instalarse, pero no hemos tenido en cuenta otras aplicaciones. La mayoría de las aplicaciones estándar registran su desinstalador con Windows, por lo que podemos trabajar con ellas localmente buscándolas en el Registro de Windows.

### Enumerar todas las aplicaciones no instalables
Aunque no hay ninguna manera en firme de encontrar todas las aplicaciones en un sistema, sí se pueden encontrar todos los programas en las listas que se muestran en el cuadro de diálogo Agregar o quitar programas. Agregar o quitar programas detecta estas aplicaciones en la siguiente clave del Registro:

**HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall**.

También podemos examinar esta clave para buscar aplicaciones. Podemos hacer que la clave Uninstall sea más fácil de ver si asignamos una unidad de Windows PowerShell a esta ubicación del Registro:

```
PS>    

Name       Provider      Root                                   CurrentLocation
----       --------      ----                                   ---------------
Uninstall  Registry      HKEY_LOCAL_MACHINE\SOFTWARE\Micr...
```

> [!NOTE]
> La unidad **HKLM:** se asigna a la raíz de **HKEY_LOCAL_MACHINE**, por lo que usamos esa unidad en la ruta de acceso a la clave Uninstall. En vez de **HKLM:**, podríamos haber especificado la ruta del Registro mediante **HKLM** o **HKEY_LOCAL_MACHINE**. La ventaja de usar una unidad del Registro existente es que podemos usar la finalización con tabulación para rellenar los nombres de claves, con lo cual no es necesario escribirlas.

Ya tenemos una unidad denominada "Uninstall" que se puede usar para buscar instalaciones de aplicaciones de forma cómoda y rápida. Podemos hallar el número de aplicaciones instaladas contando el número de claves del Registro en la unidad de Windows PowerShell Uninstall:

```
PS> (Get-ChildItem -Path Uninstall:).Count
459
```

Podemos realizar más búsquedas en esta lista de aplicaciones mediante diversas técnicas, comenzando por **Get-ChildItem**. Use el siguiente comando para obtener una lista de aplicaciones y guardarlas en la variable **$UninstallableApplications**:

```
$UninstallableApplications = Get-ChildItem -Path Uninstall:
```

> [!NOTE]
> Aquí usamos un nombre de variable largo para mayor claridad. En la realidad, no es necesario usar nombres largos. Aunque se puede recurrir a la finalización con tabulación en los nombres de variables, también se pueden usar nombres de 1-2 caracteres para acelerar el proceso. Los nombres más largos y descriptivos son muy útiles cuando se está desarrollando código para reutilizarlo.

Para ver los valores de las entradas del Registro en las claves del Registro bajo Uninstall, use el método GetValue en las claves del Registro. El valor del método es el nombre de la entrada del Registro.

Por ejemplo, use el siguiente comando para buscar los nombres para mostrar de las aplicaciones en la clave Uninstall:

```
PS> Get-ChildItem -Path Uninstall: | ForEach-Object -Process { $_.GetValue("DisplayName") }
```

No hay ninguna garantía de que estos valores sean únicos. En el siguiente ejemplo, aparecen dos elementos instalados como "Windows Media Encoder 9 Series":

```
PS> Get-ChildItem -Path Uninstall: | Where-Object -FilterScript { $_.GetValue("DisplayName") -eq "Windows Media Encoder 9 Series"}

   Hive: Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Micros
oft\Windows\CurrentVersion\Uninstall

SKC  VC Name                           Property
---  -- ----                           --------
  0   3 Windows Media Encoder 9        {DisplayName, DisplayIcon, UninstallS...
  0  24 {E38C00D0-A68B-4318-A8A6-F7... {AuthorizedCDFPrefix, Comments, Conta...
```

### Instalación de aplicaciones
Puede usar la clase **Win32_Product** para instalar paquetes de Windows Installer, tanto de forma local como remota.

> [!NOTE] En Windows Vista, Windows Server 2008 y versiones posteriores de Windows, hay que iniciar Windows PowerShell con la opción "Ejecutar como administrador" para instalar una aplicación.

En las instalaciones remotas, use una ruta de red de convención de nomenclatura Universal (UNC) para especificar la ruta de acceso al paquete .msi, ya que el subsistema WMI no entiende las rutas de acceso de Windows PowerShell. Por ejemplo, para instalar el paquete NewPackage.msi ubicado en el recurso compartido de red \\AppServ\dsp en el equipo remoto PC01, escriba el siguiente comando en el símbolo del sistema de Windows PowerShell:

```
(Get-WMIObject -ComputerName PC01 -List | Where-Object -FilterScript {$_.Name -eq "Win32_Product"}).Install(\\AppSrv\dsp\NewPackage.msi)
```

Las aplicaciones que no usan la tecnología de Windows Installer pueden tener métodos específicos de la aplicación disponibles para la implementación automatizada. Para saber si hay un método de automatización de la implementación, consulte la documentación de la aplicación pertinente o el sistema de Ayuda del proveedor de la aplicación. En algunos casos, incluso en aquellos en los que el proveedor de la aplicación no la haya diseñado específicamente para la automatización de la instalación, es posible que el fabricante del software de instalador posea algunas técnicas para la automatización.

### Desinstalación de aplicaciones
El proceso de desinstalación de un paquete de Windows Installer mediante Windows PowerShell funciona aproximadamente del mismo modo que la instalación de un paquete. En este ejemplo, el paquete que se va a desinstalar se selecciona por su nombre; en algunos casos, probablemente sea más fácil filtrar con **IdentifyingNumber**:

```
(Get-WmiObject -Class Win32_Product -Filter "Name='ILMerge'" -ComputerName . ).Uninstall()
```

Desinstalar otras aplicaciones no es tan sencillo, aun cuando se haga localmente. Encontraremos las cadenas de desinstalación de línea de comandos de estas aplicaciones extrayendo la propiedad **UninstallString**. Este método funciona con aplicaciones de Windows Installer y con los programas antiguos que aparecen bajo la clave Uninstall:

```
Get-ChildItem -Path Uninstall: | ForEach-Object -Process { $_.GetValue("UninstallString") }
```

Si lo desea, puede filtrar el resultado por el nombre para mostrar:

```
Get-ChildItem -Path Uninstall: | Where-Object -FilterScript { $_.GetValue("DisplayName") -like "Win*"} | ForEach-Object -Process { $_.GetValue("UninstallString") }
```

Sin embargo, puede que estas cadenas no se puedan usar directamente desde el símbolo del sistema de Windows PowerShell sin realizar alguna modificación.

### Actualización de aplicaciones de Windows Installer
Para actualizar una aplicación, es necesario conocer su nombre y la ruta de acceso al paquete de actualización de la aplicación. Con esa información, podrá actualizar una aplicación con un solo comando de Windows PowerShell:

```
(Get-WmiObject -Class Win32_Product -ComputerName . -Filter "Name='OldAppName'").Upgrade(\\AppSrv\dsp\OldAppUpgrade.msi)
```



<!--HONumber=May16_HO2-->


