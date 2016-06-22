---
title:  Iniciar la versión de 32 bits de Windows PowerShell
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  12b31890-2609-4a76-8c24-0ebe78084f50
---

# Iniciar la versión de 32 bits de Windows PowerShell
Al instalar Windows PowerShell en un equipo de 64 bits, **Windows PowerShell (x86)**, se instala una versión de 32 bits de Windows PowerShell además de la versión de 64 bits. Al ejecutar Windows PowerShell, la versión de 64 bits se ejecuta de manera predeterminada.

En cambio, en ocasiones deberá ejecutar **Windows PowerShell (x86)**, como cuando usa un módulo que requiere la versión de 32 bits o cuando se conecta de forma remota a un equipo de 32 bits.

Para iniciar una versión de 32 bits de Windows PowerShell, use uno de los procedimientos siguientes.

#### In Windows Server® 2012 R2

-   En la pantalla **Inicio**, escriba **Windows PowerShell (x86)**. Haga clic en el icono **Windows PowerShell x86**.

-   En **Administrador del servidor**, desde el menú **Herramientas**, seleccione **Windows PowerShell (x86)**.

-   En el escritorio, mueva el cursor a la esquina superior derecha, haga clic en **Buscar**, escriba **PowerShell x86** y, a continuación, haga clic en **Windows PowerShell (x86)**.

-   Mediante la línea de comandos, escriba: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### En Windows Server 2012

-   En la página **Inicio**, escriba **Powershell** y, después, haga clic en **Windows PowerShell (x86)**.

-   En **Administrador del servidor**, desde el menú **Herramientas**, seleccione **Windows PowerShell (x86)**.

-   En el escritorio, mueva el cursor a la esquina superior derecha, haga clic en **Buscar**, escriba **PowerShell** y, a continuación, haga clic en **Windows PowerShell (x86)**.

-   Mediante la línea de comandos, escriba: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### En Windows 8.1

-   En la pantalla **Inicio**, escriba **Windows PowerShell (x86)**. Haga clic en el icono **Windows PowerShell x86**.

-   Si está ejecutando [Herramientas de administración remota del servidor](http://go.microsoft.com/fwlink/?LinkID=304145) para Windows 8.1, también puede abrir Windows PowerShell x86 desde el menú **Server Manager Tools** (Herramientas del administrador del servidor). Seleccione **Windows PowerShell (x86)**.

-   En el escritorio, mueva el cursor a la esquina superior derecha, haga clic en **Buscar**, escriba **PowerShell x86** y, a continuación, haga clic en **Windows PowerShell (x86)**.
   
-   Mediante la línea de comandos, escriba: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### En Windows 8

-   En la pantalla **Inicio**, mueva el cursor a la esquina superior derecha, haga clic en **Configuración**, haga clic en **Mosaicos** y, a continuación, mueva el control deslizante **Mostrar herramientas administrativas** a Sí. A continuación, escriba **PowerShell** y haga clic en **Windows PowerShell (x86)**.

-   Si está ejecutando [Herramientas de administración remota del servidor](http://www.microsoft.com/download/details.aspx?id=28972) para Windows 8, también puede abrir Windows PowerShell x86 desde el menú **Server ManagerTools** (Herramientas del administrador del servidor). Seleccione **Windows PowerShell (x86)**.

-   En la página **Inicio** o en el escritorio, escriba **Powershell (x86)** y, después, haga clic en **Windows PowerShell (x86)**.

-   Mediante la línea de comandos, escriba: `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`


<!--HONumber=Jun16_HO3-->


