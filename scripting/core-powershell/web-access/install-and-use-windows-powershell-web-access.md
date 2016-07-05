---
title: "Instalación y uso de Windows PowerShell Web Access"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: d2f78148402f06992f5f58cd40e8c4f624b5e4b5

---

#  Instalación y uso de Windows PowerShell Web Access

Actualizado: 5 de noviembre de 2013

Se aplica a: Windows Server 2012 R2, Windows Server 2012

Windows PowerShell® Web Access, que se incluyó por primera vez en Windows Server® 2012, actúa como puerta de enlace de Windows PowerShell, ofreciendo una consola de Windows PowerShell basada en web dirigida a un equipo remoto. Permite a los profesionales de TI ejecutar comandos y scripts de Windows PowerShell desde una consola de Windows PowerShell en un explorador web, sin que sea necesario contar con Windows PowerShell, software de administración remota o la instalación de un complemento de explorador en el dispositivo cliente. Lo único que se necesita para ejecutar la consola de Windows PowerShell basada en web es una puerta de enlace de Windows PowerShell Web Access correctamente configurada y un explorador en el dispositivo cliente que admita JavaScript® y acepte cookies.

Entre los dispositivos cliente se incluyen equipos portátiles, equipos que no son de trabajo, equipos prestados, Tablet PC, quioscos multimedia web, equipos que no ejecutan un sistema operativo Windows y exploradores de teléfono móvil. Los profesionales de TI pueden llevar a cabo tareas críticas de administración en servidores remotos basados en Windows desde dispositivos que cuenten con acceso a una conexión de Internet y un explorador web.

Después de instalar y configurar la puerta de enlace correctamente, los usuarios pueden obtener acceso a una consola de Windows PowerShell mediante un explorador web. Al abrir el sitio web protegido de Windows PowerShell Web Access y, una vez que se hayan autenticado correctamente, los usuarios podrán ejecutar una consola de Windows PowerShell basada en web.

El proceso de instalación y configuración de Windows PowerShell Web Access consta de tres pasos:

1.  Instalación de Windows PowerShell Web Access

2.  Configuración de la puerta de enlace

3.  Configuración de las reglas de autorización que permiten a los usuarios tener acceso a la consola de Windows PowerShell basada en web

Antes de instalar y configurar Windows PowerShell Web Access, se recomienda leer toda la guía, que incluye instrucciones sobre cómo instalar, proteger y desinstalar Windows PowerShell Web Access. En el tema [Uso de la consola de Windows PowerShell basada en web](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx) se describe cómo los usuarios inician sesión en la consola basada en web y se explican las limitaciones y diferencias entre la consola de Windows PowerShell basada en web y la consola de **powershell.exe**. Los usuarios finales de la consola basada en web deben leer [Uso de la consola de Windows PowerShell basada en web](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx), pero no es necesario que lean el resto de esta guía.

En este tema, no se proporciona una guía de operaciones de Servidor web (IIS) exhaustiva, sino solo los pasos necesarios para configurar la puerta de enlace de Windows PowerShell Web Access. Para obtener más información sobre la configuración y protección de sitios web en IIS, vea los recursos de documentación de IIS en la sección Vea también.

En el siguiente diagrama se muestra cómo funciona Windows PowerShell Web Access.

<span><img src="https://i-technet.sec.s-msft.com/dynimg/IC564303.jpeg" title="Windows PowerShell Web Access diagram" alt="Windows PowerShell Web Access diagram" id="ee15fa8f-ce13-49e5-933d-514f6d60a2b1" /></span>

En este tema:

-   [Requisitos para ejecutar Windows PowerShell Web Access](#BKMK_reqs)

-   [Compatibilidad con exploradores y dispositivos cliente](#BKMK_browser)

-   [Implementación recomendada (rápida)](#BKMK_recm)

-   [Implementación personalizada](#BKMK_custom)

-   [Configurar un certificado original](#BKMK_configcert)

<a href="" id="BKMK_reqs"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Requisitos para ejecutar Windows PowerShell Web Access</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_0" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Windows PowerShell Web Access requiere que Servidor web (IIS), .NET Framework 4.5 y Windows PowerShell 3.0 o Windows PowerShell 4.0 se ejecuten en el servidor donde desea ejecutar la puerta de enlace. Puede instalar Windows PowerShell Web Access en un servidor que ejecuta Windows Server 2012 R2 o Windows Server 2012 mediante el uso del Asistente para agregar roles y características en el Administrador del servidor o de los cmdlets de implementación de Windows PowerShell para el Administrador del servidor. Al instalar Windows PowerShell Web Access mediante el Administrador del servidor o sus cmdlets de implementación, automáticamente se agregan los roles y las características necesarios como parte del proceso de instalación.

Windows PowerShell Web Access permite a los usuarios remotos obtener acceso a los equipos de la organización mediante Windows PowerShell en un explorador web. Si bien Windows PowerShell Web Access es una herramienta de administración eficaz y conveniente, el acceso web conlleva riesgos de seguridad, por lo que debe configurarse de la manera más segura posible. Se recomienda que los administradores que configuran la puerta de enlace de Windows PowerShell Web Access usen los niveles de seguridad disponibles, tanto las reglas de autorización basadas en cmdlets incluidas con Windows PowerShell Web Access como los niveles de seguridad disponibles en Servidor web (IIS) y en aplicaciones de terceros. En esta documentación se incluyen ejemplos que no son seguros, recomendados únicamente para entornos de prueba, así como ejemplos recomendados para implementaciones seguras.

<a href="" id="BKMK_browser"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Compatibilidad con exploradores y dispositivos cliente</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Windows PowerShell Web Access admite los siguientes exploradores de Internet. Aunque los exploradores móviles no se admiten oficialmente, muchos de ellos pueden ejecutar la consola de Windows PowerShell basada en web. También se espera que funcionen otros exploradores que aceptan cookies y ejecutan JavaScript y sitios web HTTPS, pero no se han probado de manera oficial.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Exploradores de equipos de escritorio admitidos</span></a>

------------------------------------------------------------------------

-   Windows® Internet Explorer® para Microsoft Windows® 8.0, 9.0, 10.0 y 11.0

-   Mozilla Firefox® 10.0.2

-   Google Chrome™ 17.0.963.56m para Windows

-   Apple Safari® 5.1.2 para Windows

-   Apple Safari 5.1.2 para Mac OS®

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Exploradores o dispositivos móviles mínimamente probados</span></a>

------------------------------------------------------------------------

-   Windows Phone 7 y 7.5

-   Google Android WebKit 3.1 Browser Android 2.2.1 (Kernel 2.6)

-   Apple Safari para sistema operativo iPhone 5.0.1

-   Apple Safari para sistema operativo iPad 2 5.0.1

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Requisitos de explorador</span></a>

------------------------------------------------------------------------

Para usar la consola de Windows PowerShell Web Access basada en web, los exploradores deben poder hacer lo siguiente.

-   Permitir cookies del sitio web de la puerta de enlace de Windows PowerShell Web Access.

-   Abrir y leer páginas HTTPS.

-   Abrir y ejecutar sitios web que usen JavaScript.

<a href="" id="BKMK_recm"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Implementación recomendada (rápida)</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Puede instalar la puerta de enlace de Windows PowerShell Web Access en un servidor que ejecuta Windows Server 2012 R2 o Windows Server 2012 mediante el uso de los cmdlets de Windows PowerShell o del Asistente para agregar roles y características que se abre desde el Administrador del servidor. Para la instalación y la configuración rápida, use cmdlets de Windows PowerShell, como se describe en esta sección.

-   [Paso 1: Instalar Windows PowerShell Web Access](#BKMK_step1)

-   [Paso 2: Configurar la puerta de enlace](#BKMK_step2)

-   [Paso 3: Configurar una regla de autorización restrictiva](#BKMK_step3)

<a href="" id="BKMK_step1"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 1: Instalar Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### Para instalar Windows PowerShell Web Access mediante cmdlets de Windows PowerShell

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell**, en la barra de tareas y, luego,en **Ejecutar como administrador**.

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en **Windows PowerShell** y, luego, en **Ejecutar como administrador**.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>En Windows PowerShell 3.0 y 4.0, no es necesario importar el módulo de cmdlets del Administrador del servidor en la sesión de Windows PowerShell antes de ejecutar los cmdlets que forman parte del módulo. El módulo se importa automáticamente la primera vez que se ejecuta un cmdlet que es parte del módulo. Asimismo, los cmdlets de Windows PowerShell no distinguen mayúsculas de minúsculas.</p></td>
    </tr>
    </tbody>
    </table>

2.  Escriba lo siguiente y presione **Entrar**, donde *computer\_name* representa un equipo remoto en el cual desea instalar Windows PowerShell Web Access, si procede. El parámetro <span class="code">Restart</span> reinicia automáticamente los servidores de destino si es necesario.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_374a9c21-4f6e-471e-b957-bb190a594533'); "Copy to clipboard.")

        Install-WindowsFeature –Name WindowsPowerShellWebAccess -ComputerName <computer_name> -IncludeManagementTools -Restart

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Al instalarse Windows PowerShell Web Access mediante el uso de los cmdlets de Windows PowerShell, no se agregan herramientas de administración de Servidor web (IIS) de forma predeterminada. Si desea instalar las herramientas de administración en el mismo servidor que el de la puerta de enlace de Windows PowerShell Web Access, agregue el parámetro <span class="code">IncludeManagementTools</span> al comando de instalación (como se indica en este paso). Si administra el sitio web de Windows PowerShell Web Access desde un equipo remoto, instale el complemento Administrador de IIS mediante la instalación de las <a href="http://go.microsoft.com/fwlink/?LinkID=304145">Herramientas de administración remota del servidor para Windows 8.1</a> o <a href="http://go.microsoft.com/fwlink/p/?LinkID=238560">Herramientas de administración remota del servidor para Windows 8</a> en el equipo desde el cual desea administrar la puerta de enlace.</p></td>
    </tr>
    </tbody>
    </table>

    Para instalar roles y características en un VHD sin conexión, debe agregar los parámetros <span class="code">ComputerName</span> y <span class="code">VHD</span>. El parámetro <span class="code">ComputerName</span> contiene el nombre del servidor en el que se montará el VHD y el parámetro <span class="code">VHD</span> contiene la ruta de acceso al archivo VHD en el servidor especificado.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_d841d509-347e-49d0-bf54-8d1f306bece6'); "Copy to clipboard.")

        Install-WindowsFeature –Name WindowsPowerShellWebAccess –VHD <path> -ComputerName <computer_name> -IncludeManagementTools -Restart

3.  Una vez completada la instalación, compruebe que Windows PowerShell Web Access se haya instalado en los servidores de destino mediante la ejecución del cmdlet **Get-WindowsFeature** en un servidor de destino, en una consola de Windows PowerShell abierta con derechos de usuario elevados. Otro modo de comprobar que se haya instalado Windows PowerShell Web Access en la consola del Administrador del servidor consiste en seleccionar un servidor de destino en la página **Todos los servidores** y, a continuación, visualizar el icono **Roles y características** del servidor seleccionado. También puede ver el archivo Léame para Windows PowerShell Web Access.

4.  Una vez instalado Windows PowerShell Web Access, se le pedirá que revise el archivo Léame, que contiene las instrucciones de configuración necesarias básicas para la puerta de enlace. Estas instrucciones de configuración también se encuentran en la sección siguiente, [Paso 2: Configurar la puerta de enlace](#BKMK_step2). La ruta de acceso al archivo Léame es <span class="computerOutputInline">C:\\Windows\\Web\\PowerShellWebAccess\\wwwroot\\README.txt</span>.

<a href="" id="BKMK_step2"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 2: Configurar la puerta de enlace</span></a>

------------------------------------------------------------------------

El cmdlet **Install-PswaWebApplication** es un método rápido para configurar Windows PowerShell Web Access. Si bien puede agregar el parámetro <span class="code">UseTestCertificate</span> al cmdlet <span class="code">Install-PswaWebApplication</span> para instalar un certificado SSL autofirmado con fines de prueba, esto no es seguro. En un entorno de producción seguro, siempre use un certificado SSL válido firmado por una entidad de certificación (CA). Los administradores pueden reemplazar el certificado de prueba por un certificado firmado de su elección mediante la consola del Administrador de IIS.

Puede completar la configuración de la aplicación web Windows PowerShell Web Access mediante la ejecución del cmdlet <span class="code">Install-PswaWebApplication</span> o mediante los pasos de configuración basados en GUI del Administrador de IIS. De manera predeterminada, el cmdlet instala la aplicación web, **pswa** (y un grupo de aplicaciones, **pswa\_pool**), en el contenedor **Sitio web predeterminado**, como se muestra en el Administrador de IIS. Si lo desea, puede indicar al cmdlet que cambie el contenedor del sitio predeterminado de la aplicación web. El Administrador de IIS proporciona opciones de configuración que se encuentran disponibles para las aplicaciones web, como cambiar el número de puerto o el certificado de Capa de sockets seguros (SSL).

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC17938.jpeg" title="System_CAPS_security" alt="System_CAPS_security" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-security" /></span><span class="alertTitle"> Nota de seguridad </span></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Se recomienda encarecidamente que los administradores configuren la puerta de enlace para que use un certificado válido firmado por una CA.</p></td>
</tr>
</tbody>
</table>

-   [Para configurar la puerta de enlace de Windows PowerShell Web Access con un certificado de prueba mediante Install-PswaWebApplication](#BKMK_testcert)

-   [Para configurar la puerta de enlace de Windows PowerShell Web Access con un certificado original mediante Install-PswaWebApplication y el Administrador de IIS](#BKMK_gencert)

#### Para configurar la puerta de enlace de Windows PowerShell Web Access con un certificado de prueba mediante Install-PswaWebApplication

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell** en la barra de tareas.

    -   En la pantalla **Inicio** de Windows, haga clic en **Windows PowerShell**.

2.  Escriba lo siguiente y, después, presione **Entrar**.

    **Install-PswaWebApplication -UseTestCertificate**

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC17938.jpeg" title="System_CAPS_security" alt="System_CAPS_security" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-security" /></span><span class="alertTitle"> Nota de seguridad </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>El parámetro <span class="code">UseTestCertificate</span> solo debe usarse en un entorno de prueba privado. Para un entorno de producción seguro, se recomienda usar un certificado válido firmado por una CA.</p></td>
    </tr>
    </tbody>
    </table>

    La ejecución del cmdlet instala la aplicación web Windows PowerShell Web Access dentro del contenedor Sitio web predeterminado de IIS. El cmdlet crea la infraestructura necesaria para ejecutar Windows PowerShell Web Access en el sitio web predeterminado, https://&lt;server\_name&gt;/pswa. Para instalar la aplicación web en otro sitio web, proporcione el nombre del sitio web mediante la adición del parámetro <span class="code">WebSiteName</span>. Para cambiar el nombre de la aplicación web (el nombre predeterminado es <span class="code">pswa</span>), agregue el parámetro <span class="code">WebApplicationName</span>.

    La siguiente configuración se establece mediante la ejecución del cmdlet. Puede cambiarla de forma manual en la consola del Administrador de IIS, si lo desea.

    -   Path: /pswa

    -   ApplicationPool: pswa\_pool

    -   EnabledProtocols: http

    -   PhysicalPath: %*windir*%/Web/PowerShellWebAccess/wwwroot

    <span class="label">Ejemplo:</span> <span class="code">Install-PswaWebApplication –webApplicationName myWebApp –useTestCertificate</span>

    En este ejemplo, el sitio web resultante para Windows PowerShell Web Access es https://&lt; *server\_name*&gt;/myWebApp.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>No podrá iniciar sesión hasta que se haya concedido a los usuarios acceso al sitio web mediante la adición de reglas de autorización. Para obtener más información, consulte <a href="#BKMK_step3">Paso 3: Configurar una regla de autorización restrictiva</a> y <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Reglas de autorización y características de seguridad de Windows PowerShell Web Access</a>.</p></td>
    </tr>
    </tbody>
    </table>

#### Para configurar la puerta de enlace de Windows PowerShell Web Access con un certificado original mediante Install-PswaWebApplication y el Administrador de IIS

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell** en la barra de tareas.

    -   En la pantalla **Inicio** de Windows, haga clic en **Windows PowerShell**.

2.  Escriba lo siguiente y, después, presione **Entrar**.

    **Install-PswaWebApplication**

    La siguiente configuración de puerta de enlace se establece mediante la ejecución del cmdlet. Puede cambiarla de forma manual en la consola del Administrador de IIS, si lo desea. También puede especificar valores para los parámetros <span class="code">WebsiteName</span> y <span class="code">WebApplicationName</span> del cmdlet <span class="code">Install-PswaWebApplication</span>.

    -   Path: /pswa

    -   ApplicationPool: pswa\_pool

    -   EnabledProtocols: http

    -   PhysicalPath: %*windir*%/Web/PowerShellWebAccess/wwwroot

3.  Abra la consola del Administrador de IIS mediante uno de los siguientes procedimientos.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor. En el menú **Herramientas** del Administrador del servidor, haga clic en **Administrador de Internet Information Services (IIS)**.

    -   En la pantalla **Inicio** de Windows, haga clic en **Administrador del servidor**.

4.  En el panel del árbol del Administrador de IIS, expanda el nodo del servidor en el que está instalado Windows PowerShell Web Access hasta que aparezca la carpeta **Sitios**. Expanda la carpeta **Sitios**.

5.  Seleccione el sitio web en el que ha instalado la aplicación web Windows PowerShell Web Access. En el panel **Acciones**, haga clic en **Enlaces**.

6.  En el cuadro de diálogo **Enlaces de sitios**, haga clic en **Agregar**.

7.  En el cuadro de diálogo **Agregar enlace de sitio**, en el campo **Tipo**, seleccione **https**.

8.  En el campo **Certificado SSL**, seleccione su certificado firmado en el menú desplegable. Haga clic en **Aceptar**. Consulte [Para configurar un certificado SSL en el Administrador de IIS](#BKMK_cert) en este tema para obtener más información sobre cómo obtener un certificado.

    La aplicación web Windows PowerShell Web Access ya está configurada para usar su certificado SSL firmado. Para obtener acceso a Windows PowerShell Web Access, abra https://&lt;server\_name&gt;/pswa en una ventana del explorador.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>No podrá iniciar sesión hasta que se haya concedido a los usuarios acceso al sitio web mediante la adición de reglas de autorización. Para obtener más información, consulte <a href="#BKMK_step3">Paso 3: Configurar una regla de autorización restrictiva</a> y <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Reglas de autorización y características de seguridad de Windows PowerShell Web Access</a>.</p></td>
    </tr>
    </tbody>
    </table>

<a href="" id="BKMK_step3"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 3: Configurar una regla de autorización restrictiva</span></a>

------------------------------------------------------------------------

Una vez instalado Windows PowerShell Web Access y configurada la puerta de enlace, los usuarios podrán abrir la página de inicio de sesión en un explorador, pero no podrán iniciar sesión hasta que el administrador de Windows PowerShell Web Access les conceda acceso de manera explícita. El control de acceso de Windows PowerShell Web Access se administra mediante el conjunto de cmdlets de Windows PowerShell descrito en la siguiente tabla. No existe una GUI comparable para agregar o administrar reglas de autorización. Para obtener más información sobre los cmdlets de Windows PowerShell Web Access, consulte los temas de referencia de cmdlet, [Windows PowerShell Web Access Cmdlets](https://technet.microsoft.com/library/hh918342.aspx) (Cmdlets de Windows PowerShell Web Access).

Para obtener más información sobre la seguridad y las reglas de autorización de Windows PowerShell Web Access, consulte [Reglas de autorización y características de seguridad de Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx).

#### Para agregar una regla de autorización restrictiva

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell**, en la barra de tareas y, luego,en **Ejecutar como administrador**.

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en **Windows PowerShell** y, luego, en **Ejecutar como administrador**.

2.  <span class="label">Paso opcional para restringir el acceso del usuario mediante configuraciones de sesión:</span> Compruebe que las configuraciones de sesión que quiere usar en las reglas ya existen. Si aún no se han creado, use las instrucciones para crear configuraciones de sesión proporcionadas en [about\_Session\_Configuration\_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx) en MSDN.

3.  Escriba lo siguiente y, después, presione **Entrar**.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_f9e7959b-75d0-4d63-8f8e-02334a8dd09d'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Esta regla de autorización concede a un usuario específico acceso a un equipo de la red al que tiene acceso normalmente, con acceso a una configuración de sesión determinada en el ámbito de las necesidades de cmdlet y scripting típicas del usuario. En el siguiente ejemplo, se concede acceso a un usuario llamado <span class="code">JSmith</span> en el dominio <span class="code">Contoso</span> para administrar el equipo <span class="code">Contoso\_214</span> y usar una configuración de sesión llamada <span class="code">NewAdminsOnly</span>.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_ebd5bc5e-ec5d-4955-a86a-63843e480e37'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Compruebe que se ha creado la regla ejecutando el cmdlet **Get-PswaAuthorizationRule** cmdlet o **Test-PswaAuthorizationRule -UserName &lt;domain\\user | computer\\user&gt; -ComputerName** &lt;computer\_name&gt;. Por ejemplo, **Test-PswaAuthorizationRule –UserName Contoso\\JSmith –ComputerName Contoso\_214**.

Después de configurar una regla de autorización, está listo para que los usuarios autorizados inicien sesión en la consola basada en web y empiecen a usar Windows PowerShell Web Access.

<a href="" id="BKMK_custom"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Implementación personalizada</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Puede instalar la puerta de enlace de Windows PowerShell Web Access en un servidor que ejecuta Windows Server 2012 R2 o Windows Server 2012 mediante el uso del Asistente para agregar roles y características en el Administrador del servidor. Después de instalar Windows PowerShell Web Access, puede personalizar la configuración de la puerta de enlace en el Administrador de IIS.

<a href="" id="BKMK_custom1"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 1: Instalar Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### Para instalar Windows PowerShell Web Access mediante el Asistente para agregar roles y características

1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

    -   En la pantalla **Inicio** de Windows, haga clic en **Administrador del servidor**.

2.  En el menú **Administrar**, haga clic en **Agregar roles y características**.

3.  En la página **Seleccionar tipo de instalación**, seleccione **Instalación basada en características o en roles**. Haga clic en **Siguiente**.

4.  En la página **Seleccionar servidor de destino**, seleccione un servidor del grupo de servidores o un VHD sin conexión. Para seleccionar un VHD sin conexión como servidor de destino, primero seleccione el servidor en el que montará el VHD y, a continuación, seleccione el archivo VHD. Para obtener información sobre cómo agregar servidores al grupo de servidores, consulte la Ayuda del Administrador del servidor. Una vez seleccionado el servidor de destino, haga clic en **Siguiente**.

5.  En la página **Seleccionar características** del asistente, expanda **Windows PowerShell** y seleccione **Windows PowerShell Web Access**.

6.  Se le pedirá que agregue las características necesarias, como .NET Framework 4.5 y los servicios de rol de Servidor web (IIS). Agregue las características necesarias y continúe.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>Al instalar Windows PowerShell Web Access mediante el uso del Asistente para agregar roles y características también se instala Servidor web (IIS), incluido el complemento Administrador de IIS. El complemento y otras herramientas de administración de IIS se instalan de manera predeterminada si usa el Asistente para agregar roles y características. Si instala Windows PowerShell Web Access mediante cmdlets de Windows PowerShell, como se describe en el siguiente procedimiento, las herramientas de administración no se agregan de manera predeterminada.</p></td>
    </tr>
    </tbody>
    </table>

7.  En la página **Confirmar selecciones de instalación**, si los archivos de características para Windows PowerShell Web Access no están almacenados en el servidor de destino seleccionado en el paso 4, haga clic en **Especifique una ruta de acceso de origen alternativa** y proporcione la ruta de acceso a los archivos de características. De lo contrario, haga clic en **Instalar**.

8.  Después de hacer clic en **Instalar**, la página **Progreso de la instalación** mostrará el progreso de la instalación, los resultados y diferentes mensajes, como advertencias, errores o los pasos de configuración posteriores a la instalación necesarios para Windows PowerShell Web Access. Una vez instalado Windows PowerShell Web Access, se le pedirá que revise el archivo Léame, que contiene las instrucciones de configuración necesarias básicas para la puerta de enlace. Estas instrucciones también se incluyen en este tema. La ruta de acceso al archivo Léame es <span class="computerOutputInline">C:\\Windows\\Web\\PowerShellWebAccess\\wwwroot\\README.txt</span>.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 2: Configurar la puerta de enlace</span></a>

------------------------------------------------------------------------

Las instrucciones de esta sección explican cómo instalar la aplicación web Windows PowerShell Web Access en un subdirectorio del sitio web, no en el directorio raíz. Este procedimiento es el equivalente basado en GUI de las acciones realizadas por el cmdlet <span class="code">Install-PswaWebApplication</span>. En esta sección, también se incluyen instrucciones sobre cómo usar el Administrador de IIS para configurar la puerta de enlace de Windows PowerShell Web Access como un sitio web raíz.

-   [Para usar el Administrador de IIS para configurar la puerta de enlace en un sitio web existente](#BKMK_configman)

-   [Para usar el Administrador de IIS para configurar la puerta de enlace como sitio web raíz con un certificado de prueba](#BKMK_configroot)

-   

#### Para usar el Administrador de IIS para configurar la puerta de enlace en un sitio web existente

1.  Abra la consola del Administrador de IIS mediante uno de los siguientes procedimientos.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor. En el menú **Herramientas** del Administrador del servidor, haga clic en **Administrador de Internet Information Services (IIS)**.

    -   En la pantalla **Inicio** de Windows, escriba cualquier parte del nombre **Administrador de Internet Information Services (IIS)**. Haga clic en el acceso directo cuando se muestre en los resultados de **Aplicaciones**.

2.  Cree un nuevo grupo de aplicaciones para Windows PowerShell Web Access. Expanda el nodo del servidor de puerta de enlace en el panel del árbol del Administrador de IIS, seleccione **Grupos de aplicaciones** y haga clic en **Agregar grupo de aplicaciones** en el panel **Acciones**.

3.  Agregue un nuevo grupo de aplicaciones con el nombre **pswa\_pool** o proporcione otro nombre. Haga clic en **Aceptar**.

4.  En el panel del árbol del Administrador de IIS, expanda el nodo del servidor en el que está instalado Windows PowerShell Web Access hasta que aparezca la carpeta **Sitios**. Seleccione la carpeta **Sitios**.

5.  Haga clic con el botón derecho en el sitio web (por ejemplo, **Sitio web predeterminado**) en el que quiere agregar el sitio web de Windows PowerShell Web Access y, después, haga clic en **Agregar aplicación**.

6.  En el campo **Alias**, escriba pswa o proporcione un alias diferente. El alias se convierte en el nombre de directorio virtual. Por ejemplo, **pswa** en la siguiente dirección URL representa el alias especificado en este paso: https://&lt;server\_name&gt;/pswa.

7.  En el campo **Grupo de aplicaciones**, seleccione el grupo de aplicaciones creado en el paso 3.

8.  En el campo **Ruta de acceso física**, busque la ubicación de la aplicación. Puede usar la ubicación predeterminada %windir%/Web/PowerShellWebAccess/wwwroot. Haga clic en **Aceptar**.

9.  Siga los pasos del procedimiento [Para configurar un certificado SSL en el Administrador de IIS](#BKMK_cert) en este tema.

10. <span class="label">Paso de seguridad opcional:</span> con el sitio web seleccionado en el panel del árbol, haga doble clic en **Configuración de SSL** en el panel de contenido. Seleccione **Requerir SSL** y, a continuación, en el panel **Acciones**, haga clic en **Aplicar**. De forma opcional, en el panel **Configuración de SSL**, puede requerir que los usuarios que se conecten al sitio web de Windows PowerShell Web Access tengan certificados de cliente. Los certificados de cliente ayudan a comprobar la identidad de un usuario del dispositivo cliente. Para más información sobre el modo en que se puede incrementar la seguridad de Windows PowerShell Web Access al requerir certificados de cliente, consulte [Reglas de autorización y características de seguridad de Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx) en esta guía.

11. Abra una sesión del explorador en un dispositivo cliente. Para más información sobre los dispositivos y exploradores admitidos, consulte [Compatibilidad con exploradores y dispositivos cliente](#BKMK_browser) en este tema.

12. Abra el nuevo sitio web de Windows PowerShell Web Access, https://&lt; *gateway\_server\_name*&gt;/pswa.

    El explorador debe mostrar la página de inicio de sesión de la consola de Windows PowerShell Web Access.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>No podrá iniciar sesión hasta que se haya concedido a los usuarios acceso al sitio web mediante la adición de reglas de autorización.</p></td>
    </tr>
    </tbody>
    </table>

13. En una sesión de Windows PowerShell abierta con derechos de usuario elevados (Ejecutar como administrador), ejecute el siguiente script, en el que *application\_pool\_name* representa el nombre del grupo de aplicaciones creado en el paso 3, para conceder al grupo de aplicaciones derechos de acceso en el archivo de autorización.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_c1a80a93-8fcf-4beb-a025-5f81bfb8bdae'); "Copy to clipboard.")

        $applicationPoolName = "<application_pool_name>"
        $authorizationFile = "C:\windows\web\powershellwebaccess\data\AuthorizationRules.xml"
        c:\windows\system32\icacls.exe $authorizationFile /grant ('"' + "IIS AppPool\$applicationPoolName" + '":R') > $null

    Para ver los derechos de acceso existentes en el archivo de autorización, ejecute el siguiente comando:

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_ac2179c2-9548-4a17-8663-267fdd105380'); "Copy to clipboard.")

        c:\windows\system32\icacls.exe $authorizationFile

#### Para usar el Administrador de IIS para configurar la puerta de enlace como sitio web raíz con un certificado de prueba

1.  Abra la consola del Administrador de IIS mediante uno de los siguientes procedimientos.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor. En el menú **Herramientas** del Administrador del servidor, haga clic en **Administrador de Internet Information Services (IIS)**.

    -   En la pantalla **Inicio** de Windows, escriba cualquier parte del nombre **Administrador de Internet Information Services (IIS)**. Haga clic en el acceso directo cuando se muestre en los resultados de **Aplicaciones**.

2.  En el panel del árbol del Administrador de IIS, expanda el nodo del servidor en el que está instalado Windows PowerShell Web Access hasta que aparezca la carpeta **Sitios**. Seleccione la carpeta **Sitios**.

3.  En el panel **Acciones**, haga clic en **Agregar sitio web**.

4.  Escriba un nombre para el sitio web, como **Windows PowerShell Web Access**.

5.  Se creará automáticamente un grupo de aplicaciones para el nuevo sitio web. Para usar un grupo de aplicaciones diferente, haga clic en **Seleccionar** para seleccionar un grupo de aplicaciones a fin de asociarlo con el nuevo sitio web. Seleccione el grupo de aplicaciones alternativo en el cuadro de diálogo **Seleccionar grupo de aplicaciones** y, después, haga clic en **Aceptar**.

6.  En el cuadro de texto **Ruta de acceso física**, vaya a %*windir*%/Web/PowerShellWebAccess/wwwroot.

7.  En el campo **Tipo** del área **Enlace**, seleccione **https**.

8.  Asigne al sitio web un número de puerto que no esté en uso por otro sitio o aplicación. Para encontrar puertos abiertos, puede ejecutar el comando **netstat** en una ventana del símbolo del sistema. El número de puerto predeterminado es 443.

    Cambie el puerto predeterminado si otro sitio web ya está usando el 443 o por algún otro motivo de seguridad. Si otro sitio web que se ejecuta en el servidor de puerta de enlace está usando el puerto seleccionado, se mostrará una advertencia cuando haga clic en **Aceptar** en el cuadro de diálogo **Agregar sitio web**. Debe usar un puerto que no esté en uso para ejecutar Windows PowerShell Web Access.

9.  De forma opcional, si es necesario en su organización, especifique un nombre de host que tenga sentido para la organización y los usuarios, como **www.contoso.com**. Haga clic en **Aceptar**.

10. En un entorno de producción más seguro, se recomienda encarecidamente proporcionar un certificado válido firmado por una CA. Debe proporcionar un certificado SSL, porque los usuarios solo se pueden conectar a Windows PowerShell Web Access a través de un sitio web HTTPS. Consulte [Para configurar un certificado SSL en el Administrador de IIS](#BKMK_cert) en este tema para obtener más información sobre cómo obtener un certificado.

11. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Agregar sitio web**.

12. En una sesión de Windows PowerShell abierta con derechos de usuario elevados (Ejecutar como administrador), ejecute el siguiente script, en el que *application\_pool\_name* representa el nombre del grupo de aplicaciones creado en el paso 4, para conceder al grupo de aplicaciones derechos de acceso en el archivo de autorización.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_35ae9944-ca44-4af7-9c96-616083b3e3db'); "Copy to clipboard.")

        $applicationPoolName = "<application_pool_name>"
        $authorizationFile = "C:\windows\web\powershellwebaccess\data\AuthorizationRules.xml"
        c:\windows\system32\icacls.exe $authorizationFile /grant ('"' + "IIS AppPool\$applicationPoolName" + '":R') > $null

    Para ver los derechos de acceso existentes en el archivo de autorización, ejecute el siguiente comando:

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_0eb6d70a-2807-498b-b088-fa5233ed68d5'); "Copy to clipboard.")

        c:\windows\system32\icacls.exe $authorizationFile

13. Con el nuevo sitio web seleccionado en el panel del árbol del Administrador de IIS, haga clic en **Inicio** en el panel **Acciones** para iniciar el sitio web.

14. Abra una sesión del explorador en un dispositivo cliente. Para más información sobre los dispositivos y exploradores admitidos, consulte [Compatibilidad con exploradores y dispositivos cliente](#BKMK_browser) en este documento.

15. Abra el nuevo sitio web de Windows PowerShell Web Access.

    Como el sitio web raíz apunta a la carpeta Windows PowerShell Web Access, el explorador debería mostrar la página de inicio de sesión de Windows PowerShell Web Access cuando abra https://&lt; *gateway\_server\_name*&gt;. No será necesario que agregue **/pswa** a la dirección URL.

    <table>
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th><span><img src="https://i-technet.sec.s-msft.com/dynimg/IC101471.jpeg" title="System_CAPS_note" alt="System_CAPS_note" id="s-e6f6a65cf14f462597b64ac058dbe1d0-system-media-system-caps-note" /></span><span class="alertTitle">Nota </span></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>No podrá iniciar sesión hasta que se haya concedido a los usuarios acceso al sitio web mediante la adición de reglas de autorización.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 3: Configurar una regla de autorización restrictiva</span></a>

------------------------------------------------------------------------

Una vez instalado Windows PowerShell Web Access y configurada la puerta de enlace, los usuarios podrán abrir la página de inicio de sesión en un explorador, pero no podrán iniciar sesión hasta que el administrador de Windows PowerShell Web Access les conceda acceso de manera explícita. El control de acceso de Windows PowerShell Web Access se administra mediante el conjunto de cmdlets de Windows PowerShell descrito en la siguiente tabla. No existe una GUI comparable para agregar o administrar reglas de autorización. Para obtener más información sobre los cmdlets de Windows PowerShell Web Access, consulte los temas de referencia de cmdlet, [Windows PowerShell Web Access Cmdlets](https://technet.microsoft.com/library/hh918342.aspx) (Cmdlets de Windows PowerShell Web Access).

Para obtener más información sobre la seguridad y las reglas de autorización de Windows PowerShell Web Access, consulte [Reglas de autorización y características de seguridad de Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx).

#### Para agregar una regla de autorización restrictiva

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell**, en la barra de tareas y, luego,en **Ejecutar como administrador**.

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en **Windows PowerShell** y, luego, en **Ejecutar como administrador**.

2.  <span class="label">Paso opcional para restringir el acceso del usuario mediante configuraciones de sesión:</span> Compruebe que las configuraciones de sesión que quiere usar en las reglas ya existen. Si aún no se han creado, use las instrucciones para crear configuraciones de sesión proporcionadas en [about\_Session\_Configuration\_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx) en MSDN.

3.  Escriba lo siguiente y, después, presione **Entrar**.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_4df22c91-f56f-4bb5-91e7-99f9b365ed5d'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Esta regla de autorización concede a un usuario específico acceso a un equipo de la red al que tiene acceso normalmente, con acceso a una configuración de sesión determinada en el ámbito de las necesidades de cmdlet y scripting típicas del usuario. En el siguiente ejemplo, se concede acceso a un usuario llamado <span class="code">JSmith</span> en el dominio <span class="code">Contoso</span> para administrar el equipo <span class="code">Contoso\_214</span> y usar una configuración de sesión llamada <span class="code">NewAdminsOnly</span>.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_efc3999a-2905-453f-86cd-014b41658ffc'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Compruebe que se ha creado la regla ejecutando el cmdlet **Get-PswaAuthorizationRule** cmdlet o **Test-PswaAuthorizationRule -UserName &lt;domain\\user | computer\\user&gt; -ComputerName** &lt;computer\_name&gt;. Por ejemplo, **Test-PswaAuthorizationRule –UserName Contoso\\JSmith –ComputerName Contoso\_214**.

Después de configurar una regla de autorización, está listo para que los usuarios autorizados inicien sesión en la consola basada en web y empiecen a usar Windows PowerShell Web Access.

<a href="" id="BKMK_configcert"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Configurar un certificado original</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_4" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Para un entorno de producción seguro, se recomienda usar un certificado SSL válido firmado por una entidad de certificación (CA). En el procedimiento descrito en esta sección se explica cómo obtener y aplicar un certificado SSL válido de una CA.

### Para configurar un certificado SSL en el Administrador de IIS

1.  En el panel del árbol del Administrador de IIS, seleccione el servidor en el que está instalado Windows PowerShell Web Access.

2.  En el panel de contenido, haga doble clic en **Certificados de servidor**.

3.  En el panel **Acciones**, realice una de las acciones siguientes. Para obtener más información sobre la configuración de certificados de servidor en IIS, consulte [Configurar certificados de servidor en IIS 7](https://technet.microsoft.com/library/cc732230.aspx).

    -   Haga clic en **Importar** para importar un certificado válido existente desde una ubicación de la red.

    -   Haga clic en **Crear una solicitud de certificado** para solicitar un certificado de una CA como VeriSign™, Thawte o GeoTrust®. El nombre común del certificado debe coincidir con el encabezado host de la solicitud. Por ejemplo, si el explorador del cliente solicita http://www.contoso.com/, el nombre común también deberá ser http://www.contoso.com/. Esta es la opción más segura y recomendada para proporcionar la puerta de enlace de Windows PowerShell Web Access con un certificado.

    -   Haga clic en **Crear un certificado autofirmado** para crear un certificado para usarlo inmediatamente y, más tarde, hágalo firmar por una CA, si lo desea. Especifique un nombre descriptivo para el certificado autofirmado, como **Windows PowerShell Web Access**. Esta opción no se considera segura y solo se recomienda para un entorno de prueba privado.

4.  Una vez creado u obtenido el certificado, seleccione el sitio web al que se aplicará (por ejemplo, **Sitio web predeterminado**) en el panel del árbol del Administrador de IIS y, a continuación, haga clic en **Enlaces** en el panel **Acciones**.

5.  En el cuadro de diálogo **Agregar enlace de sitio**, agregue un enlace **https** para el sitio, si aún no se muestra ninguno. Si no usa un certificado autofirmado, especifique el nombre de host del paso 3 de este procedimiento. Si usa un certificado autofirmado, este paso no es necesario.

6.  Seleccione el certificado obtenido o creado en el paso 3 de este procedimiento y, después, haga clic en **Aceptar**.

<a href="" id="BKMK_using"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Uso de la consola de Windows PowerShell basada en web</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_5" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Una vez instalado Windows PowerShell Web Access y configurada la puerta de enlace como se describe en este tema, la consola de Windows PowerShell basada en web estará lista para usarse. Para obtener más información sobre la introducción a la consola basada en web, consulte [Uso de la consola de Windows PowerShell basada en web](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx).

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Consulte también</span></a>
<a href="/en-us/library/hh831611(v=ws.11).aspx#Anchor_6" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Documentación de Internet Information Services (IIS) 7.0](https://technet.microsoft.com/library/cc753433.aspx)
[Ayuda del Administrador de IIS 7.0](https://technet.microsoft.com/library/cc732664.aspx)
[Configurar la seguridad de los servidores web (IIS 7)](https://technet.microsoft.com/library/cc731278.aspx)
[Recursos de implementación de IPsec](https://technet.microsoft.com/network/bb531150)

<span>Mostrar:</span> heredado protegido

<span class="stdr-votetitle">¿Le resultó útil esta página?</span>
Sí No

¿Algún otro comentario?

<span class="stdr-count"><span class="stdr-charcnt">1500</span> caracteres restantes</span> Enviar Omitir esto

<span class="stdr-thankyou">Gracias.</span> <span class="stdr-appreciate">Apreciamos sus comentarios.</span>

[Administre su perfil](https://social.technet.microsoft.com/profile)

|

<a href="javascript:void(0)" id="SiteFeedbackLinkOpener"><span id="FeedbackButton" class="FeedbackButton clip20x21"> <img src="https://i-technet.sec.s-msft.com/Areas/Epx/Content/Images/ImageSprite.png?v=635975720914499532" alt="Site Feedback" id="feedBackImg" class="cl_footer_feedback_icon" /> </span> Comentarios del sitio</a> Comentarios del sitio

<a href="javascript:void(0)" id="SiteFeedbackLinkCloser">x</a>

Cuéntenos su experiencia...

¿Se ha cargado rápido la página?

<span> Sí<span> </span></span> <span> No<span> </span></span>

¿Le gusta el diseño de página?

<span> Sí<span> </span></span> <span> No<span> </span></span>

Más información

-   [Boletín de Flash](https://technet.microsoft.com/cc543196.aspx)
-   |
-   [Póngase en contacto con nosotros](https://technet.microsoft.com/cc512759.aspx)
-   |
-   [Declaración de privacidad](https://privacy.microsoft.com/privacystatement)
-   |
-   [Términos de uso](https://technet.microsoft.com/cc300389.aspx)
-   |
-   [Marcas comerciales](https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/)
-   |

© 2016 Microsoft

© 2016 Microsoft

Los scripts y el código de terceros vinculados a este sitio web o a los que este hace referencia se le ofrecen a usted bajo licencia de las partes propietarias de dicho código, no de Microsoft. Consulte los términos de uso de Ajax CDN para ASP.NET en http://www.asp.net/ajaxlibrary/CDN.ashx.
<img src="https://m.webtrends.com/dcsjwb9vb00000c932fd0rjc7_5p3t/njs.gif?dcsuri=/nojavascript&amp;WT.js=No" alt="DCSIMG" id="Img1" width="1" height="1" />




<!--HONumber=Jun16_HO4-->


