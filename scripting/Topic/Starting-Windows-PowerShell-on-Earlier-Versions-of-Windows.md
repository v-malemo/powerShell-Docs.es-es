---
title: Iniciar Windows PowerShell en versiones anteriores de Windows
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57125436-3d1e-4e7f-b5c4-8f0ecb49d642
---
# Iniciar Windows PowerShell en versiones anteriores de Windows
En esta sección se explica cómo iniciar [!INCLUDE[wps_2](../Token/wps_2_md.md)] y [!INCLUDE[mshgraphicalhost](../Token/mshgraphicalhost_md.md)] en [!INCLUDE[win7_client_firstref](../Token/win7_client_firstref_md.md)], [!INCLUDE[win7_server_firstref](../Token/win7_server_firstref_md.md)] y [!INCLUDE[lserver](../Token/lserver_md.md)]. También se explica cómo habilitar la característica opcional para [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)] de [!INCLUDE[psversion2](../Token/psversion2_md.md)] en [!INCLUDE[win7_server_firstref](../Token/win7_server_firstref_md.md)] y [!INCLUDE[lserver](../Token/lserver_md.md)].

Para instalar [!INCLUDE[psversion4](../Token/psversion4_md.md)] en sistemas compatibles, descargue e instale [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881). Para obtener más información, consulte [Instalar Windows PowerShell](../Topic/Installing-Windows-PowerShell.md).

Para instalar [!INCLUDE[psversion3](../Token/psversion3_md.md)] en sistemas compatibles, descargue e instale [Windows Management Framework 3.0](http://go.microsoft.com/fwlink/?LinkID=240290). Para obtener más información, consulte [Instalar Windows PowerShell](../Topic/Installing-Windows-PowerShell.md).

## Como iniciar [!INCLUDE[mshshort](../Token/mshshort_md.md)] en versiones anteriores de Windows
Use cualquiera de los métodos siguientes para iniciar la versión instalada de [!INCLUDE[psversion3](../Token/psversion3_md.md)], o [!INCLUDE[psversion4](../Token/psversion4_md.md)], cuando corresponda.

#### Desde el menú Inicio

-   Haga clic en **Inicio**, escriba **ISE** y, después, haga clic en **Windows PowerShell**.

-   Desde el menú **Inicio**, haga clic en **Inicio**, **Todos los programas**, **Accesorios**, la carpeta **Windows PowerShell** y, luego, en **Windows PowerShell**.

#### En el símbolo del sistema

-   En Cmd.exe, [!INCLUDE[wps_2](../Token/wps_2_md.md)] o [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE, para iniciar [!INCLUDE[wps_2](../Token/wps_2_md.md)], escriba:

    ```
    PowerShell
    ```

    También puede usar los parámetros del programa de PowerShell.exe para personalizar la sesión. Para obtener más información, consulte [Ayuda de línea de comandos de PowerShell.exe](../Topic/PowerShell.exe-Command-Line-Help.md).

#### Con privilegios administrativos ("Ejecutar como administrador")

1.  Haga clic en **Inicio**, escriba **PowerShell**, haga clic con el botón derecho en **Windows PowerShell** y, después, haga clic en **Ejecutar como administrador**.

## Cómo iniciar [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)] en versiones anteriores de Windows
Use cualquiera de los métodos siguientes para iniciar [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)].

#### Desde el menú Inicio

-   Haga clic en **Inicio**, escriba **ISE** y, después, haga clic en **Windows PowerShell ISE**.

-   Desde el menú **Inicio**, haga clic en **Inicio**, **Todos los programas**, **Accesorios**, la carpeta **Windows PowerShell** y, luego, en **Windows PowerShell ISE**.

#### En el símbolo del sistema

-   En Cmd.exe, [!INCLUDE[wps_2](../Token/wps_2_md.md)] o [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE, para iniciar [!INCLUDE[wps_2](../Token/wps_2_md.md)], escriba:

    ```
    PowerShell_ISE
    ```

    o

    ```
    ISE
    ```

#### Con privilegios administrativos ("Ejecutar como administrador")

1.  Haga clic en **Inicio**, escriba **ISE**, haga clic con el botón derecho en **Windows PowerShell ISE** y, después, haga clic en **Ejecutar como administrador**.

## Cómo habilitar [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)] en versiones anteriores de Windows
En [!INCLUDE[psversion4](../Token/psversion4_md.md)] y [!INCLUDE[psversion3](../Token/psversion3_md.md)], [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)] está habilitado de forma predeterminada en todas las versiones de Windows. Si aún no está habilitado, Windows Management Framework 4.0 o [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)] lo habilitan.

En [!INCLUDE[psversion2](../Token/psversion2_md.md)], [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)] está habilitado de forma predeterminada en [!INCLUDE[win7_client_secondref](../Token/win7_client_secondref_md.md)]. Sin embargo, en [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)] y [!INCLUDE[lserver](../Token/lserver_md.md)], es una característica opcional.

Para habilitar [!INCLUDE[mshgraphicalhostshort](../Token/mshgraphicalhostshort_md.md)] de [!INCLUDE[psversion2](../Token/psversion2_md.md)] en [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)] o [!INCLUDE[lserver](../Token/lserver_md.md)], use el procedimiento siguiente.

#### Para habilitar [!INCLUDE[mshgraphicalhost](../Token/mshgraphicalhost_md.md)]

1.  Inicie el Administrador del servidor.

2.  Haz clic en **Características** y, a continuación, en **Agregar características**.

3.  En Seleccionar características, haga clic en [!INCLUDE[mshgraphicalhost](../Token/mshgraphicalhost_md.md)].



<!--HONumber=Apr16_HO1-->


