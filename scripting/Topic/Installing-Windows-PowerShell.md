---
title: Instalar Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fbb0409-5a54-48ec-95e6-7f8b7d8c4969
---
# Instalar Windows PowerShell
[!INCLUDE[win8_client_1](../Token/win8_client_1_md.md)] y [!INCLUDE[win8_server_1](../Token/win8_server_1_md.md)] incluyen [!INCLUDE[psversion3](../Token/psversion3_md.md)] y todos sus requisitos previos. El sistema también incluye el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] para mantener la compatibilidad con versiones anteriores de los programas host que no pueden usar [!INCLUDE[psversion3](../Token/psversion3_md.md)].

En este tema se explica cómo instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)] en sistemas anteriores e instalar y habilitar las características necesarias.

Este tema incluye las siguientes secciones:

-   [Instalar Windows PowerShell en Windows 8 y Windows Server 2012](../Topic/Installing-Windows-PowerShell.md#BKMK_InstallingOnWindows8andWindowsServer2012)

-   [Instalar Windows PowerShell en Windows 7 y Windows Server 2008 R2](../Topic/Installing-Windows-PowerShell.md#BKMK_InstallingOnWindows7andWindowsServer2008R2)

-   [Instalar Windows PowerShell en Windows Server 2008](../Topic/Installing-Windows-PowerShell.md#BKMK_InstallingOnWindowsServer2008LH)

-   [Instalar Windows PowerShell en Server Core](../Topic/Installing-Windows-PowerShell.md#BKMK_InstallingOnServerCore)

-   [Implementar Windows PowerShell Web Access](assetId:///639d0eff-98a3-4124-b52c-26921ebd98b0)

-   [Instalar el motor de Windows PowerShell 2.0](../Topic/Installing-the-Windows-PowerShell-2.0-Engine.md)

## <a name="BKMK_InstallingOnWindows8andWindowsServer2012"></a>Instalar Windows PowerShell en Windows 8 y Windows Server 2012
[!INCLUDE[psversion3](../Token/psversion3_md.md)] se entrega instalado, configurado y listo para usar. [!INCLUDE[mshgraphicalhost](../Token/mshgraphicalhost_md.md)] está instalado y habilitado. Para obtener información acerca de cómo iniciar [!INCLUDE[mshshort](../Token/mshshort_md.md)], consulte los temas sobre cómo [iniciar Windows PowerShell en Windows 8](assetId:///d7be1668-8617-4890-ad90-dd9765fbd2c3) e [iniciar Windows PowerShell en Windows Server 2012](assetId:///4fc0110a-cc0c-42a4-bbb5-3cc89a0fc968).

## <a name="BKMK_InstallingOnWindows7andWindowsServer2008R2"></a>Instalar Windows PowerShell en Windows 7 y Windows Server 2008 R2
Estas instrucciones explican cómo instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)] en equipos que ejecutan [!INCLUDE[win7_client_secondref](../Token/win7_client_secondref_md.md)] con Service Pack 1 y [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)] con Service Pack 1. Se ofrecen instrucciones de instalación independientes a continuación para equipos que ejecutan la opción de instalación Server Core de [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)].

#### Preparativos para la instalación

-   Antes de instalar [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)], desinstale todas las versiones anteriores de Windows Management Framework 3.0.

#### Para instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)]

1.  Realice la instalación completa de Microsoft .NET Framework 4 (dotNetFx40_Full_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=212547](http://go.microsoft.com/fwlink/?LinkID=212547).

    O bien, instale Microsoft .NET Framework 4.5 (dotNetFx45_Full_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=242919](http://go.microsoft.com/fwlink/?LinkID=242919).

2.  Instale [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)] desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=240290](http://go.microsoft.com/fwlink/?LinkID=240290).

Para obtener información sobre cómo iniciar [!INCLUDE[psversion3](../Token/psversion3_md.md)], consulte [Iniciar Windows PowerShell en versiones anteriores de Windows](../Topic/Starting-Windows-PowerShell-on-Earlier-Versions-of-Windows.md).

## <a name="BKMK_InstallingOnServerCore"></a>Instalar Windows PowerShell en Server Core
Estas instrucciones explican cómo instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)] en equipos con la opción de instalación Server Core [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)] con Service Pack 1.

Los primeros pasos del procedimiento usan comandos de Administración y mantenimiento de imágenes de implementación (DISM) para instalar Microsoft [!INCLUDE[dnprdnlong](../Token/dnprdnlong_md.md)] para Server Core y [!INCLUDE[psversion2](../Token/psversion2_md.md)]. Estos programas son requisitos previos para [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)], que se instala en un paso posterior.

#### Preparativos para la instalación

-   Antes de instalar [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)], desinstale todas las versiones anteriores de Windows Management Framework 3.0.

#### Para instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)]

1.  Iniciar Cmd.exe

2.  Ejecute los siguientes comandos DISM. Estos comandos instalan [!INCLUDE[dnprdnlong](../Token/dnprdnlong_md.md)] y [!INCLUDE[psversion2](../Token/psversion2_md.md)].

    ```
    dism /online /enable-feature:NetFx2-ServerCore
    dism /online /enable-feature:MicrosoftWindowsPowerShell
    dism /online /enable-feature:NetFx2-ServerCore-WOW64
    ```

3.  Realice la instalación completa de Microsoft [!INCLUDE[netfx40_short](../Token/netfx40_short_md.md)] para Server Core desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=248450](http://go.microsoft.com/fwlink/?LinkID=248450).

4.  Instale [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)] desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=240290](http://go.microsoft.com/fwlink/?LinkID=240290).

## <a name="BKMK_InstallingOnWindowsServer2008LH"></a>Instalar Windows PowerShell en Windows Server 2008
En estas instrucciones se explica cómo instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)] en equipos que ejecutan [!INCLUDE[lserver](../Token/lserver_md.md)] con Service Pack 2.

En los sistemas [!INCLUDE[lserver](../Token/lserver_md.md)], Windows Management Framework ([!INCLUDE[psversion2](../Token/psversion2_md.md)], KB 968930) es un requisito previo para [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)]. La característica de "protección ampliada para la autenticación" protege el equipo frente a ataques de reenvío de autenticación y le permite usar el parámetro **UseSSL** al crear sesiones remotas. Use el siguiente procedimiento para instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)] y el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

#### Preparativos para la instalación

-   Antes de instalar [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)], desinstale todas las versiones anteriores de Windows Management Framework 3.0.

#### Para instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)]

1.  Instale Microsoft .NET Framework 3.5 con Service Pack 1 desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=242910](http://go.microsoft.com/fwlink/?LinkID=242910).

2.  Instale Windows Management Framework ([!INCLUDE[psversion2](../Token/psversion2_md.md)], KB 968930) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkId=243035](http://go.microsoft.com/fwlink/?LinkId=243035).

3.  Realice la instalación completa de Microsoft .NET Framework 4 (dotNetFx40_Full_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=212547](http://go.microsoft.com/fwlink/?LinkID=212547).

    O bien, instale Microsoft .NET Framework 4.5 (dotNetFx45_Full_setup.exe) desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=242919](http://go.microsoft.com/fwlink/?LinkID=242919).

4.  Instale la característica de "protección ampliada para la autenticación" (KB 968389) desde [http://go.microsoft.com/fwlink/?LinkID=186398](http://go.microsoft.com/fwlink/?LinkID=186398).

5.  Instale [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)] desde el Centro de descarga de Microsoft en [http://go.microsoft.com/fwlink/?LinkID=240290](http://go.microsoft.com/fwlink/?LinkID=240290).

## Consulte también
[Requisitos del sistema de Windows PowerShell](../Topic/Windows-PowerShell-System-Requirements.md)
[Iniciar Windows PowerShell [ps]](assetId:///8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)



<!--HONumber=Apr16_HO1-->


