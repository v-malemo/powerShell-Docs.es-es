#  Desinstalar Windows PowerShell Web Access

Actualizado: 24 de junio de 2013

Se aplica a: Windows Server 2012 R2, Windows Server 2012

Siga los pasos facilitados en este tema para eliminar el sitio web y la aplicación de Windows PowerShell Web Access del servidor de puerta de enlace en el que se está ejecutando Windows Server 2012 R2 o Windows Server 2012. Antes de comenzar, notifique a los usuarios de la consola basada en web de que quitará el sitio web.

<a href="" id="BKMK_uninstall"></a>

------------------------------------------------------------------------

Antes de desinstalar Windows PowerShell Web Access del servidor de puerta de enlace, ejecute el cmdlet <span class="code">Uninstall-PswaWebApplication</span> para quitar el sitio web y las aplicaciones web de Windows PowerShell Web Access, o bien use el procedimiento del Administrador de IIS, [para eliminar el sitio web y las aplicaciones web de Windows PowerShell Web Access mediante el Administrador de IIS](#BKMK_delsite)..

Al desinstalar Windows PowerShell Web Access, no se desinstalan IIS ni las demás características que se instalaron automáticamente porque eran necesarias para la ejecución de Windows PowerShell Web Access. El proceso de desinstalación deja instaladas las características de las cuales dependía Windows PowerShell Web Access. Si necesita desinstalarlas, puede hacerlo de manera individual.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Desinstalación (rápida) recomendada</span></a>
<a href="/en-us/library/dn282396(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Los procedimientos de esta sección le ayudarán a desinstalar tanto la aplicación web como la característica de Windows PowerShell Web Access mediante cmdlets de Windows PowerShell.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 1: Eliminar la aplicación web</span></a>

------------------------------------------------------------------------

Si ha especificado su propio nombre de sitio web personalizado, agregue el parámetro <span class="code">WebsiteName</span> a su comando y especifique el nombre del sitio web. Si ha usado una aplicación web personalizada (no la aplicación predeterminada, **pswa**, agregue el parámetro <span class="code">WebApplicationName</span> a su comando y especifique el nombre de la aplicación web.

#### Para quitar el sitio web y las aplicaciones web mediante el cmdlet Uninstall-PswaWebApplication

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell** en la barra de tareas.

    -   En la pantalla **Inicio** de Windows, haga clic en **Windows PowerShell**..

2.  Escriba **Uninstall-PswaWebApplication** y luego presione **Entrar**..

3.  Si usa un certificado de prueba, agregue el parámetro <span class="code">DeleteTestCertificate</span> al cmdlet, como se muestra en el siguiente ejemplo.

    [Copiar](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_28147344-ab2f-49e7-b1c2-6dbe649d4366'); "Copiar al Portapapeles.")

        Uninstall-PswaWebApplication -DeleteTestCertificate

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 2: Desinstalar Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### Para desinstalar Windows PowerShell Web Access mediante cmdlets de Windows PowerShell

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados. Si ya se ha abierto una sesión, vaya al siguiente paso.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell** en la barra de tareas y luego haga clic en **Ejecutar como administrador**..

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en **Windows PowerShell** y luego haga clic en **Ejecutar como administrador**..

2.  Escriba lo siguiente y luego presione **Entrar**, donde *computer_name* representa un servidor remoto del cual desea quitar Windows PowerShell Web Access. El parámetro <span class="code">–Restart</span> reinicia automáticamente los servidores de destino, si es necesario para la eliminación.

    [Copiar](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_7b534520-f292-471f-89e3-a1079c03e369'); "Copiar al Portapapeles.")

        Uninstall-WindowsFeature –Name WindowsPowerShellWebAccess -ComputerName <computer_name> -Restart

    Para quitar roles y características de un VHD sin conexión, debe agregar los parámetros <span class="code">-ComputerName</span> y <span class="code">-VHD</span> . El parámetro <span class="code">-ComputerName</span> contiene el nombre del servidor en el que se montará el VHD y el parámetro <span class="code">-VHD</span> contiene la ruta de acceso al archivo VHD en el servidor especificado.

    [Copiar](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_5d8f91ee-b91a-4653-b7df-e745187fd72d'); "Copiar al Portapapeles.")

        Uninstall-WindowsFeature –Name WindowsPowerShellWebAccess –VHD <path> -ComputerName <computer_name> -Restart

3.  Para comprobar la eliminación de Windows PowerShell Web Access una vez finalizada, abra la página **Todos los servidores** del Administrador del servidor, seleccione un servidor del cual se haya quitado la característica y visualice el icono **Roles y características** en la página del servidor seleccionado. También puede ejecutar el cmdlet <span class="code">Get-WindowsFeature</span> destinado al servidor seleccionado (Get-WindowsFeature -ComputerName &lt;*computer\_name*&gt;) para ver una lista de los roles y las características instalados en el servidor.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Desinstalación personalizada</span></a>
<a href="/en-us/library/dn282396(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Los procedimientos de esta sección le ayudarán a desinstalar tanto la aplicación web como la característica de Windows PowerShell Web Access mediante el Asistente para quitar roles y características del Administrador del servidor y la consola de Administrador de IIS.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 1: Eliminar la aplicación web</span></a>

------------------------------------------------------------------------

#### Para eliminar el sitio web y las aplicaciones web de Windows PowerShell Web Access mediante el Administrador de IIS

1.  Abra la consola del Administrador de IIS mediante uno de los siguientes procedimientos. Si ya se ha abierto, vaya al siguiente paso.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor. En el menú **Herramientas** del Administrador del servidor, haga clic en **Administrador de Internet Information Services (IIS)**..

    -   En la pantalla **Inicio** de Windows, escriba cualquier parte del nombre **Administrador de Internet Information Services (IIS)**. Haga clic en el acceso directo cuando se muestre en los resultados de **Aplicaciones**.

2.  En el panel del árbol del Administrador de IIS, seleccione el sitio web que ejecuta la aplicación web de Windows PowerShell Web Access.

3.  En el panel **Acciones**, en **Administrar sitio web**, haga clic en **Detener**..

4.  En el panel del árbol, haga clic con el botón derecho en la aplicación web del sitio web que ejecuta la aplicación web de Windows PowerShell Web Access y luego haga clic en **Quitar**..

5.  En el panel del árbol, seleccione **Grupos de aplicaciones**, seleccione la carpeta del grupo de aplicaciones de Windows PowerShell Web Access, haga clic en **Detener** en el panel **Acciones** y luego haga clic en **Quitar** en el panel de contenido.

6.  Cierre el Administrador de IIS.

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
    <td><p>El certificado no se elimina durante la desinstalación. Si creó un certificado autofirmado o usó un certificado de prueba y desea quitarlo, hágalo en el Administrador de IIS.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Paso 2: Desinstalar Windows PowerShell Web Access</span></a>

------------------------------------------------------------------------

#### Para desinstalar Windows PowerShell Web Access mediante el Asistente para quitar roles y características

1.  Si ya se ha abierto el Administrador del servidor, vaya al siguiente paso. Si todavía no se ha abierto el Administrador del servidor, ábralo mediante una de las siguientes acciones.

    -   En el escritorio de Windows, haga clic en **Administrador del servidor** en la barra de tareas de Windows para iniciar el Administrador del servidor.

    -   En la pantalla **Inicio** de Windows, haga clic en **Administrador del servidor**..

2.  En el menú **Administrar**, haga clic en **Quitar roles y funciones**..

3.  En la página **Seleccionar servidor de destino**, seleccione el servidor o VHD sin conexión del cual desea quitar la característica. Para seleccionar un VHD sin conexión, primero seleccione el servidor en el que montará el VHD y, a continuación, seleccione el archivo VHD. Una vez seleccionado el servidor de destino, haga clic en **Siguiente**..

4.  Vuelva a hacer clic en **Siguiente** para ir directamente a la página **Quitar características**.

5.  Desactive la casilla **Windows PowerShell Web Access** y luego haga clic en **Siguiente**..

6.  En la página **Confirmar selecciones de eliminación**, haga clic en **Quitar**..

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Consulte también</span></a>
<a href="/en-us/library/dn282396(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Instalación y uso de Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx)
[Ayuda del Administrador de IIS 7.0](https://technet.microsoft.com/library/cc732664.aspx)

<span>Mostrar:</span> heredado protegido

<span class="stdr-votetitle">¿Le resultó útil esta página?</span>
Sí
No

¿Algún otro comentario?

<span class="stdr-count"><span class="stdr-charcnt">1500</span> caracteres restantes</span>
Enviar
Omitir esto

<span class="stdr-thankyou">Gracias.</span> <span class="stdr-appreciate">Apreciamos sus comentarios.</span>

[Administre su perfil](https://social.technet.microsoft.com/profile)

|

<a href="javascript:void(0)" id="SiteFeedbackLinkOpener"><span id="FeedbackButton" class="FeedbackButton clip20x21"> <img src="https://i-technet.sec.s-msft.com/Areas/Epx/Content/Images/ImageSprite.png?v=635975720914499532" alt="Site Feedback" id="feedBackImg" class="cl_footer_feedback_icon" /> </span> Comentarios del sitio</a>
Comentarios del sitio

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


<!--HONumber=May16_HO2-->


