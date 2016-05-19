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
En este tema se explica cómo instalar el motor de Windows PowerShell 2.0.

Windows PowerShell 3.0 está diseñado para ser compatible con versiones anteriores de Windows PowerShell 2.0. Los cmdlets, proveedores, complementos, módulos y scripts escritos para Windows PowerShell 2.0 se ejecutan sin cambios en Windows PowerShell 3.0 y Windows PowerShell 4.0. Sin embargo, debido a un cambio en la directiva de activación en tiempo de ejecución de Microsoft .NET Framework 4, los programas host de Windows PowerShell escritos para Windows PowerShell 2.0 y compilados con Common Language Runtime (CLR) 2.0 no se pueden ejecutar sin modificaciones en versiones posteriores de Windows PowerShell, que está compilado con CLR 4.0.

Para mantener la compatibilidad con versiones anteriores de los comandos y los programas host que se ven afectados por estos cambios, los motores de Windows PowerShell 2.0, Windows PowerShell 3.0 y Windows PowerShell 4.0 están diseñados para ejecutarse en paralelo. Además, el motor de Windows PowerShell 2.0 se incluye en Windows Server 2012 R2, Windows 8.1, Windows 8, Windows Server 2012 y Windows Management Framework 3.0. El motor de Windows PowerShell 2.0 está destinado a usarse solo cuando un programa host o un script existente no se puede ejecutar porque es incompatible con Windows PowerShell 3.0, Windows PowerShell 4.0 o Microsoft .NET Framework 4. Estos casos deben ser infrecuentes.

El motor de Windows PowerShell 2.0 es una característica opcional de Windows Server 2012 R2, Windows 8.1, Windows® 8 y Windows Server® 2012. En versiones anteriores de Windows, al instalar Windows Management Framework 3.0, la instalación de Windows PowerShell 3.0 reemplaza completamente la instalación de Windows PowerShell 2.0 en el directorio de instalación de Windows PowerShell. Sin embargo, se conserva el motor de Windows PowerShell 2.0.

Para información sobre cómo iniciar el motor de Windows PowerShell 2.0, consulte [Iniciar el motor de Windows PowerShell 2.0](Starting-the-Windows-PowerShell-2.0-Engine.md)..

## En Windows 8.1 y Windows 8
En Windows 8.1 y Windows 8, la característica del motor de Windows PowerShell 2.0 está activada de forma predeterminada. Sin embargo, para usarla, debe activar la opción para Microsoft .NET Framework 3.5, que es necesaria. En esta sección también se explica cómo activar y desactivar la característica del motor de Windows PowerShell 2.0.

#### Para activar .NET Framework 3.5

1.  En la pantalla **Inicio**, escriba **Características de Windows**..

2.  En la barra **Aplicaciones**, haga clic en **Configuración** y después en **Activar o desactivar las características de Windows**..

3.  En el cuadro **Características de Windows**, haga clic en **.NET Framework 3.5 (incluye .NET 2.0 y 3.0)** para seleccionar esta opción.

    Al seleccionar **.NET Framework 3.5 (incluye .NET 2.0 y 3.0)**, el cuadro se rellena para indicar que solo una parte de la característica está seleccionada. Sin embargo, esto es suficiente para el motor de Windows PowerShell 2.0.

#### Para activar y desactivar el motor de Windows PowerShell 2.0

1.  En la pantalla **Inicio**, escriba **Características de Windows**..

2.  En la barra **Aplicaciones**, haga clic en **Configuración** y después en **Activar o desactivar las características de Windows**..

3.  En el cuadro **Características de Windows**, expanda el nodo **Windows PowerShell 2.0** y haga clic en la casilla **Motor de Windows PowerShell 2.0** para activarla o desactivarla.

## En Windows Server 2012 R2 y Windows Server 2012
Use los procedimientos siguientes para agregar las características del motor de Windows PowerShell 2.0 y Microsoft .NET Framework 3.5. El motor de Windows PowerShell 2.0 requiere Microsoft .NET Framework 2.0.50727 como mínimo. Este requisito se cumple con Microsoft .NET Framework 3.5.

#### Para agregar la característica de .NET Framework 3.5

1.  En **Administrador del servidor** del menú **Administrar**, haga clic en **Agregar roles y características**..

    O bien en **Administrador del servidor**, haga clic en **Todos los servidores**, haga clic con el botón derecho en el nombre de un servidor y luego seleccione **Agregar roles y características**..

2.  En la página **Tipo de instalación**, seleccione **Instalación basada en características o en roles**..

3.  En la página **Características**, expanda el nodo **Características de .NET Framework 3.5** y seleccione **.NET Framework 3.5 (incluye .NET 2.0 y 3.0)**..

    Las demás opciones bajo ese nodo no son necesarias para el motor de Windows PowerShell 2.0.

#### Para agregar la característica del motor de Windows PowerShell 2.0

-   En **Administrador del servidor** del menú **Administrar**, haga clic en **Agregar roles y características**..

    En **Administrador del servidor**, haga clic en **Todos los servidores**, haga clic con el botón derecho en el nombre de un servidor y luego seleccione **Agregar roles y características**..

-   En la página **Tipo de instalación**, seleccione **Instalación basada en características o en roles**..

-   En la página **Características**, expanda el nodo **Windows PowerShell (instalado)** y seleccione **Motor de Windows PowerShell 2.0**..

Para información sobre cómo iniciar el motor de Windows PowerShell 2.0, consulte [Iniciar el motor de Windows PowerShell 2.0](Starting-the-Windows-PowerShell-2.0-Engine.md)..

## En sistemas anteriores
El paquete de [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) que instala Windows PowerShell 4.0 en Windows 7, Windows Server 2008 R2 y Windows Server 2012 incluye el motor de Windows PowerShell 2.0. El motor de Windows PowerShell 2.0 está habilitado y listo para usarlo, si es necesario, sin requerir otras operaciones de instalación o configuración.

El paquete de Windows Management Framework 3.0 que instala Windows PowerShell 3.0 en Windows 7, Windows Server 2008 R2 y Windows Server 2008 incluye el motor de Windows PowerShell 2.0. El motor de Windows PowerShell 2.0 está habilitado y listo para usarlo, si es necesario, sin requerir otras operaciones de instalación o configuración.

## Consulte también
[Requisitos del sistema de Windows PowerShell](Windows-PowerShell-System-Requirements.md)
[Instalar Windows PowerShell](Installing-Windows-PowerShell.md)
[Iniciar Windows PowerShell [ps]](https://technet.microsoft.com/en-us/library/8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)
[Iniciar el motor de Windows PowerShell 2.0](Starting-the-Windows-PowerShell-2.0-Engine.md)



<!--HONumber=May16_HO2-->


