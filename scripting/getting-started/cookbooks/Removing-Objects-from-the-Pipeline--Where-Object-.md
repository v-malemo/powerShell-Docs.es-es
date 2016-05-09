---
title: Quitar objetos de la canalización (Where-Object)
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 01df8b22-2d22-4e2c-a18d-c004cd3cc284
---
# Quitar objetos de la canalización (Where-Object)
En Windows PowerShell, se suelen generar y pasar más objetos de los deseados a una canalización. Puede especificar las propiedades de los objetos concretos que quiere que se muestren mediante los cmdlets **Format**, pero esto no ayuda a resolver el problema de eliminación de objetos completos de la presentación. Es posible que quiera filtrar objetos antes del final de una canalización, para poder realizar acciones solo en un subconjunto de los objetos generados inicialmente.

Windows PowerShell incluye cmdlet **Where-Object** que permite probar cada objeto de la canalización y pasarlo solo a la canalización si cumple una condición de prueba determinada. Los objetos que no pasan la prueba se quitan de la canalización. Debe indicar la condición de prueba como el valor del parámetro **Where-ObjectFilterScript**.

### Realizar pruebas simples con Where-Object
El valor de **FilterScript** es un *bloque de script*, es decir, uno o más comandos de Windows PowerShell entre llaves ({}), que se evalúa como true o false. Estos bloques de script pueden ser muy simples, pero su creación requiere el conocimiento de otro concepto de Windows PowerShell: los operadores de comparación. Un operador de comparación compara los elementos que aparecen en cada uno de sus lados. Los operadores de comparación comienzan con un carácter '-' seguido de un nombre. Los operadores de comparación básicos funcionan en casi todos los tipos de objeto. Es posible que los operadores de comparación más avanzados solo funcionen en texto o matrices.

> [!NOTE]
> De forma predeterminada, al trabajar con texto, los operadores de comparación de Windows PowerShell no distinguen mayúsculas de minúsculas.

Debido a consideraciones de análisis, los símbolos como <, > y = no se usan como operadores de comparación. En su lugar, los operadores de comparación están formados por letras. Los operadores lógicos básicos se muestran en la tabla siguiente.

|Operador de comparación|Significado|Ejemplo (devuelve true)|
|-----------------------|-----------|--------------------------|
|-eq|es igual a|1 -eq 1|
|-ne|No es igual a|1 -ne 2|
|-lt|Es menor que|1 -lt 2|
|-le|Es menor o igual que|1 -le 2|
|-gt|Es mayor que|2 -gt 1|
|-ge|Es mayor o igual que|2 -ge 1|
|-like|Es como (comparación de comodín para texto)|"file.doc" -like "f*.do?"|
|-notlike|No es como (comparación de comodín para texto)|"file.doc" -notlike "p*.doc"|
|-contains|Contiene|1,2,3 -contains 1|
|-notcontains|No contiene|1,2,3 -notcontains 4|

Los bloques de script Where-Object usan la variable especial '$_' para hacer referencia al objeto actual en la canalización. A continuación se incluye un ejemplo de cómo funciona: Si tiene una lista de números y solo quiere que se devuelvan los que sean inferiores a 3, puede usar Where-Object para filtrar los números. Para ellos, escriba:

```
PS> 1,2,3,4 | Where-Object -FilterScript {$_ -lt 3}
1
2
```

### Filtrar por las propiedades de objeto
Dado que $_ hace referencia al objeto de canalización actual, podemos acceder a sus propiedades para nuestras pruebas.

Por ejemplo, podemos observar la clase Win32_SystemDriver en WMI. Puede haber cientos de controladores del sistema en un determinado sistema, pero puede que solo esté interesado en un conjunto concreto de estos controladores, como, por ejemplo, aquellos que se están ejecutando actualmente. Si usa Get\Member para ver los miembros de Win32_SystemDriver (**Get-WmiObject -Class Win32_SystemDriver | Get-Member -MemberType Property**), verá que la propiedad correspondiente es State y que tiene un valor "Running" cuando se ejecuta el controlador. Para filtrar los controladores del sistema seleccione solo los que se estén ejecutando. Para ello, escriba:

```
Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript {$_.State -eq "Running"}
```

Esto sigue produciendo una larga lista. Es posible que también quiera filtrar para seleccionar solo los controladores configurados para iniciarse automáticamente al probar el valor StartMode:

```
PS> Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript {$_.State -eq "Running"} | Where-Object -FilterScript {$_.StartMode -eq "Auto"}

DisplayName : RAS Asynchronous Media Driver
Name        : AsyncMac
State       : Running
Status      : OK
Started     : True

DisplayName : Audio Stub Driver
Name        : audstub
State       : Running
Status      : OK
Started     : True
```

Esto nos da mucha información que ya no necesitamos porque sabemos que los controladores se están ejecutando. De hecho, los únicos datos que probablemente necesitamos en este momento son el nombre y el nombre para mostrar. El siguiente comando solo incluye esas dos propiedades, lo que produce una salida mucho más simple:

```
PS> Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript {$_.State -eq "Running"} | Where-Object -FilterScript {$_.StartMode -eq "Manual"} | Format-Table -Property Name,DisplayName

Name                                    DisplayName
----                                    -----------
AsyncMac                                RAS Asynchronous Media Driver
Fdc                                     Floppy Disk Controller Driver
Flpydisk                                Floppy Disk Driver
Gpc                                     Generic Packet Classifier
IpNat                                   IP Network Address Translator
mouhid                                  Mouse HID Driver
MRxDAV                                  WebDav Client Redirector
mssmbios                                Microsoft System Management BIOS Driver
```

Hay dos elementos Where-Object en el comando anterior, pero puede expresarse en un único elemento Where-Object mediante el operador lógico -and, de manera similar a la siguiente:

```
Get-WmiObject -Class Win32_SystemDriver | Where-Object -FilterScript { ($_.State -eq "Running") -and ($_.StartMode -eq "Manual") } | Format-Table -Property Name,DisplayName
```

Los operadores lógicos estándar se muestran en la tabla siguiente.

|Operador lógico|Significado|Ejemplo (devuelve true)|
|--------------------|-----------|--------------------------|
|-and|Lógico and; true si ambos lados son true|(1 -eq 1) -and (2 -eq 2)|
|-or|Lógico or; true si algún lado es true|(1 -eq 1) -or (1 -eq 2)|
|-not|Lógico not; invierte true y false|-not (1 -eq 2)|
|\!|Lógico not; invierte true y false|!(1 -eq 2)|



<!--HONumber=Apr16_HO1-->


