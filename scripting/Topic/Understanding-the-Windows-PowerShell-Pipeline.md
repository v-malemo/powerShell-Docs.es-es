---
title: Descripción de la canalización de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6be50926-7943-4ef7-9499-4490d72a63fb
---
# Descripción de la canalización de Windows PowerShell
La canalización funciona prácticamente en cualquier lugar en Windows PowerShell. Aunque se ve texto en la pantalla, Windows PowerShell no canaliza texto entre los comandos. En su lugar, canaliza objetos.

La notación usada para las canalizaciones es similar a la que se usa en otros shells, por lo que, a primera vista, puede que no sea evidente que Windows PowerShell presenta algo nuevo. Por ejemplo, si usa el cmdlet **Out-Host** para forzar una presentación página por página de la salida de otro comando, la salida tiene el aspecto del texto normal que se muestra en la pantalla, dividida en páginas:

```
PS> Get-ChildItem -Path C:\WINDOWS\System32 | Out-Host -Paging

    Directory: Microsoft.Windows PowerShell.Core\FileSystem::C:\WINDOWS\system32

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        2005-10-22  11:04 PM        315 $winnt$.inf
-a---        2004-08-04   8:00 AM      68608 access.cpl
-a---        2004-08-04   8:00 AM      64512 acctres.dll
-a---        2004-08-04   8:00 AM     183808 accwiz.exe
-a---        2004-08-04   8:00 AM      61952 acelpdec.ax
-a---        2004-08-04   8:00 AM     129536 acledit.dll
-a---        2004-08-04   8:00 AM     114688 aclui.dll
-a---        2004-08-04   8:00 AM     194048 activeds.dll
-a---        2004-08-04   8:00 AM     111104 activeds.tlb
-a---        2004-08-04   8:00 AM       4096 actmovie.exe
-a---        2004-08-04   8:00 AM     101888 actxprxy.dll
-a---        2003-02-21   6:50 PM     143150 admgmt.msc
-a---        2006-01-25   3:35 PM      53760 admparse.dll
<SPACE> next page; <CR> next line; Q quit
...
```

El comando Out-Host -Paging es un elemento de la canalización útil siempre que tiene una salida larga que le gustaría mostrar lentamente. Es especialmente útil si la operación consume una gran cantidad de CPU. Dado que el procesamiento se transfiere al cmdlet Out-Host cuando tiene una página completa lista para mostrar, los cmdlets que lo preceden en la canalización detienen la operación hasta que la siguiente página de salida esté disponible. Puede verlo si usa el Administrador de tareas de Windows para supervisar el uso de la CPU y la memoria de Windows PowerShell.

Ejecute el siguiente comando: **Get-ChildItem C:\Windows -Recurse**. Compare el uso de la CPU y la memoria con este comando: **Get-ChildItem C:\Windows -Recurse | Out-Host -Paging**. Lo que ve en la pantalla es texto, lo que se debe a la necesidad de representar objetos como texto en una ventana de la consola. Se trata simplemente de una representación de lo que sucede realmente en Windows PowerShell. Por ejemplo, considere el cmdlet Get-Location. Si escribe **Get-Location** mientras la ubicación actual es la raíz de la unidad C, verá la siguiente salida:

```
PS> Get-Location

Path
----
C:\
```

Si Windows PowerShell canaliza texto, al emitir un comando como **Get-Location | Out-Host**, pasará de **Get-Location** a **Out-Host** un conjunto de caracteres en el orden en que se muestran en la pantalla. En otras palabras, si fuera a ignorar la información de encabezado, **Out-Host** recibiría primero el carácter '**C'**, luego el carácter '**:'** y, finalmente, el carácter '**\'**. El cmdlet **Out-Host** no pudo determinar el significado que debía asociarse a los caracteres emitidos por el cmdlet **Get-Location**.

En lugar de utilizar texto para permitir que los comandos de una canalización se comuniquen, Windows PowerShell usa objetos. Desde el punto de vista de un usuario, los objetos empaquetan información relacionada en un formulario que facilita la manipulación de la información como una unidad y la extracción de los elementos específicos que necesita.

El comando **Get-Location** no devuelve el texto que contenga la ruta de acceso actual. Devuelve un conjunto de información denominado objeto **PathInfo** que contiene la ruta de acceso actual junto con otra información. El cmdlet **Out-Host** envía luego este objeto **PathInfo** a la pantalla y Windows PowerShell decide qué información se mostrará y cómo se mostrará en función de sus reglas de formato.

De hecho, la información de encabezado generada por el cmdlet **Get-Location** se agrega solo al final del proceso, como parte del proceso de formato de los datos para su presentación en pantalla. Lo que ve en la pantalla es un resumen de la información y no una representación completa del objeto de salida.

Dado que puede haber más información devuelta por un comando de Windows PowerShell de la que vemos en la ventana de la consola, ¿cómo podemos recuperar los elementos no visibles? ¿Cómo se pueden ver los datos adicionales? ¿Qué ocurre si quiere ver los datos en un formato diferente al que suele usar Windows PowerShell?

En lo que queda de este capítulo explicaremos cómo puede detectar la estructura de objetos específicos de Windows PowerShell, seleccionar elementos específicos y darles formato para mostrarlos más fácilmente, y cómo enviar esta información a ubicaciones de salida alternativas, como archivos e impresoras.



<!--HONumber=Apr16_HO1-->


