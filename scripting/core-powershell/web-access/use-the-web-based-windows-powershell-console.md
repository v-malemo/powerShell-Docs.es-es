#  Uso de la consola de Windows PowerShell basada en web

Actualizado: 24 de junio de 2013

Se aplica a: Windows Server 2012 R2, Windows Server 2012

Windows PowerShell® Web Access permite a los usuarios de Windows PowerShell® iniciar sesión en un sitio web protegido mediante la Capa de sockets seguros (SSL) con el fin de usar sesiones, scripts y cmdlets de Windows PowerShell para administrar un equipo remoto. Como la consola de Windows PowerShell se ejecuta en un explorador web, puede abrirse desde una gran variedad de dispositivos cliente, incluidos teléfonos móviles, Tablet PC, quioscos multimedia informáticos públicos, equipos portátiles o equipos compartidos o prestados. La consola de Windows PowerShell basada en web está destinada a un equipo remoto que especifican los usuarios como parte del proceso de inicio de sesión. En este tema, se describe cómo iniciar sesión en la consola de Windows PowerShell Web Access basada en web y cómo comenzar a usarla.

No se describe cómo usar Windows PowerShell ni cómo ejecutar cmdlets o scripts. Para más información sobre de recursos de scripting y cómo usar Windows PowerShell, consulte la sección Consulte también al final de este tema.

En este tema:

-   [Dispositivos cliente y exploradores admitidos](#BKMK_browser)

-   [Inicio de sesión en Windows PowerShell Web Access](#BKMK_sign)

-   [Cierre de sesión y tiempo de espera agotado](#BKMK_timeout)

-   [Diferencias de la consola de Windows PowerShell basada en web](#BKMK_web)

<a href="" id="BKMK_browser"></a>
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

<a href="" id="BKMK_sign"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Inicio de sesión en Windows PowerShell Web Access</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

El administrador de Windows PowerShell Web Access debe proporcionar una dirección URL que es la dirección del sitio web de la puerta de enlace de Windows PowerShell Web Access de la organización. De manera predeterminada, la dirección de este sitio web es https://&lt;nombre\_servidor&gt;/pswa. Antes de iniciar sesión en Windows PowerShell Web Access, asegúrese de contar con el nombre o la dirección IP del equipo remoto que desea administrar. Debe ser un usuario autorizado en el equipo remoto, y el equipo debe estar configurado para admitir la administración remota. Para más información sobre de la configuración del equipo para que admita la administración remota, consulte el tema sobre cómo [Enable and Use Remote Commands in Windows PowerShell](https://technet.microsoft.com/magazine/ff700227.aspx) (Habilitación y uso de comandos remotos en Windows PowerShell). El método más simple de configurar el equipo para que admita la administración remota consiste en ejecutar el cmdlet **Enable-PSRemoting -force** en el equipo, en una sesión de Windows PowerShell abierta con permisos del usuario elevados (**Ejecutar como administrador**).).

### Para iniciar sesión en Windows PowerShell Web Access

1.  Abra el sitio web de Windows PowerShell Web Access en una pestaña o ventana del explorador de Internet.

2.  En la página de inicio de sesión de Windows PowerShell Web Access, proporcione su nombre de usuario de red, su contraseña y el nombre del equipo que desea administrar (y en el que es un usuario autorizado). Si el administrador de Windows PowerShell Web Access le ha indicado que use un URI a un servidor proxy o sitio personalizado en lugar de un nombre de equipo, seleccione **URI de conexión** en el campo **Tipo de conexión** y luego proporcione el URI.

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
    <td><ul>
    <li><p>Si el equipo de destino se encuentra en un grupo de trabajo, use la siguiente sintaxis para proporcionar su nombre de usuario e iniciar sesión en el equipo:&lt;<em>nombre_grupo_trabajo</em>&gt;\&lt;<em>nombre_usuario.</em>&gt;.</p></li>
    <li><p>Si el equipo de destino es el servidor de puerta de enlace, puede especificar <strong>localhost</strong> en el campo <strong>Nombre de equipo</strong>.</p></li>
    <li><p>Si el equipo de destino es el servidor de puerta de enlace y el servidor de puerta de enlace se encuentra en un grupo de trabajo, puede usar <strong>localhost</strong> en el campo <strong>Nombre de equipo</strong>, pero no use localhost\&lt;<em>nombre_usuario</em>&gt; en el campo <strong>Nombre de usuario</strong>. Tiene que usar &lt;<em>nombre_grupo_trabajo</em>&gt;\&lt;<em>nombre_usuario.</em>&gt;.</p></li>
    </ul></td>
    </tr>
    </tbody>
    </table>

3.  La sección **Configuración de conexión opcional** está relacionada con los requisitos de autorización del equipo remoto que desea administrar. Para más información sobre los parámetros equivalentes a la configuración de conexión opcional, consulte la [ayuda del cmdlet Enter-PSSession](https://technet.microsoft.com/library/dd315384.aspx)..

    Por lo general, las credenciales que use para pasar a través de la puerta de enlace de Windows PowerShell Web Access serán las mismas que reconoce el equipo remoto que desea administrar. No obstante, si desea usar credenciales diferentes de las especificadas en el paso 2 para administrar el equipo remoto, expanda la sección **Configuración de conexión opcional** y proporcione las credenciales alternativas. De lo contrario, vaya al paso 6.

4.  Si el administrador de Windows PowerShell Web Access ha creado una configuración de sesión personalizada para los usuarios de Windows PowerShell Web Access, escriba el nombre de la configuración de sesión en el campo **Nombre de la configuración**. Para más información sobre las configuraciones de sesión, consulte el tema [about\_Session\_Configurations](https://technet.microsoft.com/library/dd819508.aspx) (Acerca de las configuraciones de sesión) en el sitio web de Microsoft.

5.  Deje el **Tipo de autenticación** establecido en **Predeterminado**, a menos que el administrador de Windows PowerShell Web Access le haya dado otras indicaciones.

6.  Haga clic en **Iniciar sesión**..

<a href="" id="BKMK_timeout"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Cierre de sesión y tiempo de espera agotado</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Una sesión de Windows PowerShell basada en web se cerrará si el usuario:

-   Hace clic en **Cerrar sesión** en la esquina inferior derecha de la consola. (Solo Windows Server 2012)

-   Hace clic en **Guardar** o **Salir** en la esquina inferior derecha de la consola (solo Windows Server 2012 R2). Si hace clic en **Guardar** se guarda y se cierra la sesión de Windows PowerShell Web Access y puede volver a conectarse a la sesión más tarde. Al iniciar sesión en Windows PowerShell Web Access de nuevo, Windows PowerShell Web Access muestra una lista de las sesiones guardadas y puede optar por seleccionar una sesión guardada y conectarse a ella, o iniciar una sesión nueva. El administrador de puerta de enlace configura el número máximo de sesiones abiertas que se permite tener a los usuarios, tanto guardadas como activas.

    Si hace clic en **Salir** se cierra la sesión de Windows PowerShell Web Access sin guardarla.

-   Intenta iniciar sesión para administrar otro equipo remoto en la misma sesión del explorador o en una nueva pestaña de la misma sesión del explorador. (Esto no se aplica si el servidor de puerta de enlace está ejecutando Windows Server 2012 R2; si Windows PowerShell Web Access se ejecuta en Windows Server 2012 R2 se permiten las sesiones de varios usuarios en pestañas nuevas de la misma sesión del explorador.) Para más información sobre cómo usar más de una sesión activa en el mismo equipo, consulte el tema sobre cómo conectarse a varios equipos de destino simultáneamente en la sección [Limitaciones de la consola basada en web](#BKMK_limits) en este tema.

-   No se hay ninguna actividad en la sesión durante 20 minutos. El administrador de la puerta de enlace puede personalizar el período de tiempo de espera de inactividad. Para obtener más información, consulte [Session management](https://technet.microsoft.com/en-us/library/dn282394(v=ws.11).aspx#BKMK_sesmgmt) (Administración de sesiones)..

    -   Si está desconectado de una sesión en la consola basada en web debido a un error de red u otros apagados o errores no planificados y no porque haya cerrado las sesiones usted mismo, las sesiones de Windows PowerShell Web Access continúan ejecutándose, conectadas al equipo de destino, hasta que venza el período de tiempo de espera del cliente. De forma predeterminada, este período de tiempo de espera es de 20 minutos y lo configura el Administrador de puerta de enlace. La sesión se desconecta después de los 20 minutos predeterminados o después del período de tiempo de espera especificado por el administrador de la puerta de enlace, el que sea más corto.

        Si el servidor de puerta de enlace está ejecutando Windows Server 2012 R2, Windows PowerShell Web Access permite a los usuarios volver a conectarse a las sesiones guardadas anteriormente, pero no puede ver o volver a conectarte a las sesiones guardadas hasta que haya vencido el período de tiempo de espera especificado por el administrador de la puerta de enlace.

-   Cierra la pestaña o ventana del explorador.

-   Apaga el dispositivo cliente en el que se ejecuta el explorador o lo desconecta de la red.

-   Ejecuta el comando **Exit** en la consola web. Este comando no funciona si la configuración de sesión a la cual está conectado se ha establecido para admitir el modo [NoLanguage](https://msdn.microsoft.com/library/windows/desktop/system.management.automation.pslanguagemode.aspx) o se encuentra en un espacio de ejecución restringido.

Si desea volver a iniciar sesión, abra de nuevo la página web de Windows PowerShell Web Access e inicie sesión mediante los pasos descritos en [Para iniciar sesión en Windows PowerShell Web Access](#BKMK_signin) en este tema.

<a href="" id="BKMK_web"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Diferencias de la consola de Windows PowerShell basada en web</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_3" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

Tras iniciar sesión en Windows PowerShell Web Access, una consola de Windows PowerShell basada en web se abre en la pestaña o ventana del explorador. Puesto que la consola está conectada al equipo remoto especificado durante el proceso de inicio de sesión, solo aquellos cmdlets o scripts de Windows PowerShell que estén disponibles en el equipo remoto podrán usarse en la consola. Esta sección describe otras limitaciones de las consolas de Windows PowerShell Web Access y las diferencias entre las consolas de Windows PowerShell Web Access y la consola **PowerShell.exe** instalada.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Diferencias funcionales con PowerShell.exe</span></a>

------------------------------------------------------------------------

La mayoría de las funcionalidades del host de Windows PowerShell están disponibles en la consola de Windows PowerShell Web Access basada en web, pero hay algunas características que no se encuentran disponibles.

-   <span class="label">Visualización del progreso anidado.</span>  Windows PowerShell Web Access muestra una GUI de progreso para los cmdlets que notifican el progreso, pero solo se muestra información de progreso de nivel superior.

-   <span class="label">Modificación del color de entrada.</span>  No se puede cambiar el color de entrada (ni el del primer plano ni el del fondo). El estilo de los mensajes detallados, de resultados, de advertencia y de error se puede cambiar mediante la ejecución de un script.

-   <span class="label">PSHostRawUserInterface.</span>  Windows PowerShell Web Access se implementa a través de la administración remota de Windows PowerShell y utiliza un espacio de ejecución remoto. Windows PowerShell Web Access no implementa algunos métodos en esta interfaz; por ejemplo, no implementa ningún comando que escriba en la consola Windows. Los comandos como **PowerTab** no funcionan en Windows PowerShell Web Access.

-   <span class="label">Teclas de función.</span>  Windows PowerShell Web Access no admite algunas teclas de función; en muchos casos, esto se debe a que los comandos están reservados por el explorador.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Tecla de función no admitida</p></th>
<th><p>Acción</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Ctrl+C</p></td>
<td><p>En Windows PowerShell Web Access, la combinación de teclas <strong>Ctrl+C</strong> la utiliza el explorador para copiar contenido. La consola ofrece un botón <strong>Cancelar</strong> y los usuarios también pueden usar <strong>Ctrl+Q</strong> para cancelar comandos.</p></td>
</tr>
<tr class="even">
<td><p>Alt+Espacio, e, l</p></td>
<td><p>Desplazarse por el búfer de pantalla</p></td>
</tr>
<tr class="odd">
<td><p>Alt+Espacio, e, f</p></td>
<td><p>Buscar texto en el búfer de pantalla</p></td>
</tr>
<tr class="even">
<td><p>Alt+Espacio, e, k</p></td>
<td><p>Seleccionar texto para copiarlo desde el búfer de pantalla</p></td>
</tr>
<tr class="odd">
<td><p>Alt+Espacio, e, p</p></td>
<td><p>Pegar contenido del Portapapeles en la consola de Windows PowerShell</p></td>
</tr>
<tr class="even">
<td><p>Alt+Espacio, c</p></td>
<td><p>Cerrar la consola de Windows PowerShell</p></td>
</tr>
<tr class="odd">
<td><p>Ctrl+Inter</p></td>
<td><p>Forzar el cierre de la ventana de Windows PowerShell</p></td>
</tr>
<tr class="even">
<td><p>Ctrl+Inicio</p></td>
<td><p>Eliminar desde el comienzo de la línea de comandos actual</p></td>
</tr>
<tr class="odd">
<td><p>Ctrl+Fin</p></td>
<td><p>Eliminar hasta el final de la línea de comandos</p></td>
</tr>
<tr class="even">
<td><p>F1</p></td>
<td><p>Mover el cursor un carácter a la derecha en la línea de comandos</p></td>
</tr>
<tr class="odd">
<td><p>F2</p></td>
<td><p>Crear un nuevo comando copiando el último comando hasta el carácter que escriba</p></td>
</tr>
<tr class="even">
<td><p>F3</p></td>
<td><p>Completar la línea de comandos con contenido de la última línea de comandos</p></td>
</tr>
<tr class="odd">
<td><p>F4</p></td>
<td><p>Eliminar caracteres desde la posición del cursor</p></td>
</tr>
<tr class="even">
<td><p>F5</p></td>
<td><p>Examinar el historial de comandos hacia atrás. Para acceder a los comandos del historial de comandos en Windows PowerShell Web Access, haga clic en los botones de desplazamiento del <strong>Historial</strong> en la consola basada en web.</p></td>
</tr>
<tr class="odd">
<td><p>F7</p></td>
<td><p>Seleccionar de manera interactiva un comando del historial de comandos</p></td>
</tr>
<tr class="even">
<td><p>F8</p></td>
<td><p>Examinar el historial mostrando los comandos que coinciden con el texto actual</p></td>
</tr>
<tr class="odd">
<td><p>F9</p></td>
<td><p>Ejecutar un comando numerado específico desde el historial</p></td>
</tr>
<tr class="even">
<td><p>Re Pág</p></td>
<td><p>Ejecutar el primer comando del historial</p></td>
</tr>
<tr class="odd">
<td><p>Av Pág</p></td>
<td><p>Ejecutar el último comando del historial</p></td>
</tr>
<tr class="even">
<td><p>Alt+F7</p></td>
<td><p>Borrar la lista del historial de comandos</p></td>
</tr>
</tbody>
</table>

<a href="" id="BKMK_limits"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Limitaciones de la consola basada en web</span></a>

------------------------------------------------------------------------

-   <span class="label">Salto doble.</span>   Puede encontrarse con la limitación del salto doble (o la conexión a un segundo equipo desde la primera conexión) si intenta crear una nueva sesión o trabajar en ella usando Windows PowerShell Web Access. Windows PowerShell Web Access usa un espacio de ejecución remoto y, actualmente, **PowerShell.exe** no admite el establecimiento de una conexión remota a un segundo equipo desde un espacio de ejecución remoto. Si intenta conectarte a un segundo equipo remoto desde una conexión existente usando el cmdlet **Enter-PSSession**, por ejemplo, puede obtener varios errores, como “No se pueden obtener recursos de red.”.

    Para evitar los errores de salto doble, el administrador debe configurar la autenticación CredSSP en el entorno de red de la organización. Para más información sobre la configuración de la autenticación CredSSP, consulte [CredSSP for second-hop remoting](http://blogs.msdn.com/b/powershell/archive/2008/06/05/credssp-for-second-hop-remoting-part-i-domain-account.aspx) (CredSSP para comunicación remota de segundo salto) en el sitio web de Microsoft. También puede proporcionar credenciales explícitas si desea administrar un segundo equipo remoto (es poco probable que las credenciales implícitas permitan el segundo salto).

-   Windows PowerShell Web Access usa y tiene las mismas limitaciones que una sesión remota de Windows PowerShell. Los comandos que llaman directamente a las API de la consola Windows, por ejemplo, los comandos de editores basados en consola o de programas de menú basados en texto, no funcionan porque no leen o escriben en canalizaciones estándar de entrada, de salida o de error. Por lo tanto, los comandos que inician un archivo ejecutable, como **notepad.exe**, o muestran una GUI, como <span class="code">OpenGridView</span> u <span class="code">ogv</span>, no funcionan. Su experiencia se verá afectada por este comportamiento, ya que parecerá que Windows PowerShell Web Access no está respondiendo al comando.

-   La finalización con tabulación no funciona en una configuración de sesión con un espacio de ejecución restringido o en una que se encuentre en modo **NoLanguage**. Si bien los administradores pueden configurar una sesión para que admita la finalización con tabulación, esto se desaconseja por motivos de seguridad, dado que puede exponer la siguiente información a usuarios que no están autorizados.

    -   Rutas de acceso internas del sistema de archivos

    -   Carpetas compartidas en equipos internos

    -   Variables del espacio de ejecución

    -   Espacios de nombres de .NET Framework o tipos cargados

    -   Variables de entorno

-   Los usuarios que han iniciado sesión en una configuración de sesión **NoLanguage** o en un espacio de ejecución restringido en Windows PowerShell Web Access no pueden ejecutar el comando **Exit** para finalizar la sesión. Para cerrar sesión, los usuarios deben hacer clic en **Cerrar sesión** en la página de la consola.

-   <span class="label">Conexión simultánea a varios equipos de destino.</span>   Si se está ejecutando el servidor de puerta de enlace de Windows Server 2012, Windows PowerShell Web Access solo permite una conexión de equipo remoto por sesión del explorador. No permite a los usuarios iniciar sesión una vez y conectarse a varios equipos remotos mediante pestañas del explorador independientes. Al abrir una nueva pestaña o ventana del explorador, Windows PowerShell Web Access le pedirá que desconecte su sesión actual e inicie una nueva sesión, para permitirle conectarse a un nuevo equipo remoto (o al mismo). No obstante, si desea iniciar sesión en dos o varias sesiones individuales en equipos remotos distintos, existe una característica de Internet Explorer que le permite crear una nueva sesión. Para iniciar una nueva sesión del explorador en Internet Explorer, presione **ALT**, abra el menú **Archivo** y luego seleccione **Nueva sesión**. Después, abra el sitio web de Windows PowerShell Web Access en una nueva sesión e inicie sesión para obtener acceso a otro equipo remoto.

    Cuando la puerta de enlace de Windows PowerShell Web Access se está ejecutando en Windows Server 2012 R2, los usuarios pueden abrir varias conexiones a equipos remotos en pestañas diferentes del explorador. Si quiere abrir más de una conexión a un equipo remoto usando la consola de Windows PowerShell basada en web, póngase en contacto con su administrador de puerta de enlace de Windows PowerShell Web Access para ver si esta característica es compatible con el servidor de puerta de enlace.

-   <span class="label">Sesiones persistentes de Windows PowerShell (reconexión).</span>   Una vez agotado el tiempo de espera de la puerta de enlace de Windows PowerShell Web Access, se cierra la conexión remota entre esa puerta de enlace y el equipo de destino. Esto detiene cualquier cmdlet o script que se encuentre en curso actualmente. Se recomienda usar la infraestructura **-Job** de Windows PowerShell al llevar a cabo tareas de ejecución prolongadas, para que pueda iniciar trabajos, desconectarse del equipo, volver a conectarte más tarde y hacer que los trabajos sean persistentes. Otra ventaja de utilizar cmdlets **-Job** es que puede iniciarlos mediante Windows PowerShell Web Access, cerrar la sesión y volver a conectarse más tarde, ya sea mediante la ejecución de Windows PowerShell Web Access u otro host (como el Entorno de scripting integrado (ISE) de Windows PowerShell®).

-   <span class="label">Cambiar el tamaño de la consola.</span>   Se puede cambiar el tamaño de la ventana de la consola de **PowerShell.exe** de las tres maneras siguientes.

    -   Arrastrar y ajustar el tamaño de la ventana de la consola con el mouse

    -   Cambiar las propiedades de alto y ancho mediante una GUI para propiedades de consola

    -   Cambiar el alto y el ancho de la ventana de la consola mediante un cmdlet

        La ventana de la consola de Windows PowerShell Web Access se puede configurar mediante cmdlets, de la siguiente manera. En el siguiente ejemplo, un usuario cambia el ancho de la consola de Windows PowerShell Web Access a **20**..

        [Copiar](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_778d5e55-9195-4bd7-b313-d1fbca7876e4'); "Copiar al Portapapeles.")

            $newSize = $Host.UI.RawUI.WindowSize
            $newSize.Width = $newSize.Width - 20

            $oldSize = $Host.UI.RawUI.WindowSize

            $Host.UI.RawUI.WindowSize = $newSize

        Puede cambiar el alto de la consola de modo similar.

        En el [blog del equipo de Windows PowerShell](http://blogs.msdn.com/b/powershell/), encontrará más ejemplos para personalizar la visualización de la consola..

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Consulte también</span></a>
<a href="/en-us/library/hh831417(v=ws.11).aspx#Anchor_4" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Referencia sobre cmdlets de Windows PowerShell](https://technet.microsoft.com/library/ee407531(ws.10).aspx)
[Windows PowerShell en Microsoft TechNet](https://technet.microsoft.com/library/bb978526.aspx)
[Repositorio del centro de scripts de TechNet](http://gallery.technet.microsoft.com/scriptcenter)
[Centro de scripts: blog Hey, Scripting Guy!](https://technet.microsoft.com/scriptcenter)
[Blog del equipo de Windows PowerShell](http://blogs.msdn.com/b/powershell/)

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


