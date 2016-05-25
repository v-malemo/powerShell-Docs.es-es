---
title:  Administrar la ubicación actual
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  a9f9e7a7-3ea8-47d3-bbb4-6e437f6d4a4a
---

# Administrar la ubicación actual
Al navegar por los sistemas de carpetas en el Explorador de archivos, normalmente tiene una ubicación de trabajo específica; es decir, la carpeta abierta actual. Los elementos de la carpeta actual se pueden manipular fácilmente haciendo clic en ellos. En las interfaces de línea de comandos como Cmd.exe, si está en la misma carpeta que un archivo determinado, puede acceder a este especificando un nombre relativamente corto. No será necesario especificar la ruta de acceso completa al archivo. El directorio actual se conoce como el directorio de trabajo.

Windows PowerShell usa el término **Location** para referirse al directorio de trabajo e implementa una familia de cmdlets para examinar y manipular la ubicación.

### Obtener la ubicación actual (Get-Location)
Para averiguar la ruta de acceso de su ubicación de directorio actual, escriba el comando **Get-Location**:

```
PS> Get-Location
Path
----
C:\Documents and Settings\PowerUser
```

> [!NOTE]
> El cmdlet Get-Location es similar al comando **pwd** en el shell de BASH. El cmdlet Set-Location es similar al comando **cd** en Cmd.exe.

### Configurar la ubicación actual (Set-Location)
El comando **Get-Location** se usa con el comando **Set-Location**. El comando **Set-Location** permite especificar su ubicación de directorio actual.

```
PS> Set-Location -Path C:\Windows
```

Después de escribir el comando, observará que no recibe ningún tipo de información directa sobre el efecto del comando. Casi todos los comandos de Windows PowerShell que realizan una acción apenas si generan resultados, ya que no siempre son útiles. Para comprobar que se ha producido un cambio de directorio correcto al escribir el comando **Set-Location**, incluya el parámetro **-PassThru** cuando escriba el comando **Set-Location**:

```
PS> Set-Location -Path C:\Windows -PassThru
Path
----
C:\WINDOWS
```

El parámetro **-PassThru** se puede usar con muchos comandos Set en Windows PowerShell para devolver información sobre el resultado en aquellos casos en que no haya ninguna salida predeterminada.

Puede especificar rutas relativas a la ubicación actual de la misma manera en que lo haría en la mayoría de los shells de comandos de UNIX y Windows. En la notación estándar de las rutas de acceso relativas, un punto (**.**) representa la carpeta actual y dos puntos (**..**), el directorio principal de su ubicación actual.

Así, si está en la carpeta **C:\Windows**, un punto (**.**) representa a **C:\Windows** y dos puntos (**..**), a **C:**. Puede cambiar su ubicación actual por la raíz de la unidad C:; para ello, escriba:

<pre>PS> Set-Location -Path .. -PassThru Path ---- C:\</pre>

Esta misma técnica funciona en unidades de Windows PowerShell que no son unidades del sistema de archivos, como **HKLM:**. Puede establecer su ubicación en la clave HKLM\Software del Registro escribiendo lo siguiente:

```
PS> Set-Location -Path HKLM:\SOFTWARE -PassThru

Path
----
HKLM:\SOFTWARE
```

Luego, puede cambiar la ubicación del directorio al directorio principal (que es la raíz de la unidad HKLM: de Windows PowerShell) usando una ruta de acceso relativa:

```
PS> Set-Location -Path .. -PassThru

Path
----
HKLM:\
```

Puede escribir Set-Location o usar cualquiera de los alias de Windows PowerShell integrados para Set-Location (cd, chdir, sl). Por ejemplo:

```
cd -Path C:\Windows
```

`chdir -Path .. -PassThru`

```
sl -Path HKLM:\SOFTWARE -PassThru
```

### Guardar y recuperar ubicaciones recientes (Push-Location y Pop-Location)
Al cambiar de ubicación, es útil realizar un seguimiento de dónde ha estado y poder volver a la ubicación anterior. El cmdlet **Push-Location** de Windows PowerShell crea un historial ordenado (una "pila") de las rutas de acceso de directorio donde ha estado, mientras que el cmdlet complementario **Pop-Location** sirve para ir hacia atrás en el historial de las rutas de acceso de directorio.

Por ejemplo, Windows PowerShell suele iniciarse en el directorio principal del usuario.

```
PS> Get-Location

Path
----
C:\Documents and Settings\PowerUser
```

> [!NOTE]
> La palabra *pila* tiene un significado especial en muchas configuraciones de programación, incluido .NET Framework. Al igual que sucede con una pila física de elementos, el último elemento que se coloca en la pila es el primer elemento que se puede extraer de ella. Agregar un elemento a una pila se conoce coloquialmente como "insertar" el elemento en la pila, mientras que extraer un elemento de la pila se suele conocer como "retirar" el elemento de la pila.

Escriba lo siguiente para insertar la ubicación actual en la pila y, después, ir a la carpeta Configuración local:

```
PS> Push-Location -Path "Local Settings"
```

Tras ello, puede escribir lo siguiente para insertar la ubicación de Configuración local en la pila y moverse a la carpeta Temp:

```
PS> Push-Location -Path Temp
```

Si quiere confirmar que ha cambiado de directorio, escriba el comando **Get-Location**:

```
PS> Get-Location

Path
----
C:\Documents and Settings\PowerUser\Local Settings\Temp
```

Tras ello, puede regresar al último directorio en el que ha estado escribiendo el comando **Pop-Location**, así como comprobar el cambio especificando el comando **Get-Location**:

```
PS> Pop-Location
PS> Get-Location

Path
----
C:\Documents and Settings\me\Local Settings
```

Al igual que con el cmdlet **Set-Location**, puede incluir el parámetro **-PassThru** cuando escriba el cmdlet **Pop-Location** para mostrar el directorio que ha especificado:

```
PS> Pop-Location -PassThru

Path
----
C:\Documents and Settings\PowerUser
```

Los cmdlets Location también se pueden usar con rutas de acceso de red. Si tiene un servidor denominado FS01 con un recurso compartido denominado Public, puede cambiar la ubicación escribiendo:

```
Set-Location \\FS01\Public
```

o

```
Push-Location \\FS01\Public
```

Puede usar los comandos **Push-Location** y **Set-Location** para cambiar la ubicación a cualquier unidad disponible. Por ejemplo, si tiene una unidad de CD-ROM local con la letra de unidad D que contiene un CD de datos, puede cambiar la ubicación a dicha unidad de CD escribiendo el comando **Set-Location D:**.

Si la unidad está vacía, obtendrá el siguiente mensaje de error:

```
PS> Set-Location D:
Set-Location : Cannot find path 'D:\' because it does not exist.
```

Cuando se usa una interfaz de línea de comandos, conviene no usar el Explorador de archivos para examinar las unidades de disco físicas disponibles. Además, el Explorador de archivos no le mostrará todas las unidades de disco de Windows PowerShell. Windows PowerShell proporciona un conjunto de comandos para manipular unidades de Windows PowerShell, aspecto que trataremos más adelante.



<!--HONumber=May16_HO2-->


