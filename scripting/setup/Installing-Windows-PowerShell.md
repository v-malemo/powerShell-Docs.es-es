---
title:  Instalar Windows PowerShell
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  6fbb0409-5a54-48ec-95e6-7f8b7d8c4969
---

# Instalación de Windows PowerShell
Windows® 8 y Windows Server® 2012 incluyen Windows PowerShell 3.0 y todos los requisitos previos. El sistema también incluye el motor de Windows PowerShell 2.0 para mantener la compatibilidad con versiones anteriores de los programas host que no pueden usar Windows PowerShell 3.0.

En este tema se explica cómo instalar Windows PowerShell 3.0 en sistemas anteriores e instalar y habilitar las características necesarias.

Este tema incluye las siguientes secciones:

-   [Instalar Windows PowerShell en Windows 8 y Windows Server 2012](Installing-Windows-PowerShell.md#BKMK_InstallingOnWindows8andWindowsServer2012)

-   [Instalar Windows PowerShell en Windows 7 y Windows Server 2008 R2](Installing-Windows-PowerShell.md#BKMK_InstallingOnWindows7andWindowsServer2008R2)

-   [Instalar Windows PowerShell en Windows Server 2008](Installing-Windows-PowerShell.md#BKMK_InstallingOnWindowsServer2008LH)

-   [Instalar Windows PowerShell en Server Core](Installing-Windows-PowerShell.md#BKMK_InstallingOnServerCore)

-   [Implementar Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/639d0eff-98a3-4124-b52c-26921ebd98b0)

-   [Instalar el motor de Windows PowerShell 2.0](Installing-the-Windows-PowerShell-2.0-Engine.md)

## <a name="BKMK_InstallingOnWindows8andWindowsServer2012"></a>Instalar Windows PowerShell en Windows 8 y Windows Server 2012
Windows PowerShell 3.0 se entrega instalado, configurado y listo para usar. Windows PowerShell Integrated Scripting Environment (ISE) está instalado y habilitado. Para más información sobre cómo iniciar Windows PowerShell, vea [Starting Windows PowerShell on Windows 8](https://technet.microsoft.com/en-us/library/d7be1668-8617-4890-ad90-dd9765fbd2c3) (Iniciar Windows PowerShell en Windows 8) y [Starting Windows PowerShell on Windows Server 2012](https://technet.microsoft.com/library/hh831491.aspx#BKMK_powershell) (Iniciar Windows PowerShell en Windows Server 2012).

## <a name="BKMK_InstallingOnWindows7andWindowsServer2008R2"></a>Instalar Windows PowerShell en Windows 7 y Windows Server 2008 R2
En estas instrucciones se explica cómo instalar Windows PowerShell 3.0 en equipos que ejecutan Windows 7 con Service Pack 1 y Windows Server 2008 R2 con Service Pack 1. Se ofrecen instrucciones de instalación independientes a continuación para equipos que ejecutan la opción de instalación Server Core de Windows Server 2008 R2.

#### Preparativos para la instalación

-   Antes de instalar Windows Management Framework 3.0, desinstale todas las versiones anteriores de Windows Management Framework 3.0.

#### Para instalar Windows PowerShell 3.0

1.  Realice la instalación completa de Microsoft .NET Framework 4 (dotNetFx40\_Full\_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=212547](http://go.microsoft.com/fwlink/?LinkID=212547).

    O bien, instale Microsoft .NET Framework 4.5 (dotNetFx45\_Full\_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=242919](http://go.microsoft.com/fwlink/?LinkID=242919).

2.  Instale Windows Management Framework 3.0 desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=240290](http://go.microsoft.com/fwlink/?LinkID=240290).

Para más información sobre cómo iniciar Windows PowerShell 3.0, vea [Starting Windows PowerShell on Earlier Versions of Windows](Starting-Windows-PowerShell-on-Earlier-Versions-of-Windows.md) (Iniciar Windows PowerShell en versiones anteriores de Windows).

## <a name="BKMK_InstallingOnServerCore"></a>Instalar Windows PowerShell en Server Core
En estas instrucciones se explica cómo instalar Windows PowerShell 3.0 en equipos con la opción de instalación Server Core de Windows Server 2008 R2 con Service Pack 1.

Los primeros pasos del procedimiento usan comandos de Administración y mantenimiento de imágenes de implementación (DISM) para instalar Microsoft .NET Framework 2.0 para Server Core y Windows PowerShell 2.0. Estos programas son requisitos previos para Windows Management Framework 3.0, que se instala en un paso posterior.

#### Preparativos para la instalación

-   Antes de instalar Windows Management Framework 3.0, desinstale todas las versiones anteriores de Windows Management Framework 3.0.

#### Para instalar Windows PowerShell 3.0

1.  Iniciar Cmd.exe

2.  Ejecute los siguientes comandos DISM. Estos comandos instalan .NET Framework 2.0 y Windows PowerShell 2.0.

    ```
    dism /online /enable-feature:NetFx2-ServerCore
    dism /online /enable-feature:MicrosoftWindowsPowerShell
    dism /online /enable-feature:NetFx2-ServerCore-WOW64
    ```

3.  Realice la instalación completa de Microsoft .NET Framework 4.0 para Server Core desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=248450](http://go.microsoft.com/fwlink/?LinkID=248450).

4.  Instale Windows Management Framework 3.0 desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=240290](http://go.microsoft.com/fwlink/?LinkID=240290).

## <a name="BKMK_InstallingOnWindowsServer2008LH"></a>Instalar Windows PowerShell en Windows Server 2008
En estas instrucciones se explica cómo instalar Windows PowerShell 3.0 en equipos que ejecutan Windows Server 2008 con Service Pack 2.

En los sistemas Windows Server 2008, Windows Management Framework (Windows PowerShell 2.0, KB968930) es un requisito previo para Windows Management Framework 3.0. La característica de "protección ampliada para la autenticación" protege el equipo frente a ataques de reenvío de autenticación y le permite usar el parámetro **UseSSL** al crear sesiones remotas. Para instalar Windows PowerShell 3.0 y el motor de Windows PowerShell 2.0, utilice el procedimiento siguiente.

#### Preparativos para la instalación

-   Antes de instalar Windows Management Framework 3.0, desinstale todas las versiones anteriores de Windows Management Framework 3.0.

#### Para instalar Windows PowerShell 3.0

1.  Instale Microsoft .NET Framework 3.5 con Service Pack 1 desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=242910](http://go.microsoft.com/fwlink/?LinkID=242910).

2.  Instale Windows Management Framework (Windows PowerShell 2.0, KB 968930) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkId=243035](http://go.microsoft.com/fwlink/?LinkId=243035).

3.  Realice la instalación completa de Microsoft .NET Framework 4 (dotNetFx40\_Full\_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=212547](http://go.microsoft.com/fwlink/?LinkID=212547).

    O bien, instale Microsoft .NET Framework 4.5 (dotNetFx45\_Full\_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=242919](http://go.microsoft.com/fwlink/?LinkID=242919).

4.  Instale la característica de "protección ampliada para la autenticación" (KB 968389) desde [http://go.microsoft.com/fwlink/?LinkID=186398](http://go.microsoft.com/fwlink/?LinkID=186398).

5.  Instale Windows Management Framework 3.0 desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=240290](http://go.microsoft.com/fwlink/?LinkID=240290).

## Consulte también
[Requisitos del sistema de Windows PowerShell](Windows-PowerShell-System-Requirements.md)
[Iniciar Windows PowerShell [ps]](https://technet.microsoft.com/en-us/library/8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)


<!--HONumber=Jun16_HO3-->


