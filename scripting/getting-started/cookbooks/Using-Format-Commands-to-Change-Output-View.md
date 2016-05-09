---
title: Usar comandos de formato para cambiar la vista de salida
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63515a06-a6f7-4175-a45e-a0537f4f6d05
---
# Usar comandos de formato para cambiar la vista de salida
Windows PowerShell tiene un conjunto de cmdlets que permite controlar las propiedades que se muestran para determinados objetos. Los nombres de todos los cmdlets empiezan con el verbo **Format**. Permiten seleccionar una o más propiedades para mostrar.

Los cmdlets **Format** son **Format-Wide**, **Format-List**, **Format-Table** y **Format-Custom**. Solo describiremos los cmdlets **Format-Wide**, **Format-List** y **Format-Table** en esta guía de usuario.

Cada cmdlet de formato tiene propiedades predeterminadas que se usarán si no indica propiedades específicas para mostrar. Asimismo, cada cmdlet usa el mismo nombre de parámetro, **Property**, para especificar las propiedades que se deben mostrar. Dado que **Format-Wide** solo muestra una propiedad única, su parámetro **Property** solo toma un valor único, pero los parámetros de propiedad de **Format-List** y **Format-Table** aceptarán una lista de nombres de propiedad.

Si usa el comando **Get-Process -Name powershell** con dos instancias de ejecución de Windows PowerShell, obtendrá una salida similar a la siguiente:

```
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    995       9    30308      27996   152     2.73   2760 powershell
    331       9    23284      29084   143     1.06   3448 powershell
```

En el resto de esta sección, exploraremos cómo usar los cmdlets **Format** para cambiar la manera en que se muestra la salida de este comando.

### Usar Format-Wide para la salida de un solo elemento
De forma predeterminada, el cmdlet **Format-Wide** muestra solo la propiedad predeterminada de un objeto. La información asociada a cada objeto se muestra en una sola columna:

```
PS> Get-Process -Name powershell | Format-Wide

powershell                              powershell
```

También puede especificar una propiedad no predeterminada:

```
PS> Get-Process -Name powershell | Format-Wide -Property Id

2760                                    3448
```

#### Controlar la presentación de Format-Wide con Column
Con el cmdlet **Format-Wide**, solo puede mostrar una única propiedad a la vez. Resulta útil para mostrar listas simples que muestren solo un elemento por línea. Para obtener una lista simple, establezca el valor del parámetro **Column** en 1. Para ello, escriba:

```
Get-Command Format-Wide -Property Name -Column 1
```

### Usar Format-List para obtener una vista de lista
El cmdlet **Format-List** muestra un objeto en forma de una lista, donde cada propiedad aparece etiquetada y en una línea diferente:

```
PS> Get-Process -Name powershell | Format-List

Id      : 2760
Handles : 1242
CPU     : 3.03125
Name    : powershell

Id      : 3448
Handles : 328
CPU     : 1.0625
Name    : powershell
```

Puede especificar tantas propiedades como desee:

```
PS> Get-Process -Name powershell | Format-List -Property ProcessName,FileVersion
,StartTime,Id

ProcessName : powershell
FileVersion : 1.0.9567.1
StartTime   : 2006-05-24 13:42:00
Id          : 2760

ProcessName : powershell
FileVersion : 1.0.9567.1
StartTime   : 2006-05-24 13:54:28
Id          : 3448
```

#### Obtener información detallada usando Format-List con caracteres comodín
El cmdlet **Format-List** permite usar un carácter comodín como valor de su parámetro **Property**. Esto le permite visualizar información detallada. A menudo, los objetos incluyen más información de la que necesita, motivo por el cual Windows PowerShell no muestra todos los valores de propiedad de forma predeterminada. Para mostrar todas las propiedades de un objeto, use el comando **Format-List -Property &#42;**. El siguiente comando genera más de 60 líneas de salida para un único proceso:

```
Get-Process -Name powershell | Format-List -Property *
```

Aunque el comando **Format-List** resulta útil para mostrar detalles, si desea una introducción de la salida que incluya muchos elementos, una vista tabular más sencilla suele ser más útil.

### Usar Format-Table para la salida tabular
Si usa el cmdlet **Format-Table** sin ningún nombre de propiedad especificado para formatear la salida del comando **Get-Process**, obtendrá exactamente la misma salida que al aplicar cualquier formato. El motivo es que los procesos se suelen mostrar en formato tabular, como la mayoría de los objetos de Windows PowerShell.

```
PS> Get-Process -Name powershell | Format-Table

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   1488       9    31568      29460   152     3.53   2760 powershell
    332       9    23140        632   141     1.06   3448 powershell
```

#### Mejorar la salida de Format-Table (AutoSize)
Aunque una vista tabular resulta útil para mostrar mucha información comparable, puede ser difícil de interpretar si la presentación es demasiado estrecha para los datos. Por ejemplo, si intenta mostrar la ruta de acceso del proceso, el identificador, el nombre y la empresa, obtendrá una salida truncada para la ruta de acceso del proceso y la columna de la empresa:

```
PS> Get-Process -Name powershell | Format-Table -Property Path,Name,Id,Company

Path                Name                                 Id Company
----                ----                                 -- -------
C:\Program Files... powershell                         2836 Microsoft Corpor...
```

Si especifica el parámetro **AutoSize** al ejecutar el comando **Format-Table**, Windows PowerShell calculará los anchos de columna en función de los datos reales que va a mostrar. Esto hace que la columna **Path** se pueda leer, pero la columna de la empresa sigue estando truncada:

```
PS> Get-Process -Name powershell | Format-Table -Property Path,Name,Id,Company -
AutoSize

Path                                                    Name         Id Company
----                                                    ----         -- -------
C:\Program Files\Windows PowerShell\v1.0\powershell.exe powershell 2836 Micr...
```

El cmdlet **Format-Table** todavía podría truncar datos, pero solo lo hace al final de la pantalla. Las propiedades distintas de la última que se muestra obtienen el tamaño que necesitan para que su elemento de datos más largo se muestre correctamente. Puede ver que el nombre de la empresa es visible, pero la ruta de acceso está truncada si intercambia las ubicaciones de **Path** y **Company** en la lista de valores **Property**:

```
PS> Get-Process -Name powershell | Format-Table -Property Company,Name,Id,Path -
AutoSize

Company               Name         Id Path
-------               ----         -- ----
Microsoft Corporation powershell 2836 C:\Program Files\Windows PowerShell\v1...
```

El comando **Format-Table** supone que, cuanto más cerca está una propiedad del principio de la lista de propiedades, más importante es. Por tanto, intenta mostrar las propiedades lo más cerca posible del principio. Si el comando **Format-Table** no puede mostrar todas las propiedades, quitará algunas columnas de la presentación y mostrará una advertencia. Puede ver este comportamiento si hace que **Name** sea la última propiedad de la lista:

```
PS> Get-Process -Name powershell | Format-Table -Property Company,Path,Id,Name -
AutoSize

WARNING: column "Name" does not fit into the display and was removed.

Company               Path                                                    I
                                                                              d
-------               ----                                                    -
Microsoft Corporation C:\Program Files\Windows PowerShell\v1.0\powershell.exe 6
```

En la salida anterior, la columna ID se ha truncado para caber en la lista y los encabezados de columna están apilados. El cambio automático del tamaño de las columnas no siempre produce el resultado deseado.

#### Ajustar la salida de Format-Table en columnas (Wrap)
Puede forzar los datos de **Format-Table** extensos para ajustar la columna de presentación mediante el parámetro **Wrap**. El parámetro **Wrap** solo no realizará necesariamente la acción prevista, ya que usa la configuración predeterminada si no se especifica también **AutoSize**:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -Property Name,Id,Company,
Path

Name                                 Id Company             Path
----                                 -- -------             ----
powershell                         2836 Microsoft Corporati C:\Program Files\Wi
                                        on                  ndows PowerShell\v1
                                                            .0\powershell.exe
```

Una ventaja de usar el parámetro **Wrap** solo es no ralentiza en exceso el procesamiento. Si realiza una lista de archivo recursiva de un sistema de directorio de gran tamaño, podría tardar mucho tiempo y usar una gran cantidad de memoria antes de mostrar los primeros elementos de salida si usa **AutoSize**.

Si no le preocupa la carga del sistema, **AutoSize** funciona bien con el parámetro **Wrap**. Siempre se asigna el ancho necesario a las columnas iniciales, ya que deben mostrar los elementos en una sola línea, al igual que cuando se especifica **AutoSize** sin el parámetro **Wrap**. La única diferencia es que la última columna se ajustará si es necesario:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Name,I
d,Company,Path

Name         Id Company               Path
----         -- -------               ----
powershell 2836 Microsoft Corporation C:\Program Files\Windows PowerShell\v1.0\
                                      powershell.exe
```

Es posible que algunas columnas no se muestren correctamente si especifica primero las columnas más anchas, por lo que es más seguro especificar los elementos de datos más pequeños en primer lugar. En el siguiente ejemplo, especificamos el elemento de ruta de acceso extremadamente ancho en primer lugar y, a pesar del ajuste, perdemos la columna **Name** final:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Path,I
d,Company,Name

WARNING: column "Name" does not fit into the display and was removed.

Path                                                      Id Company
----                                                      -- -------
C:\Program Files\Windows PowerShell\v1.0\powershell.exe 2836 Microsoft Corporat
                                                             ion
```

#### Organizar la salida de la tabla (-GroupBy)
Otro parámetro útil para el control de la salida tabular es **GroupBy**. Las listas tabulares largas pueden ser especialmente difíciles de comparar. El parámetro **GroupBy** agrupa la salida según un valor de propiedad. Por ejemplo, podemos agrupar procesos por empresa para facilitar la inspección. Para ello, se omite el valor de la empresa de la lista de propiedades:

```
PS> Get-Process -Name powershell | Format-Table -Wrap -AutoSize -Property Name,I
d,Path -GroupBy Company

   Company: Microsoft Corporation

Name         Id Path
----         -- ----
powershell 1956 C:\Program Files\Windows PowerShell\v1.0\powershell.exe
powershell 2656 C:\Program Files\Windows PowerShell\v1.0\powershell.exe
```



<!--HONumber=Apr16_HO1-->


