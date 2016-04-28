---
title: Instalar el motor de Windows PowerShell 2.0
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82928f2b-f96a-4ae6-a0d0-6e7b181da308
---
# Instalar el motor de Windows PowerShell 2.0
En este tema se explica cómo instalar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

[!INCLUDE[psversion3](../Token/psversion3_md.md)] está diseñado para ser compatible con versiones anteriores de [!INCLUDE[psversion2](../Token/psversion2_md.md)]. Los cmdlets, proveedores, complementos, módulos y scripts escritos para [!INCLUDE[psversion2](../Token/psversion2_md.md)] se ejecutan sin cambios en [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)]. Sin embargo, debido a un cambio en la directiva de activación en tiempo de ejecución de Microsoft .NET Framework 4, los programas host de Windows PowerShell escritos para [!INCLUDE[psversion2](../Token/psversion2_md.md)] y compilados con Common Language Runtime (CLR) 2.0 no se puede ejecutar sin modificaciones en versiones posteriores de [!INCLUDE[wps_2](../Token/wps_2_md.md)], que está compilado con CLR 4.0.

Para mantener la compatibilidad con versiones anteriores de los comandos y los programas host que se ven afectados por estos cambios, los motores de [!INCLUDE[psversion2](../Token/psversion2_md.md)], [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)] están diseñados para ejecutarse en paralelo. Además, el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] se incluye en [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)], [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)] y [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)]. El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] está destinado a usarse solo cuando un programa host o un script existente no se puede ejecutar porque no es compatible con [!INCLUDE[psversion3](../Token/psversion3_md.md)], [!INCLUDE[psversion4](../Token/psversion4_md.md)] o Microsoft .NET Framework 4. Estos casos deben ser infrecuentes.

El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] es una característica opcional de [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], [!INCLUDE[win8_client_1](../Token/win8_client_1_md.md)] y [!INCLUDE[win8_server_1](../Token/win8_server_1_md.md)]. En versiones anteriores de Windows, al instalar [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)], la instalación de [!INCLUDE[psversion3](../Token/psversion3_md.md)] reemplaza por completo la instalación de [!INCLUDE[psversion2](../Token/psversion2_md.md)] en el directorio de instalación de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. Sin embargo, el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] se conserva.

Para obtener información acerca de cómo iniciar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], consulte [Iniciar el motor de Windows PowerShell 2.0](../Topic/Starting-the-Windows-PowerShell-2.0-Engine.md).

## En [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)] y [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)]
En [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)] y [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)], la característica del motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] está activada de forma predeterminada. Sin embargo, para usarla, debe activar la opción para Microsoft .NET Framework 3.5, que es necesaria. En esta sección también se explica cómo activar y desactivar la característica del motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

#### Para activar .NET Framework 3.5

1.  En la pantalla **Inicio**, escriba **Características de Windows**.

2.  En la barra **Aplicaciones**, haga clic en **Configuración** y, después, en **Activar o desactivar las características de Windows**.

3.  En el cuadro **Características de Windows**, haga clic en **.NET Framework 3.5 (incluye .NET 2.0 y 3.0)** para seleccionar esta opción.

    Al seleccionar **.NET Framework 3.5 (incluye .NET 2.0 y 3.0)**, el cuadro se rellena para indicar que solo una parte de la característica está seleccionada. Sin embargo, es suficiente para el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

#### Para activar y desactivar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]

1.  En la pantalla **Inicio**, escriba **Características de Windows**.

2.  En la barra **Aplicaciones**, haga clic en **Configuración** y, después, en **Activar o desactivar las características de Windows**.

3.  En el cuadro **Características de Windows**, expanda el nodo **[!INCLUDE[psversion2](../Token/psversion2_md.md)]** y haga clic en la casilla **Motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]** para activarla o desactivarla.

## En [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)] y [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)]
Use los procedimientos siguientes para agregar las características del motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] y Microsoft .NET Framework 3.5. El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] requiere Microsoft .NET Framework 2.0.50727 como mínimo. Este requisito se cumple con Microsoft .NET Framework 3.5.

#### Para agregar la característica de .NET Framework 3.5

1.  En el menú **Administrador del servidor** del menú **Administrar**, haga clic en **Agregar roles y características**.

    O bien en **Administrador del servidor**, haga clic en **Todos los servidores**, haga clic con el botón derecho en el nombre de un servidor y, a continuación, seleccione **Agregar roles y características**.

2.  En la página **Tipo de instalación**, seleccione **Instalación basada en características o en roles**.

3.  En la página **Características**, expanda el nodo **Características de .NET Framework 3.5** y seleccione **.NET Framework 3.5 (incluye .NET 2.0 y 3.0)**.

    Las demás opciones bajo ese nodo no son necesarias para el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

#### Para agregar la característica del motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]

-   En el menú **Administrador del servidor** del menú **Administrar**, haga clic en **Agregar roles y características**.

    En **Administrador del servidor**, haga clic en **Todos los servidores**, haga clic con el botón derecho en el nombre de un servidor y, a continuación, seleccione **Agregar roles y características**.

-   En la página **Tipo de instalación**, seleccione **Instalación basada en características o en roles**.

-   En la página **Características**, expanda el nodo **[!INCLUDE[mshshort](../Token/mshshort_md.md)] (instalado)** y seleccione **Motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]**.

Para obtener información acerca de cómo iniciar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], consulte [Iniciar el motor de Windows PowerShell 2.0](../Topic/Starting-the-Windows-PowerShell-2.0-Engine.md).

## En sistemas anteriores
El paquete [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) que instala [!INCLUDE[psversion4](../Token/psversion4_md.md)] en [!INCLUDE[win7_client_secondref](../Token/win7_client_secondref_md.md)], [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)] y [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)], incluye el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]. El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] está habilitado y listo para usar, si es necesario, sin requerir otras operaciones de instalación o configuración.

El paquete [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)] que instala [!INCLUDE[psversion3](../Token/psversion3_md.md)] en [!INCLUDE[win7_client_secondref](../Token/win7_client_secondref_md.md)], [!INCLUDE[win7_server_secondref](../Token/win7_server_secondref_md.md)] y [!INCLUDE[lserver](../Token/lserver_md.md)], incluye el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]. El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] está habilitado y listo para usar, si es necesario, sin requerir otras operaciones de instalación o configuración.

## Consulte también
[Requisitos del sistema de Windows PowerShell](../Topic/Windows-PowerShell-System-Requirements.md)
[Instalar Windows PowerShell](../Topic/Installing-Windows-PowerShell.md)
[Iniciar Windows PowerShell [ps]](assetId:///8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)
[Iniciar el motor de Windows PowerShell 2.0](../Topic/Starting-the-Windows-PowerShell-2.0-Engine.md)



<!--HONumber=Apr16_HO1-->


