---
title: Requisitos del sistema de Windows PowerShell
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 6d1d3c75-3be4-4fc9-8805-ca9b2c454d42
translationtype: Human Translation
ms.sourcegitcommit: 1ae9150b226147c039acf0738690de4da8686a71
ms.openlocfilehash: e2e129c1c90ab7561861a7d9c71fb654569d5712

---

# Requisitos del sistema de Windows PowerShell
En este tema se enumeran los requisitos del sistema de Windows PowerShell 3.0 y Windows PowerShell 4.0, así como de características especiales, como Entorno de scripting integrado (ISE) de Windows PowerShell, comandos CIM y flujos de trabajo.

Windows® 8.1 y Windows Server® 2012 R2 incluyen todos los programas necesarios. Este tema está diseñado para usuarios de versiones anteriores de Windows.

## Requisitos del sistema operativo
Windows PowerShell 4.0 se ejecuta en las siguientes versiones de Windows.

-   Windows 8.1, instalado de manera predeterminada

-   Windows Server 2012 R2, instalado de manera predeterminada

-   Windows® 7 con Service Pack 1: instale [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkId=293881) para ejecutar Windows PowerShell 4.0

-   Windows Server® 2008 R2 con Service Pack 1: instale [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkId=293881) para ejecutar Windows PowerShell 4.0

Windows PowerShell 3.0 se ejecuta en las siguientes versiones de Windows.

-   Windows 8, instalado de manera predeterminada

-   Windows Server 2012, instalado de manera predeterminada

-   Windows® 7 con Service Pack 1: instale [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) para ejecutar Windows PowerShell 3.0

-   Windows Server® 2008 R2 con Service Pack 1: instale [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) para ejecutar Windows PowerShell 3.0

-   Windows Server 2008 con Service Pack 2: instale [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) para ejecutar Windows PowerShell 3.0

## Requisitos de Microsoft .NET Framework
Windows PowerShell 4.0 requiere la instalación completa de Microsoft .NET Framework 4.5. Windows 8.1 y Windows Server 2012 R2 incluyen Microsoft .NET Framework 4.5 de manera predeterminada.

Windows PowerShell 3.0 requiere la instalación completa de Microsoft .NET Framework 4. Windows 8 y Windows Server 2012 incluyen Microsoft .NET Framework 4.5 de manera predeterminada, con lo que se cumple este requisito.

Para instalar Microsoft .NET Framework 4.5 (dotNetFx45\_Full\_setup.exe), consulte [Microsoft .NET Framework 4.5](http://go.microsoft.com/fwlink/?LinkID=242919) en el Centro de descarga de Microsoft.

Para realizar la instalación completa de Microsoft .NET Framework 4 (dotNetFx40\_Full\_setup.exe), consulte [Microsoft .NET Framework 4 (Instalador web)](http://go.microsoft.com/fwlink/?LinkID=212931) en el Centro de descarga de Microsoft.

## WS\-Management 3.0
Windows PowerShell 3.0 y Windows PowerShell 4.0 requieren WS\-Management 3.0, que admite el servicio WinRM y el protocolo WSMan. Este programa está incluido en Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows Management Framework 4.0 y Windows Management Framework 3.0.

## Instrumental de administración de Windows 3.0
Windows PowerShell 3.0 y Windows PowerShell 4.0 requiere Instrumental de administración de Windows 3.0 (WMI). Este programa está incluido en Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows Management Framework 4.0 y Windows Management Framework 3.0. Si este programa no está instalado en el equipo, las características que requieren WMI, como los comandos CIM, no se ejecutan.

## Common Language Runtime 4.0
Windows PowerShell 3.0 y Windows PowerShell 4.0 se compilan en Common Language Runtime (CLR) 4.0.

## Requisitos de la interfaz gráfica de usuario
Windows PowerShell es una aplicación basada en consola que no requiere una interfaz gráfica de usuario. Por lo tanto, es idónea para equipos que no tienen pantallas, monitores ni interfaz de usuario, como las opciones de instalación de Server Core de Windows Server 2012 R2 o Windows Server 2012.

Sin embargo, algunos elementos, como los siguientes, requieren una interfaz gráfica de usuario. Para obtener más información, vea el tema de ayuda de cada elemento.

-   Entorno de scripting integrado (ISE) de Windows PowerShell

-   Cmdlets

    1.  [Out-Gridview](https://technet.microsoft.com/en-us/library/70915a86-d753-464e-8349-cba02316154c)

    2.  [Show-Command](https://technet.microsoft.com/en-us/library/65bba50b-91a8-49d5-80a2-a30fc684ba41)

    3.  [Show-ControlPanelItem](https://technet.microsoft.com/en-us/library/0685d42c-37cc-498f-acf6-0ecfeb0cb162)

    4.  [Show-EventLog](https://technet.microsoft.com/en-us/library/a3b0f5ad-0438-42c7-915b-d1b4793a431c)

-   Parámetros

    1.  Parámetro **ShowWindow** del cmdlet [Get-Help](https://technet.microsoft.com/en-us/library/1f46eeb4-49d7-4bec-bb29-395d9b42f54a).

    2.  Parámetro **ShowSecurityDescriptorUI** de los cmdlets [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) y [Set-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/b21fbad3-1759-4260-b206-dcb8431cd6ea).

## Requisitos del motor de Windows PowerShell
Windows PowerShell 4.0 está diseñado para ser compatible con versiones anteriores de Windows PowerShell 3.0 y Windows PowerShell 2.0. Los cmdlets, proveedores, complementos, módulos y scripts escritos para Windows PowerShell 2.0 y Windows PowerShell 3.0 se ejecutan sin cambios en Windows PowerShell 4.0.

Sin embargo, debido a un cambio en la directiva de activación de tiempo de ejecución en Microsoft .NET Framework 4, los programas de host de Windows PowerShell escritos para Windows PowerShell 2.0 y compilados con Common Language Runtime (CLR) 2.0 no se pueden ejecutar sin modificaciones en versiones posteriores de Windows PowerShell 3.0, que está compilado con CLR 4.0.

El motor de Windows PowerShell 2.0 requiere Microsoft .NET Framework 2.0.50727 como mínimo. Este requisito se cumple mediante Microsoft .NET Framework 3.5 Service Pack 1. Este requisito no se cumple con Microsoft .NET Framework 4 y versiones posteriores de Microsoft .NET Framework.

Para información sobre cómo agregar o instalar el motor de Windows PowerShell 2.0 y agregar o instalar las versiones necesarias de Microsoft .NET Framework, consulte [Instalación del motor de Windows PowerShell 2.0](Installing-the-Windows-PowerShell-2.0-Engine.md). Para obtener información sobre cómo iniciar el motor de Windows PowerShell 2.0, consulte [Iniciar el motor de Windows PowerShell 2.0](Starting-the-Windows-PowerShell-2.0-Engine.md).

## Entorno de preinstalación de Windows
Windows PowerShell 2.0, Windows PowerShell 3.0 y Windows PowerShell 4.0 se ejecutan en el Entorno de preinstalación de Windows (Windows PE). Sin embargo, no se admiten los siguientes cmdlets.

-   [Cmdlets del Servicio de transferencia inteligente en segundo plano (BITS)](http://go.microsoft.com/fwlink/?LinkId=257514)

-   [Get-EventLog](https://technet.microsoft.com/en-us/library/b4985b11-82bf-487d-928d-becd96fc0419)

-   [Get-WinEvent](https://technet.microsoft.com/en-us/library/5fe94870-ed6b-4ce2-9500-93846cc65c95)

-   [Save-Help](https://technet.microsoft.com/en-us/library/aed94f90-b73f-4e25-a25d-7c18d9f161fa)

-   [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545)

Además, el servicio **WinRM** no está presente en Windows PE.

## Consulte también
[Introducción a Windows PowerShell](../getting-started/Getting-Started-with-Windows-PowerShell.md)

[Instalación de Windows PowerShell](Installing-Windows-PowerShell.md)

[Iniciar Windows PowerShell](https://technet.microsoft.com/en-us/library/8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)




<!--HONumber=Jun16_HO4-->


