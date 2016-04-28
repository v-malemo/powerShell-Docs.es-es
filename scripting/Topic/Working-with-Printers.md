---
title: Trabajar con impresoras
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f29ead3-f83b-4706-ac3e-f2154ff38dc5
---
# Trabajar con impresoras
Puede usar Windows PowerShell para administrar impresoras mediante WMI y el objeto COM WScript.Network de WSH. Usaremos una combinación de ambas herramientas para demostrar tareas específicas.

### Enumerar las conexiones de impresora
La manera más simple de enumerar las impresoras instaladas en un equipo es usar la clase WMI **Win32_Printer**:

```
Get-WmiObject -Class Win32_Printer -ComputerName
```

También puede enumerar las impresoras mediante el objeto COM **WScript.Network** que suele usarse en los scripts de WSH:

```
(New-Object -ComObject WScript.Network).EnumPrinterConnections()
```

Dado que este comando devuelve una colección de cadenas simple de nombres de puerto y nombres de dispositivo de impresión sin ninguna etiqueta distintiva, no es fácil de interpretar.

### Agregar una impresora de red
Para agregar una nueva impresora de red, use **WScript.Network**:

```
(New-Object -ComObject WScript.Network).AddWindowsPrinterConnection("\\Printserver01\Xerox5")
```

### Establecer una impresora predeterminada
Para usar WMI para establecer la impresora predeterminada, busque la impresora en la colección **Win32_Printer** y, a continuación, invoque el método **SetDefaultPrinter**:

```
(Get-WmiObject -ComputerName . -Class Win32_Printer -Filter "Name='HP LaserJet 5Si'").SetDefaultPrinter()
```

**WScript.Network** es un poco más fácil de usar, porque tiene un método **SetDefaultPrinter** que toma el nombre de la impresora como argumento:

```
(New-Object -ComObject WScript.Network).SetDefaultPrinter('HP LaserJet 5Si')
```

### Quitar una conexión de impresora
Para quitar una conexión de impresora, use el método **WScript.Network RemovePrinterConnection**:

```
(New-Object -ComObject WScript.Network).RemovePrinterConnection("\\Printserver01\Xerox5")
```



<!--HONumber=Apr16_HO1-->


