---
title: Requisitos del sistema de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d1d3c75-3be4-4fc9-8805-ca9b2c454d42
---
# Requisitos del sistema de Windows PowerShell
En este tema se enumeran los requisitos del sistema de [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)], así como de características especiales como [!INCLUDE[mshgraphicalhost](../Token/mshgraphicalhost_md.md)], comandos CIM y flujos de trabajo.

[!INCLUDE[winblue_client_1](../Token/winblue_client_1_md.md)] y [!INCLUDE[winblue_server_1](../Token/winblue_server_1_md.md)] incluyen todos los programas necesarios. Este tema está diseñado para usuarios de versiones anteriores de Windows.

## Requisitos del sistema operativo
[!INCLUDE[psversion4](../Token/psversion4_md.md)] se ejecuta en las siguientes versiones de Windows.

-   [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], instalado de forma predeterminada.

-   [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], instalado de forma predeterminada.

-   [!INCLUDE[win7_client_firstref](../Token/win7_client_firstref_md.md)] con Service Pack 1, instale [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkId=293881) para ejecutar [!INCLUDE[psversion4](../Token/psversion4_md.md)]

-   [!INCLUDE[win7_server_firstref](../Token/win7_server_firstref_md.md)] con Service Pack 1, instale [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkId=293881) para ejecutar [!INCLUDE[psversion4](../Token/psversion4_md.md)].

[!INCLUDE[psversion3](../Token/psversion3_md.md)] se ejecuta en las siguientes versiones de Windows.

-   [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)], instalado de forma predeterminada.

-   [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)], instalado de forma predeterminada.

-   [!INCLUDE[win7_client_firstref](../Token/win7_client_firstref_md.md)] con Service Pack 1, instale [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) para ejecutar [!INCLUDE[psversion3](../Token/psversion3_md.md)].

-   [!INCLUDE[win7_server_firstref](../Token/win7_server_firstref_md.md)] con Service Pack 1, instale [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) para ejecutar [!INCLUDE[psversion3](../Token/psversion3_md.md)].

-   [!INCLUDE[lserver](../Token/lserver_md.md)] con Service Pack 2, instale [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) para ejecutar [!INCLUDE[psversion3](../Token/psversion3_md.md)].

## Requisitos de Microsoft .NET Framework
[!INCLUDE[psversion4](../Token/psversion4_md.md)] requiere la instalación completa de Microsoft .NET Framework 4.5. [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)] y [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)] incluyen Microsoft .NET Framework 4.5 de forma predeterminada.

[!INCLUDE[psversion3](../Token/psversion3_md.md)] requiere la instalación completa de Microsoft .NET Framework 4. [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)] y [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)] incluyen Microsoft .NET Framework 4.5 de forma predeterminada, con lo que cumple este requisito.

Para instalar Microsoft .NET Framework 4.5 (dotNetFx45_Full_setup.exe), consulte [Microsoft .NET Framework 4.5](http://go.microsoft.com/fwlink/?LinkID=242919) en el Centro de descarga de Microsoft.

Para realizar la instalación completa de Microsoft .NET Framework 4 (dotNetFx40_Full_setup.exe), consulte [Microsoft .NET Framework 4 (Web Installer)](http://go.microsoft.com/fwlink/?LinkID=212931) en el Centro de descarga de Microsoft.

## WS-Management 3.0
[!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)] requieren WS-Management 3.0, que admite el servicio WinRM y el protocolo WSMan. Este programa está incluido en [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)], [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)], Windows Management Framework 4.0 y [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)].

## Instrumental de administración de Windows 3.0
[!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)] requieren Instrumental de administración de Windows 3.0 (WMI). Este programa está incluido en [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)], [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)], Windows Management Framework 4.0 y [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)]. Si este programa no está instalado en el equipo, las características que requieren WMI, como los comandos CIM, no se ejecutan.

## Common Language Runtime 4.0
[!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)] se compilan con Common Language Runtime (CLR) 4.0.

## Requisitos de la interfaz gráfica de usuario
[!INCLUDE[wps_2](../Token/wps_2_md.md)] es una aplicación basada en consola que no requiere una interfaz gráfica de usuario. Por lo tanto, es idónea para equipos que no tienen pantallas, monitores ni interfaz de usuario, como las opciones de instalación de Server Core de [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)] o [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)].

Sin embargo, algunos elementos, como los siguientes, requieren una interfaz gráfica de usuario. Para obtener más información, vea el tema de ayuda de cada elemento.

-   [!INCLUDE[mshgraphicalhost](../Token/mshgraphicalhost_md.md)]

-   Cmdlets

    1.  [Out-Gridview](assetId:///70915a86-d753-464e-8349-cba02316154c)

    2.  [Show-Command](assetId:///65bba50b-91a8-49d5-80a2-a30fc684ba41)

    3.  [Show-ControlPanelItem](assetId:///0685d42c-37cc-498f-acf6-0ecfeb0cb162)

    4.  [Show-EventLog](assetId:///a3b0f5ad-0438-42c7-915b-d1b4793a431c)

-   Parámetros

    1.  Parámetro **ShowWindow** del cmdlet [Get-Help](assetId:///1f46eeb4-49d7-4bec-bb29-395d9b42f54a).

    2.  Parámetro **ShowSecurityDescriptorUi** de los cmdlets [Register-PSSessionConfiguration](assetId:///e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) y [Set-PSSessionConfiguration](assetId:///b21fbad3-1759-4260-b206-dcb8431cd6ea).

## Requisitos del motor de Windows PowerShell
[!INCLUDE[psversion4](../Token/psversion4_md.md)] está diseñado para ser compatible con versiones anteriores de [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion2](../Token/psversion2_md.md)]. Los cmdlets, proveedores, complementos, módulos y scripts escritos para [!INCLUDE[psversion2](../Token/psversion2_md.md)] y [!INCLUDE[psversion3](../Token/psversion3_md.md)] se ejecutan sin cambios en [!INCLUDE[psversion4](../Token/psversion4_md.md)].

Sin embargo, debido a un cambio en la directiva de activación en tiempo de ejecución de Microsoft .NET Framework 4, los programas host de [!INCLUDE[wps_2](../Token/wps_2_md.md)] escritos para [!INCLUDE[psversion2](../Token/psversion2_md.md)] y compilados con Common Language Runtime (CLR) 2.0 no se puede ejecutar sin modificaciones en [!INCLUDE[psversion3](../Token/psversion3_md.md)], que está compilado con CLR 4.0.

El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] requiere Microsoft .NET Framework 2.0.50727 como mínimo. Este requisito se cumple mediante Microsoft .NET Framework 3.5 Service Pack 1. Este requisito no se cumple con Microsoft .NET Framework 4 y versiones posteriores de Microsoft .NET Framework.

Para obtener información sobre cómo agregar o instalar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] y agregar o instalar las versiones necesarias de Microsoft .NET Framework, vea [Instalación del motor de Windows PowerShell 2.0](../Topic/Installing-the-Windows-PowerShell-2.0-Engine.md). Para obtener información acerca de cómo iniciar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], consulte [Inicio del motor de Windows PowerShell 2.0](../Topic/Starting-the-Windows-PowerShell-2.0-Engine.md).

## Entorno de preinstalación de Windows
[!INCLUDE[psversion2](../Token/psversion2_md.md)], [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)] se ejecutan en el Entorno de preinstalación de Windows (Windows PE). Sin embargo, no se admiten los siguientes cmdlets.

-   [Cmdlets del Servicio de transferencia inteligente en segundo plano (BITS)](http://go.microsoft.com/fwlink/?LinkId=257514)

-   [Get-EventLog](assetId:///b4985b11-82bf-487d-928d-becd96fc0419)

-   [Get-WinEvent[PSITPro5_Diagnostic]](assetId:///5fe94870-ed6b-4ce2-9500-93846cc65c95)

-   [Save-Help](assetId:///aed94f90-b73f-4e25-a25d-7c18d9f161fa)

-   [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545)

Además, el servicio **WinRm** no está presente en Windows PE.

## Consulte también
[Introducción a Windows PowerShell](../Topic/Getting-Started-with-Windows-PowerShell.md)
[Instalación de Windows PowerShell](../Topic/Installing-Windows-PowerShell.md)
[Inicio de Windows PowerShell [ps]](assetId:///8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)



<!--HONumber=Apr16_HO1-->


