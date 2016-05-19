# Reglas de autorización y características de seguridad de Windows PowerShell Web Access

Actualizado: 24 de junio de 2013

Se aplica a: Windows Server 2012 R2, Windows Server 2012

Windows PowerShell® Web Access en Windows Server® 2012 R2 y Windows Server® 2012 tienen un modelo de seguridad restrictiva. Los usuarios tienen que tener el acceso concedido explícitamente antes de iniciar sesión en la puerta de enlace de Windows PowerShell ISE y usar la consola basada en web de Windows PowerShell.

-   [Configurar las reglas de autorización y la seguridad del sitio](#BKMK_auth)

-   [Administración de sesiones](#BKMK_sesmgmt)


Una vez instalado Windows PowerShell Web Access y configurada la puerta de enlace, los usuarios podrán abrir la página de inicio de sesión en un explorador, pero no podrán iniciar sesión hasta que el administrador de Windows PowerShell Web Access les conceda acceso de manera explícita. El control de acceso de Windows PowerShell Web Access se administra mediante el conjunto de cmdlets de Windows PowerShell descrito en la siguiente tabla. No existe una GUI comparable para agregar o administrar reglas de autorización. Para más información sobre los cmdlets de Windows PowerShell Web Access, consulte los temas de referencia de cmdlet, [Cmdlets de Windows PowerShell Web Access](https://technet.microsoft.com/library/hh918342.aspx).

Los administradores pueden definir entre 0 y *n* reglas de autenticación para Windows PowerShell Web Access. La seguridad predeterminada es restrictiva más que permisiva; si no se define ninguna regla de autenticación, ningún usuario tendrá acceso a ningún sitio.

Add-PswaAuthorizationRule y Test-PswaAuthorizationRule de Windows Server 2012 R2 incluyen un parámetro de credenciales que le permite agregar y probar reglas de autorización de Windows PowerShell Web Access desde un equipo remoto o desde una sesión activa de Windows PowerShell Web Access. Como ocurre con otros cmdlets de Windows PowerShell que tienen un parámetro de credenciales, puede especificar un objeto PSCredential como valor del parámetro. Para crear un objeto PSCredential que contenga las credenciales que quiere pasar a un equipo remoto, ejecute el cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx).

Las reglas de autenticación de Windows PowerShell Web Access son reglas de la lista blanca. Cada regla es una definición de una conexión permitida entre usuarios, equipos de destino y [configuraciones de sesión](https://technet.microsoft.com/library/dd819508.aspx) de Windows PowerShell determinadas (también denominadas puntos de conexión o espacios de ejecución) en equipos de destino especificados.

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
<td><p>Un usuario solo necesita que una regla sea verdadera para obtener el acceso. Si a un usuario se le concede acceso a un equipo con acceso de lenguaje completo o acceso únicamente a los cmdlets de administración remota de Windows PowerShell, desde la consola basada en web, el usuario puede saltar a otros equipos conectados al primer equipo de destino e iniciar sesión en ellos. El modo más seguro de configurar Windows PowerShell Web Access consiste en permitir a los usuarios acceder únicamente a configuraciones de sesión restringidas (también denominadas puntos de conexión o espacios de ejecución) que les permitan llevar a cabo tareas específicas que, por lo general, deben realizar de forma remota.</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Nombre</p></th>
<th><p>Descripción</p></th>
<th><p>Parámetros</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="https://technet.microsoft.com/library/jj592890.aspx">Add-PswaAuthorizationRule</a></p></td>
<td><p>Agrega una nueva regla de autorización al conjunto de reglas de autorización de Windows PowerShell Web Access.</p></td>
<td><ul>
<li><p>ComputerGroupName</p></li>
<li><p>ComputerName</p></li>
<li><p>ConfigurationName</p></li>
<li><p>RuleName</p></li>
<li><p>UserGroupName</p></li>
<li><p>UserName</p></li>
<li><p>Credencial (Windows Server 2012 R2 y versiones posteriores)</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><a href="https://technet.microsoft.com/library/jj592893.aspx">Remove-PswaAuthorizationRule</a></p></td>
<td><p>Quita una regla de autorización especificada de Windows PowerShell Web Access.</p></td>
<td><ul>
<li><p>Id</p></li>
<li><p>RuleName</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><a href="https://technet.microsoft.com/library/jj592891.aspx">Get-PswaAuthorizationRule</a></p></td>
<td><p>Devuelve un conjunto de reglas de autorización de Windows PowerShell Web Access. Cuando se usa sin parámetros, el cmdlet devuelve todas las reglas.</p></td>
<td><ul>
<li><p>Id</p></li>
<li><p>RuleName</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><a href="https://technet.microsoft.com/library/jj592892.aspx">Test-PswaAuthorizationRule</a></p></td>
<td><p>Evalúa las reglas de autorización para determinar si una solicitud de acceso de usuario, equipo o configuración de sesión específica tiene autorización. De manera predeterminada, si no se agrega ningún parámetro, el cmdlet evalúa todas las reglas de autorización. Mediante la adición de parámetros, los administradores pueden especificar que se pruebe una regla de autorización o un subconjunto de reglas.</p></td>
<td><ul>
<li><p>ComputerName</p></li>
<li><p>ConfigurationName</p></li>
<li><p>RuleName</p></li>
<li><p>UserName</p></li>
<li><p>Credencial (Windows Server 2012 R2 y versiones posteriores)</p></li>
</ul></td>
</tr>
</tbody>
</table>

Los cmdlets anteriores crean un conjunto de reglas de acceso que se usan para autorizar a un usuario en la puerta de enlace de Windows PowerShell Web Access. Las reglas difieren de las listas de control de acceso (ACL) en el equipo de destino y proporcionan un nivel de seguridad adicional para el acceso web. En la siguiente sección, se proporcionan más detalles sobre la seguridad.

Si los usuarios no pueden pasar alguno de los niveles de seguridad anteriores, recibirán un mensaje de “acceso denegado” genérico en la ventana del explorador. Si bien los detalles de seguridad se registran en el servidor de puerta de enlace, los usuarios finales no verán ninguna información acerca de cuántos niveles de seguridad pasaron ni en qué nivel se produjo el error de autenticación o de inicio de sesión.

Para más información sobre la configuración de reglas de autorización, consulte [Configuración de reglas de autorización](#BKMK_configrules) en este tema.

<a href="" id="BKMK_sec"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Seguridad</span></a>

------------------------------------------------------------------------

El modelo de seguridad de Windows PowerShell Web Access tiene cuatro niveles entre un usuario final de la consola basada en web y un equipo de destino. Los administradores de Windows PowerShell Web Access pueden agregar niveles de seguridad mediante configuración adicional en la consola del Administrador de IIS. Para más información sobre la protección de sitios web en la consola del Administrador de IIS, consulte [Configurar la seguridad de los servidores web (IIS 7)](https://technet.microsoft.com/library/cc731278(v=ws.10).aspx). Para más información sobre los procedimientos recomendados de IIS y la prevención de ataques por denegación de servicio, consulte [Best Practices for Preventing DoS/Denial of Service Attacks](https://technet.microsoft.com/library/cc750213.aspx) (Procedimientos recomendados para impedir ataques por denegación de servicio (DoS)). Los administradores también pueden comprar e instalar software de autenticación de venta minorista adicional.

En la siguiente tabla, se describen los cuatro niveles de seguridad entre los usuarios finales y los equipos de destino.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Orden</p></th>
<th><p>Nivel</p></th>
<th><p>Descripción</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>1</p></td>
<td><p>Características de seguridad de Servidor web (IIS), como la autenticación de certificados de cliente</p></td>
<td><p>Los usuarios de Windows PowerShell Web Access siempre deben proporcionar un nombre de usuario y una contraseña para autenticar sus cuentas en la puerta de enlace. No obstante, los administradores de Windows PowerShell Web Access también pueden activar y desactivar la autenticación de certificados de cliente opcional (consulte el paso 10 de la sección acerca de cómo usar el Administrador de IIS para configurar la puerta de enlace en un sitio web existente en <a href="https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx">Instalación y uso de Windows PowerShell Web Access</a>. La característica de certificado de cliente opcional, que forma parte de la configuración de Servidor web (IIS), requiere que los usuarios finales tengan un certificado de cliente válido, además de sus nombres de usuario y contraseñas. Cuando el nivel de certificado de cliente está habilitado, la página de inicio de sesión de Windows PowerShell Web Access solicita a los usuarios que proporcionen certificados válidos antes de evaluar sus credenciales de inicio de sesión. La autenticación de certificados de cliente comprueba automáticamente si existe un certificado de cliente.</p>
<p>Si no se encuentra un certificado válido, Windows PowerShell Web Access lo notifica a los usuarios para que lo proporcionen. En el caso de que sí se encuentre uno, Windows PowerShell Web Access abre la página de inicio de sesión para que los usuarios proporcionen sus nombres de usuario y contraseñas.</p>
<p>Este es un ejemplo de una configuración de seguridad adicional proporcionada por Servidor web (IIS). Para más información sobre otras características de seguridad de IIS, consulte <a href="https://technet.microsoft.com/library/cc731278(ws.10).aspx">Configurar la seguridad de los servidores web (IIS 7).</a>.</p></td>
</tr>
<tr class="even">
<td><p>2</p></td>
<td><p>Autenticación de puerta de enlace basada en formularios de Windows PowerShell Web Access</p></td>
<td><p>La página de inicio de sesión de Windows PowerShell Web Access requiere un conjunto de credenciales (nombre de usuario y contraseña) y ofrece a los usuarios la opción de proporcionar distintas credenciales para el equipo de destino. Si el usuario no proporciona credenciales alternativas, el nombre de usuario y la contraseña principales usados para la conexión con la puerta de enlace también se usarán para la conexión con el equipo de destino.</p>
<p>Las credenciales requeridas se autentican en la puerta de enlace de Windows PowerShell Web Access. Estas credenciales deben ser cuentas de usuario válidas en el servidor de puerta de enlace de Windows PowerShell Web Access local o en Active Directory®.</p>
<p>Una vez autenticado un usuario en la puerta de enlace, Windows PowerShell Web Access comprueba las reglas de autorización para averiguar si el usuario tiene acceso al equipo de destino solicitado. Una vez completada la autorización correctamente, las credenciales del usuario se pasan al equipo de destino.</p></td>
</tr>
<tr class="odd">
<td><p>3</p></td>
<td><p>Reglas de autorización de Windows PowerShell Web Access</p></td>
<td><p>Una vez autenticado un usuario en la puerta de enlace, Windows PowerShell Web Access comprueba las reglas de autorización para averiguar si el usuario tiene acceso al equipo de destino solicitado. Una vez completada la autorización correctamente, las credenciales del usuario se pasan al equipo de destino.</p>
<p>Estas reglas solo se evalúan una vez que la puerta de enlace ha autenticado al usuario y antes de que el usuario pueda autenticarse en el equipo de destino.</p></td>
</tr>
<tr class="even">
<td><p>4</p></td>
<td><p>Reglas de autorización y autenticación de destino</p></td>
<td><p>El nivel de seguridad final de Windows PowerShell Web Access es la configuración de seguridad propia del equipo de destino. Los usuarios deben tener configurados los derechos de acceso correspondientes en el equipo de destino, y en la reglas de autorización de Windows PowerShell Web Access, para ejecutar una consola de Windows PowerShell basada en web que afecte a un equipo de destino mediante Windows PowerShell Web Access.</p>
<p>Este nivel ofrece los mismos mecanismos de seguridad que evaluarían los intentos de conexión si los usuarios intentaran crear una sesión remota de Windows PowerShell a un equipo de destino desde dentro de Windows PowerShell mediante la ejecución de los cmdlets <strong>Enter-PSSession</strong> o <strong>New-PSSession</strong>.</p>
<p>De manera predeterminada, Windows PowerShell Web Access usa el nombre de usuario y la contraseña principales para la autenticación, tanto en la puerta de enlace como en el equipo de destino. La página de inicio de sesión basada en web, en una sección titulada <strong>Configuración de conexión opcional</strong>, ofrece a los usuarios la opción de proporcionar distintas credenciales para el equipo de destino, en caso de que sean necesarias. Si el usuario no proporciona credenciales alternativas, el nombre de usuario y la contraseña principales usados para la conexión con la puerta de enlace también se usarán para la conexión con el equipo de destino.</p>
<p>Las reglas de autorización se pueden usar para conceder a los usuarios acceso a una configuración de sesión determinada. Puede crear configuraciones de sesión o espacios de ejecución restringidos para Windows PowerShell Web Access y permitir a usuarios específicos conectarse únicamente a configuraciones de sesión determinadas cuando inician sesión en Windows PowerShell Web Access. Puede usar listas de control de acceso (ACL) para determinar qué usuarios tendrán acceso a extremos específicos y para restringir aún más el acceso al extremo para un conjunto específico de usuarios mediante las reglas de autorización descritas en esta sección. Para más información sobre los espacios de ejecución restringidos, consulte <a href="https://msdn.microsoft.com/library/windows/desktop/ee706589.aspx">Constrained Runspaces</a> (Espacios de ejecución restringidos) en MSDN.</p></td>
</tr>
</tbody>
</table>

<a href="" id="BKMK_configrules"></a>
###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Configuración de reglas de autorización</span></a>

------------------------------------------------------------------------

Es probable que los administradores deseen usar para los usuarios de Windows PowerShell Web Access la misma regla de autorización que ya está definida en su entorno para la administración remota de Windows PowerShell. El primer procedimiento de esta sección describe cómo agregar una regla de autorización segura que conceda acceso a un usuario que inicia sesión para administrar un equipo dentro de una sola configuración de sesión. El segundo procedimiento describe cómo quitar una regla de autorización que ya no se necesita.

Si planea usar configuraciones de sesión personalizadas para permitir a usuarios específicos trabajar solo dentro de espacios de ejecución restringidos en Windows PowerShell Web Access, cree sus propias configuraciones de sesión personalizadas antes de agregar reglas de autorización que hagan referencia a ellas. No se pueden usar los cmdlets de Windows PowerShell Web Access para crear configuraciones de sesión personalizadas. Para más información sobre la creación de configuraciones de sesión personalizadas, consulte [about\_Session\_Configuration\_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx) (Acerca de los archivos de configuración de sesión) en MSDN.

Los cmdlets de Windows PowerShell Web Access admiten un carácter comodín: el asterisco (\*). No se admiten los caracteres comodín dentro de las cadenas. Use un solo asterisco por propiedad (usuarios, equipos o configuraciones de sesión).

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
<td><p>Para conocer otros modos de usar las reglas de autorización para conceder acceso a los usuarios y ayudar a proteger el entorno de Windows PowerShell Web Access, consulte <a href="#BKMK_others">Otros ejemplos de escenarios de reglas de autorización</a> en este tema.</p></td>
</tr>
</tbody>
</table>

#### Para agregar una regla de autorización restrictiva

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón derecho en **Windows PowerShell** en la barra de tareas y luego haga clic en **Ejecutar como administrador**..

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en **Windows PowerShell** y luego haga clic en **Ejecutar como administrador**..

2.  <span class="label">Paso opcional para restringir el acceso del usuario mediante configuraciones de sesión:</span> Compruebe que las configuraciones de sesión que quiere usar en las reglas ya existen. Si aún no se han creado, use las instrucciones para crear configuraciones de sesión proporcionadas en [about\_Session\_Configuration\_Files](https://msdn.microsoft.com/library/windows/desktop/hh847838.aspx) (Acerca de los archivos de configuración de sesión) en MSDN.

3.  Escriba lo siguiente y, a continuación, presione **Entrar**..

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_1079478f-cd51-4d35-8022-4b532a9d57a4'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –UserName <domain\user | computer\user> -ComputerName <computer_name> -ConfigurationName <session_configuration_name>

    Esta regla de autorización concede a un usuario específico acceso a un equipo de la red al que tiene acceso normalmente, con acceso a una configuración de sesión determinada en el ámbito de las necesidades de cmdlet y scripting típicas del usuario. En el siguiente ejemplo, se concede acceso a un usuario llamado <span class="code">JSmith</span> en el dominio <span class="code">Contoso</span> para administrar el equipo <span class="code">Contoso\_214</span> y usar una configuración de sesión llamada <span class="code">NewAdminsOnly.</span>.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_4e760377-e401-4ef4-988f-7a0aec1b2a90'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –UserName Contoso\JSmith -ComputerName Contoso_214 -ConfigurationName NewAdminsOnly

4.  Compruebe que se ha creado la regla ejecutando el cmdlet **Get-PswaAuthorizationRule** o **Test-PswaAuthorizationRule -UserName &lt;domain\\user | computer\\user&gt; -ComputerName** &lt;computer\_name&gt;. Por ejemplo, **Test-PswaAuthorizationRule –UserName Contoso\\JSmith –ComputerName Contoso\_214**.

#### Para quitar una regla de autorización

1.  Si aún no tiene abierta una sesión de Windows PowerShell, consulte el paso 1 de [Agregar una regla de autorización restrictiva](#BKMK_arar) en esta sección.

2.  Escriba lo siguiente y, a continuación, presione **Entrar**, donde *identificador de regla* representa el número de identificación único de la regla que desea quitar.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_0daef66d-0ecf-47fb-a8a0-d4dbceb8409d'); "Copy to clipboard.")

        Remove-PswaAuthorizationRule -ID <rule ID>

    Como alternativa, si no conoce el número de identificación, pero conoce el nombre descriptivo de la regla que desea quitar, puede obtener el nombre de la regla y canalizarlo al cmdlet <span class="code">Remove-PswaAuthorizationRule</span> para quitar la regla, tal como se muestra en el siguiente ejemplo: Get-PswaAuthorizationRule -RuleName &lt;*nombre de regla*&gt; | Remove-PswaAuthorizationRule.

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
    <td><p>No se le pedirá confirmación para eliminar la regla de autorización especificada; esta se eliminará al presionar <strong>Entrar</strong>. Asegúrate de que realmente quieres quitar la regla antes de ejecutar el cmdlet <strong>Remove-PswaAuthorizationRule</strong>.</p></td>
    </tr>
    </tbody>
    </table>

<a href="" id="BKMK_others"></a>
####

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Ejemplos de otros escenarios de reglas de autorización</span></a>

------------------------------------------------------------------------

Cada sesión de Windows PowerShell usa una configuración de sesión. Si esta no se especifica para una sesión, Windows PowerShell usa la configuración de sesión de Windows PowerShell integrada predeterminada, que se llama Microsoft.PowerShell. La configuración de sesión predeterminada incluye todos los cmdlets que se encuentran disponibles en un equipo. Los administradores pueden restringir el acceso a todos los equipos mediante la definición de una configuración de sesión con un espacio de ejecución restringido (una variedad limitada de cmdlets y tareas que los usuarios finales pueden realizar). Un usuario al que se le concede acceso a un equipo con acceso de lenguaje completo o acceso únicamente a los cmdlets de administración remota de Windows PowerShell puede conectarse a otros equipos conectados al primer equipo. La definición de un espacio de ejecución restringido puede impedir que los usuarios obtengan acceso a otros equipos desde su espacio de ejecución de Windows PowerShell permitido y mejora la seguridad del entorno de Windows PowerShell Web Access. La configuración de sesión se puede distribuir (mediante la directiva de grupo) a todos los equipos que los administradores desean que sean accesibles mediante Windows PowerShell Web Access. Para más información sobre las configuraciones de sesión, consulte [about_Session_Configurations](https://technet.microsoft.com/library/dd819508.aspx) (Acerca de las configuraciones de sesión). A continuación, se proporcionan algunos ejemplos de este escenario.

-   Un administrador crea un punto de conexión, llamado **PswaEndpoint**, con un espacio de ejecución restringido. A continuación, crea una regla, **\*,\*,PswaEndpoint** y distribuye el punto de conexión a otros equipos. La regla permite a todos los usuarios obtener acceso a todos los equipos con el punto de conexión **PswaEndpoint**. Si esta es la única regla de autorización definida en el conjunto de reglas, no se podrá obtener acceso a los equipos que no tengan ese extremo.

-   Un administrador ha creado un punto de conexión con un espacio de ejecución restringido, llamado **PswaEndpoint**, y desea restringir el acceso a usuarios específicos. Entonces, crea un grupo de usuarios llamado **Level1Support** y define la siguiente regla: **Level1Support,\*,PswaEndpoint**. La regla concede a cualquier usuario del grupo **Level1Support** acceso a todos los equipos con la configuración **PswaEndpoint**. De modo semejante, se puede restringir el acceso a un conjunto específico de equipos.

-   Algunos administradores proporcionan a determinados usuarios más acceso que a otros. Por ejemplo, un administrador crea dos grupos de usuarios, **Admins** y **BasicSupport**. También crea un punto de conexión con un espacio de ejecución restringido llamado **PswaEndpoint** y define las dos reglas siguientes: **Admins,\*,\*** y **BasicSupport,\*,PswaEndpoint**. La primera regla proporciona a todos los usuarios del grupo **Admin** acceso a todos los equipos y la segunda regla proporciona a todos los usuarios del grupo **BasicSupport** acceso únicamente a los equipos con **PswaEndpoint**..

-   Un administrador ha configurado un entorno de prueba privado y desea conceder a todos los usuarios de red autorizados acceso a todos los equipos de la red a los que tienen acceso normalmente, con acceso a todas las configuraciones de sesión a las que tienen acceso normalmente. Como se trata de un entorno de prueba privado, el administrador crea una regla de autorización que no es segura. El administrador ejecuta el cmdlet <span class="code">Add-PswaAuthorizationRule \* \* \*</span>, que usa el carácter comodín **\*** para representar a todos los usuarios, todos los equipos y todas las configuraciones. Esta regla es equivalente a la siguiente: <span class="code">Add-PswaAuthorizationRule –UserName \* -ComputerName \* -ConfigurationName \*</span>.

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
    <td><p>Esta regla no se recomienda en un entorno seguro, ya que omite el nivel de seguridad de reglas de autorización proporcionado por Windows PowerShell Web Access.</p></td>
    </tr>
    </tbody>
    </table>

-   Un administrador debe permitir a los usuarios conectarse a los equipos de destino en un entorno que incluye tanto grupos de trabajo como dominios, en el que los equipos de los grupos de trabajo ocasionalmente se usan para conectar con equipos de destino de los dominios, y los equipos de los dominios ocasionalmente se usan para conectar con equipos de destino de los grupos de trabajo. El administrador tiene un servidor de puerta de enlace, llamado *PswaServer*, en un grupo de trabajo, y el equipo de destino *srv1.contoso.com* se encuentra en un dominio. El usuario *Chris* es un usuario local autorizado tanto en el servidor de puerta de enlace del grupo de trabajo como en el equipo de destino. Su nombre de usuario en el servidor del grupo de trabajo es *chrisLocal* y su nombre de usuario en el equipo de destino es *contoso\chris*. Para autorizar el acceso de Chris a srv1.contoso.com, el administrador agrega la regla siguiente.

    [Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_8d183d3d-1c19-44b8-9297-530b0efc7c79'); "Copy to clipboard.")

        Add-PswaAuthorizationRule –userName PswaServer\chrisLocal –computerName srv1.contoso.com –configurationName Microsoft.PowerShell

    En el ejemplo de regla anterior, se autentica a Chris en el servidor de puerta de enlace y, a continuación, se autoriza su acceso a *srv1*. En la página de inicio de sesión, Chris debe proporcionar un segundo conjunto de credenciales en el área **Configuración de conexión opcional** (*contoso\\chris*). El servidor de puerta de enlace usa el conjunto de credenciales adicional para autenticarlo en el equipo de destino, *srv1.contoso.com*..

    En el escenario anterior, Windows PowerShell Web Access solo establecerá una conexión correcta con el equipo de destino una vez que se hayan completado correctamente los siguientes procesos (y estos se hayan permitido mediante una regla de autorización como mínimo).

    1.  Autenticación en el servidor de puerta de enlace del grupo de trabajo mediante la adición de un nombre de usuario, con el formato *servidor\_nombre*\\*usuario\_nombre* a la regla de autorización.

    2.  Autenticación en el equipo cliente mediante credenciales alternativas proporcionadas en la página de inicio de sesión, en el área **Configuración de conexión opcional**

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
    <td><p>Si los equipos de destino y de la puerta de enlace se encuentran en grupos de trabajo o dominios diferentes, se debe establecer una relación de confianza entre los dos equipos del grupo de trabajo, entre los dos dominios o entre el grupo de trabajo y el dominio. Esta relación no se puede configurar mediante los cmdlets de reglas de autorización de Windows PowerShell Web Access. Las reglas de autorización no definen una relación de confianza entre equipos; solo pueden autorizar a los usuarios para que se conecten a equipos de destino y configuraciones de sesión específicos. Para más información sobre el modo de configurar una relación de confianza entre dominios diferentes, consulte <a href="https://technet.microsoft.com/library/cc794775.aspx">Creating Domain and Forest Trusts</a> (Creación de confianzas entre dominios y bosques). Para más información sobre el modo de agregar equipos del grupo de trabajo a una lista de hosts de confianza, consulte <a href="https://technet.microsoft.com/library/dd759202.aspx">Administración remota con el Administrador del servidor</a>.</p></td>
    </tr>
    </tbody>
    </table>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Uso de un solo conjunto de reglas de autorización para varios sitios</span></a>

------------------------------------------------------------------------

Las reglas de autorización se almacenan en un archivo XML. De manera predeterminada, el nombre de ruta de acceso del archivo XML es %windir%\Web\PowershellWebAccess\data\AuthorizationRules.xml.

La ruta de acceso al archivo XML de reglas de autorización se almacena en el archivo **powwa.config**,que se encuentra en %windir%\\Web\\PowershellWebAccess\\data. El administrador tiene flexibilidad para cambiar la referencia a la ruta de acceso predeterminada en **powwa.config** para satisfacer preferencias o cumplir requisitos. Al permitir al administrador cambiar la ubicación del archivo, se permite que varias puertas de enlace de Windows PowerShell Web Access usen las mismas reglas de autorización, en el caso de que se desee este tipo de configuración.

<a href="" id="BKMK_sesmgmt"></a>

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Administración de sesiones</span></a>
<a href="/en-us/library/dn282394(v=ws.11).aspx#Anchor_1" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

De forma predeterminada, Windows PowerShell Web Access limita a los usuarios a tres sesiones al mismo tiempo. Puede editar el archivo **web.config** de la aplicación web en el Administrador de IIS para permitir un número diferente de sesiones por usuario. La ruta de acceso al archivo **web.config** es $Env:Windir\Web\PowerShellWebAccess\wwwroot\Web.config.

De manera predeterminada, Servidor web (IIS) está configurado para reiniciar el grupo de aplicaciones si se edita alguna configuración. Por ejemplo, el grupo de aplicaciones se reinicia si se realizan cambios en el archivo **web.config**. Como Windows PowerShell Web Access utiliza estados de sesión en memoria, los usuarios que han iniciado sesiones en Windows PowerShell Web Access las pierden cuando el grupo de aplicaciones se reinicia.

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Establecer parámetros predeterminados en la página de inicio de sesión</span></a>

------------------------------------------------------------------------

Si la puerta de enlace de Windows PowerShell Web Access se ejecuta en Windows Server 2012 R2, puede definir valores predeterminados para la configuración que se muestra en la página de inicio de sesión de Windows PowerShell Web Access. Puede configurar valores en el archivo **web.config** que se describe en el párrafo anterior. Los valores predeterminados para la configuración de la página de inicio de sesión se encuentran en la sección **appSettings** del archivo web.config; a continuación se muestra un ejemplo de la sección **appSettings**. Los valores válidos para muchas de estas opciones son los mismos que para los parámetros correspondientes del cmdlet [New-PSSession](https://technet.microsoft.com/library/hh849717.aspx) de Windows PowerShell. Por ejemplo, la clave <span class="code">defaultApplicationName</span>, como se muestra en el siguiente bloque de código, es el valor de la variable de preferencia **$PSSessionApplicationName** del equipo de destino.

[Copy](javascript:if%20(window.epx.codeSnippet)window.epx.codeSnippet.copyCode('CodeSnippetContainerCode_6ccfd0a1-485a-4ac5-9636-89ebab501bef'); "Copy to clipboard.")

    <appSettings>
            <add key="maxSessionsAllowedPerUser" value="3"/>
            <add key="defaultPortNumber" value="5985"/>
            <add key="defaultSSLPortNumber" value="5986"/>
            <add key="defaultApplicationName" value="WSMAN"/>
            <add key="defaultUseSslSelection" value="0"/>
            <add key="defaultAuthenticationType" value="0"/>
            <add key="defaultAllowRedirection" value="0"/>
            <add key="defaultConfigurationName" value="Microsoft.PowerShell"/>
    </appSettings>

###

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Tiempos de espera y desconexiones no planeadas</span></a>

------------------------------------------------------------------------

Tiempo de espera de las sesiones de Windows PowerShell Web Access. En Windows PowerShell Web Access cuando se ejecuta en Windows Server 2012 se muestra un mensaje de tiempo de espera agotado cuando los usuarios conectados no han realizado ninguna actividad en la sesión en 15 minutos. Si el usuario no responde dentro de los cinco minutos posteriores al envío del mensaje, la sesión del usuario se cerrará. Puede cambiar los períodos de tiempo de espera de las sesiones en la configuración del sitio web en el Administrador de IIS.

En Windows PowerShell Web Access cuando se ejecuta en Windows Server 2012 R2, se agota el tiempo de espera de las sesiones después de 20 minutos de inactividad. Si los usuarios están desconectados de sesiones en la consola basada en web debido a errores de red u otros apagados o errores no planificados, y no porque se hayan cerrado las sesiones ellas mismas, las sesiones de Windows PowerShell Web Access continúan ejecutándose, conectadas a equipos de destino, hasta que vence el período de tiempo de espera del cliente. La sesión se desconecta después de los 20 minutos predeterminados o después del período de tiempo de espera especificado por el administrador de la puerta de enlace, el que sea más corto.

Si el servidor de puerta de enlace está ejecutando Windows Server 2012 R2, Windows PowerShell Web Access permite a los usuarios volver a conectarse a las sesiones guardadas en otro momento, pero cuando errores de red, apagados no planificados u otros errores desconectan las sesiones, los usuarios no pueden ver o volver a conectarse a las sesiones guardadas hasta que haya vencido el período de tiempo de espera especificado por el administrador de la puerta de enlace.

<a href="javascript:void(0)" class="LW_CollapsibleArea_TitleAhref" title="Collapse"><span class="cl_CollapsibleArea_expanding LW_CollapsibleArea_Img"></span><span class="LW_CollapsibleArea_Title">Consulte también</span></a>
<a href="/en-us/library/dn282394(v=ws.11).aspx#Anchor_2" class="LW_CollapsibleArea_Anchor_Img" title="Right-click to copy and share the link for this section"></a>

------------------------------------------------------------------------

[Instalación y uso de Windows PowerShell Web Access](https://technet.microsoft.com/en-us/library/hh831611(v=ws.11).aspx)
[about\_Session\_Configurations (Acerca de las configuraciones de sesión)](https://technet.microsoft.com/library/dd819508.aspx)
[Cmdlets de Windows PowerShell Web Access](https://technet.microsoft.com/library/hh918342.aspx)

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


