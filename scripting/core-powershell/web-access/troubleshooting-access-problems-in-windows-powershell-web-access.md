#  Solución de problemas de acceso en Windows PowerShell Web Access

Actualizado: 24 de junio de 2013

Se aplica a: Windows Server 2012 R2, Windows Server 2012

<a href="" id="BKMK_trouble"></a>

------------------------------------------------------------------------

En la siguiente tabla, se identifican algunos problemas comunes que pueden experimentar los usuarios cuando intentan conectarse a un equipo remoto mediante Windows PowerShell Web Access y se incluyen sugerencias para resolverlos.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Problema</p></th>
<th><p>Causa y solución posibles</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Error de inicio de sesión</p></td>
<td><p>El error puede producirse por uno de los siguientes motivos.</p>
<ul>
<li><p>Una regla de autorización que concede al usuario acceso al equipo, o a una configuración de sesión específica en un equipo remoto, no existe. La seguridad de Windows PowerShell Web Access es restrictiva. Se debe conceder a los usuarios acceso a los equipos remotos de manera explícita mediante reglas de autorización. Para más información sobre la creación de reglas de autorización, consulte <a href="https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx">Authorization Rules and Security Features of Windows PowerShell Web Access</a> (Reglas de autorización y características de seguridad de Windows PowerShell Web Access) en esta guía.</p></li>
<li><p>El usuario no tiene acceso autorizado al equipo de destino. Este se determina mediante listas de control de acceso (ACL). Para más información, consulte “Signing in to Windows PowerShell Web Access” (Inicio de sesión en Windows PowerShell Web Access) en <a href="https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx">Use the Web-based Windows PowerShell Console</a> (Uso de la consola de Windows PowerShell basada en web) o visite el <a href="https://msdn.microsoft.com/library/windows/desktop/ee706585.aspx">blog del equipo de Windows PowerShell.</a>.</p>
<ul>
<li><p>Es posible que la administración remota de Windows PowerShell no esté habilitada en el equipo de destino. Compruebe que esté habilitada en el equipo al que intenta conectarse el usuario. Para más información, consulte "Cómo configurar el equipo para la comunicación remota" en <a href="https://technet.microsoft.com/library/dd315349.aspx">about_Remote_Requirements</a> en los temas de Ayuda conceptual de Windows PowerShell.</p></li>
</ul></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Cuando los usuarios intentan iniciar sesión en Windows PowerShell Web Access en una ventana de Internet Explorer, aparecerá la página <strong>Error interno del servidor</strong>, o bien Internet Explorer dejará de responder. Este problema es específico de Internet Explorer.</p></td>
<td><p>Esto puede ocurrir si el usuario ha iniciado sesión con un nombre de dominio que contiene caracteres en chino o si el nombre del servidor de puerta de enlace contiene uno o más caracteres en este idioma. Para solucionar este problema, el usuario debe <a href="http://ie.microsoft.com/testdrive/info/downloads/Default.html">instalar y ejecutar Internet Explorer 10</a> y luego llevar a cabo los siguientes pasos.</p>
<ol>
<li><p>Cambie la configuración <strong>Modo de documento de Internet Explorer</strong> a <strong>Estándar de IE10.</strong>.</p>
<ol>
<li><p>Presione <strong>F12</strong> para abrir la consola Herramientas de desarrollo.</p></li>
<li><p>En Internet Explorer 10, haga clic en <strong>Modo de explorador</strong> y luego seleccione <strong>Internet Explorer 10.</strong>.</p></li>
<li><p>Haga clic en <strong>Modo de documento</strong> y luego en <strong>Estándar de IE10.</strong>.</p></li>
<li><p>Vuelva a presionar <strong>F12</strong> para cerrar la consola Herramientas de desarrollo.</p></li>
</ol></li>
<li><p>Deshabilite la configuración automática de servidor proxy.</p>
<ol>
<li><p>En Internet Explorer 10, haga clic en <strong>Herramientas</strong> y luego en <strong>Opciones de Internet.</strong>.</p></li>
<li><p>En el cuadro de diálogo <strong>Opciones de Internet</strong>, en la pestaña <strong>Conexiones</strong>, haga clic en <strong>Configuración de LAN.</strong>.</p></li>
<li><p>Desactive la casilla <strong>Detectar la configuración automáticamente</strong>. Haga clic en<strong>Aceptar</strong> y, de nuevo, en <strong>Aceptar</strong> para cerrar el cuadro de diálogo <strong>Opciones de Internet</strong>.</p></li>
</ol></li>
</ol></td>
</tr>
<tr class="odd">
<td><p>Error al conectarse a un equipo de grupo de trabajo remoto</p></td>
<td><p>Si el equipo de destino es miembro de un grupo de trabajo, use la siguiente sintaxis para proporcionar su nombre de usuario e inicie sesión en el equipo: &lt;<em>nombre_grupo_de_trabajo</em>&gt;\&lt;<em>nombre_de_usuario.</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>No se encuentran las herramientas de administración de Servidor web (IIS) (aún cuando está instalado el rol)</p></td>
<td><p>Si instaló Windows PowerShell Web Access mediante el cmdlet <span class="code">Install-WindowsFeature</span>, las herramientas de administración no se instalan, a menos que el parámetro <span class="code">IncludeManagementTools</span> se agregue al cmdlet. Para ver un ejemplo, consulte el procedimiento sobre cómo instalar Windows PowerShell Web Access mediante cmdlets de Windows PowerShell en <a href="https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx">Instalación y uso de Windows PowerShell Web Access</a>. Puede agregar la consola del Administrador de IIS y otras herramientas de administración de IIS que necesite; para ello, debe seleccionarlas en una sesión del Asistente para agregar roles y características destinada al servidor de puerta de enlace. El Asistente para agregar roles y características se abre en el Administrador del servidor.</p></td>
</tr>
<tr class="odd">
<td><p>El sitio web de Windows PowerShell Web Access no es accesible</p></td>
<td><p>Si la opción Configuración de seguridad mejorada está habilitada en Internet Explorer (IE ESC), puede agregar el sitio web de Windows PowerShell Web Access a la lista de sitios de confianza, o bien puede deshabilitar IE ESC. Puede deshabilitar IE ESC en el icono <strong>Propiedades</strong> de la página <strong>Servidor Local</strong> en el Administrador del servidor.</p></td>
</tr>
<tr class="even">
<td><p>Se muestra el siguiente mensaje de error al intentar establecer la conexión si el servidor de puerta de enlace es el equipo de destino y, además, se encuentra en un grupo de trabajo: <strong>Error de autorización. Compruebe que está autorizado para conectarse al equipo de destino.</strong></p></td>
<td><p>En una situación como esta, especifique el nombre de usuario, el nombre de equipo y el nombre del grupo de usuarios, tal como se muestra en la siguiente tabla. No use un punto (.) solo para representar el nombre de equipo.</p>
<div>
<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Escenario</p></th>
<th><p>Parámetro UserName</p></th>
<th><p>Parámetro UserGroup</p></th>
<th><p>Parámetro ComputerName</p></th>
<th><p>Parámetro ComputerGroup</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>El servidor de puerta de enlace se encuentra en un dominio</p></td>
<td><p><em>Nombre_servidor</em>\<em>nombre_usuario</em>, Localhost\<em>nombre_usuario</em> o .\<em>nombre_usuario</em></p></td>
<td><p><em>Nombre_servidor</em>\<em>grupo_usuarios</em>, Localhost\<em>grupo_usuarios</em> o .\<em>grupo_usuarios</em></p></td>
<td><p>Nombre completo del servidor de puerta de enlace o localhost</p></td>
<td><p><em>Nombre_servidor</em>\<em>grupo_equipos</em>, Localhost\<em>grupo_equipos</em> o .\<em>grupo_equipos</em></p></td>
</tr>
<tr class="even">
<td><p>El servidor de puerta de enlace se encuentra en un grupo de trabajo</p></td>
<td><p><em>Nombre_servidor</em>\<em>nombre_usuario</em>, Localhost\<em>nombre_usuario</em> o .\<em>nombre_usuario</em></p></td>
<td><p><em>Nombre_servidor</em>\<em>grupo_usuarios</em>, Localhost\<em>grupo_usuarios</em> o .\<em>grupo_usuarios</em></p></td>
<td><p>Nombre del servidor</p></td>
<td><p><em>Nombre_servidor</em>\<em>grupo_equipos</em>, Localhost\<em>grupo_equipos</em> o .\<em>grupo_equipos</em></p></td>
</tr>
</tbody>
</table>
</div>
<p>Inicie sesión en un servidor de puerta de enlace como equipo de destino con credenciales que tengan uno de los siguientes formatos.</p>
<ul>
<li><p><em>Nombre_servidor</em>\<em>nombre_usuario</em></p></li>
<li><p>Localhost\<em>nombre_usuario</em></p></li>
<li><p>.\<em>nombre_de_usuario</em></p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>En las reglas de autorización se muestra un identificador de seguridad (SID) en lugar de la sintaxis <em>nombre_usuario</em>/<em>nombre_equipo</em> </p></td>
<td><p>La regla ya no es válida o se produjo un error en la consulta de Servicios de dominio de Active Directory. Por lo general, una regla de autorización no es válida en escenarios donde el servidor de puerta de enlace en algún momento perteneció a un grupo de trabajo, pero más tarde se unió a un dominio.</p></td>
</tr>
<tr class="even">
<td><p>No puede iniciar sesión en un equipo de destino que se ha especificado en las reglas de autorización como una dirección IPv6 con un dominio.</p></td>
<td><p>Las reglas de autorización no admiten direcciones IPv6 en forma de nombre de dominio. Para especificar un equipo de destino mediante una dirección IPv6, use la dirección IPv6 original (que contiene caracteres de dos puntos) en la regla de autorización. Tanto las direcciones IPv6 de dominio como las numéricas (con caracteres de dos puntos) se admiten como nombre del equipo de destino en la página de inicio de sesión de Windows PowerShell Web Access, pero no en las reglas de autorización. Para más información sobre las direcciones IPv6, consulte <a href="https://technet.microsoft.com/library/cc781672.aspx">How IPv6 Works (Cómo funciona IPv6).</a>.</p></td>
</tr>
</tbody>
</table>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Consulte también</span></a>
<a href="/en-us/library/dn282395(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Reglas de autorización y características de seguridad de Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx)
[Uso de la consola de Windows PowerShell basada en web](https://technet.microsoft.com/en-us/library/hh831417(v=ws.11).aspx)
[about\_Remote\_Requirements](https://technet.microsoft.com/library/dd315349.aspx)

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


