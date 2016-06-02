---
title:  Instalar el SDK de Windows PowerShell
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  c3636b45-61aa-4720-85f0-58312c4fc8f9
---

# Instalar el SDK de Windows PowerShell
<?xml version="1.0" encoding="utf-8"?>
<developerConceptualDocument xmlns="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://ddue.schemas.microsoft.com/authoring/2003/5 http://dduestorage.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>En el siguiente tema se describe cómo instalar PowerShell SDK en distintas versiones de Windows.</para>
  </introduction>
  <section>
    <title>Instalación de Windows PowerShell 3.0 SDK para Windows 8 y Windows Server 2012</title>
    <content>
      <para>Windows PowerShell 3.0 está instalado automáticamente en Windows 8 y Windows Server 2012. Además, puede descargar e instalar los ensamblados de referencia para Windows PowerShell 3.0 como parte de Windows 8 SDK. Estos ensamblados le permiten escribir cmdlets, proveedores y programas host para Windows PowerShell 3.0. Al instalar Windows SDK para Windows 8, los ensamblados de Windows PowerShell se instalan automáticamente en la carpeta de ensamblados de referencia, en \Archivos de programa (x86)\Reference Assemblies\Microsoft\WindowsPowerShell\3.0. Para obtener más información, consulte el <externalLink><linkText>sitio de descarga de Windows 8 SDK</linkText><linkUri>http://msdn.microsoft.com/windows/hardware/hh852363.aspx</linkUri></externalLink>. También hay disponibles ejemplos de código de Windows PowerShell en el Centro de desarrollo. Para obtener más información, consulte la página de ejemplos de código de escritorio en el <externalLink><linkText>sitio del Centro de desarrollo</linkText><linkUri>http://code.msdn.microsoft.com/windowsdesktop/</linkUri></externalLink>. </para>
      <para>Además, Windows PowerShell 3.0 también es compatible con Windows PowerShell 2.0 SDK, que incluye varios ejemplos de código. Más abajo le indicamos cómo descargar Windows PowerShell 2.0 SDK. (Tenga en cuenta que, a pesar de que los ejemplos de código de la versión 2.0 son compatibles con Windows 8 y Windows PowerShell 3.0, no puede instalar Windows PowerShell 2.0 en una plataforma de Windows 8). </para>
    </content>
  </section>
  <section>
    <title>Instalación de Windows PowerShell 3.0 SDK para Windows 7 y Windows Server 2008 R2</title>
    <content>
      <para>Windows 7 y Windows Server 2008 R2 ya tienen instalado automáticamente PowerShell 2.0. Además, puede instalar PowerShell 3.0 en estos sistemas. (Para obtener más información, consulte <link xlink:href="6fbb0409-5a54-48ec-95e6-7f8b7d8c4969">Installing Windows PowerShell</link> [Instalación de Windows PowerShell]). Tal como se ha descrito anteriormente, también puede instalar Windows 8 SDK en Windows 7 y Windows Server 2008 R2.</para>
    </content>
  </section>
  <section>
    <title>Instalación de Windows PowerShell 2.0 SDK para Windows 7, Vista, XP, Server 2003 y Server 2008</title>
    <content>
      <para><token>mshshort</token> 2.0 SDK proporciona los ensamblados de referencia necesarios para escribir cmdlets, proveedores y aplicaciones host, así como código de ejemplo en C# que se puede usar como punto de partida para comenzar a escribir código. </para>
      <para>Para instalar este SDK, consulte <externalLink><linkText>Windows PowerShell 2.0 SDK</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=184611</linkUri></externalLink>.</para>
    </content>
    <sections>
      <section>
        <title>Ensamblados de referencia</title>
        <content>
          <para>Los ensamblados de referencia se instalan de forma predeterminada en la siguiente ubicación:<codeInline>C:\Archivos de programa\Reference Assemblies\Microsoft\WindowsPowerShell\V1.0</codeInline>.</para>
          <alert class="note">
            <para>El código compilado con los ensamblados de Windows PowerShell 2.0 no se puede cargar en las instalaciones de Windows PowerShell 1.0. Sin embargo, el código compilado con los ensamblados de Windows PowerShell 1.0 sí puede cargarse en las instalaciones de Windows PowerShell 2.0.</para>
          </alert>
        </content>
      </section>
      <section>
        <title>Ejemplos</title>
        <content>
          <para>Los ejemplos de código se instalan de forma predeterminada en la siguiente ubicación: <codeInline>C:\Archivos de programa\Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\</codeInline>.</para>
          <para>Las siguientes secciones proporcionan una breve descripción del efecto de cada ejemplo.</para>
        </content>
        <sections>
          <section>
            <title>Ejemplos de cmdlet</title>
            <content>
              <definitionTable>
                <definedTerm>GetProcessSample01</definedTerm>
                <definition>
                  <para>Muestra cómo escribir un cmdlet sencillo que obtiene todos los procesos en el equipo local.</para>
                </definition>
                <definedTerm>GetProcessSample02</definedTerm>
                <definition>
                  <para>Muestra cómo agregar parámetros al cmdlet. El cmdlet como mínimo un nombre de proceso y devuelve los procesos coincidentes.</para>
                </definition>
                <definedTerm>GetProcessSample03</definedTerm>
                <definition>
                  <para>Muestra cómo agregar parámetros que aceptan la entrada desde la canalización.</para>
                </definition>
                <definedTerm>GetProcessSample04</definedTerm>
                <definition>
                  <para>Muestra cómo controlar errores de no terminación.</para>
                </definition>
                <definedTerm>GetProcessSample05</definedTerm>
                <definition>
                  <para>Muestra cómo mostrar una lista de procesos especificados.</para>
                </definition>
                <definedTerm>SelectObject</definedTerm>
                <definition>
                  <para>Muestra cómo escribir un filtro para seleccionar solo determinados objetos. </para>
                </definition>
                <definedTerm>SelectString</definedTerm>
                <definition>
                  <para>Muestra cómo buscar patrones específicos en archivos.</para>
                </definition>
                <definedTerm>StopProcessSample01</definedTerm>
                <definition>
                  <para>Muestra cómo implementar un parámetro <parameterReference>PassThru</parameterReference> y cómo solicitar comentarios de los usuarios mediante llamadas a los métodos <codeEntityReference>Overload:System.Management.Automation.Cmdlet.ShouldProcess</codeEntityReference> y <codeEntityReference>Overload:System.Management.Automation.Cmdlet.ShouldContinue</codeEntityReference>. Los usuarios especifican el parámetro <parameterReference>PassThru</parameterReference> cuando deseen forzar al cmdlet para devolver un objeto.</para>
                </definition>
                <definedTerm>StopProcessSample02</definedTerm>
                <definition>
                  <para>Muestra cómo detener un proceso específico.</para>
                </definition>
                <definedTerm>StopProcessSample03</definedTerm>
                <definition>
                  <para>Muestra cómo declarar los alias de parámetros y cómo agregar compatibilidad con caracteres comodín.</para>
                </definition>
                <definedTerm>StopProcessSample04</definedTerm>
                <definition>
                  <para>Muestra cómo declarar los conjuntos de parámetros, el objeto que el cmdlet toma como entrada, y también cómo especificar el parámetro predeterminado configurado para usarse.</para>
                </definition>
              </definitionTable>
            </content>
          </section>
          <section>
            <title>Ejemplos de comunicación remota</title>
            <content>
              <definitionTable>
                <definedTerm>RemoteRunspace01</definedTerm>
                <definition>
                  <para>Muestra cómo crear un espacio de ejecución remoto que se usa para establecer una conexión remota.</para>
                </definition>
                <definedTerm>RemoteRunspacePool01</definedTerm>
                <definition>
                  <para>Muestra cómo crear un grupo de espacio de ejecución remoto y cómo ejecutar varios comandos simultáneamente mediante el uso de este grupo.</para>
                </definition>
                <definedTerm>Serialization01</definedTerm>
                <definition>
                  <para>Muestra cómo examinar una clase .NET existente y cómo asegurarse de que se conserva la información de las propiedades públicas seleccionadas de esta clase a través de la serialización o deserialización.</para>
                </definition>
                <definedTerm>Serialization02</definedTerm>
                <definition>
                  <para>Muestra cómo examinar una clase .NET existente y cómo asegurarse de que la información de la instancia de esta clase se conserva en la serialización o deserialización cuando la información no está disponible en las propiedades públicas de la clase.</para>
                </definition>
                <definedTerm>Serialization03</definedTerm>
                <definition>
                  <para>Muestra cómo examinar una clase .NET existente y cómo asegurarse de que las instancias de esta clase y de las clases derivadas se deserializan (rehidratan) en objetos de .NET dinámicos.</para>
                </definition>
              </definitionTable>
            </content>
          </section>
          <section>
            <title>Ejemplos de eventos</title>
            <content>
              <definitionTable>
                <definedTerm>Event01</definedTerm>
                <definition>
                  <para>Muestra cómo crear un cmdlet para el registro de eventos mediante la derivación de ObjectEventRegistrationBase.</para>
                </definition>
                <definedTerm>Event02</definedTerm>
                <definition>
                  <para>Muestra cómo recibir notificaciones de eventos de <token>mshshort</token> que se generan en equipos remotos. Usa el evento PSEventReceived que se expone a través de la clase <codeEntityReference>T:System.Management.Automation.Runspaces.Runspace</codeEntityReference>.</para>
                </definition>
              </definitionTable>
            </content>
          </section>
          <section>
            <title>Ejemplos de aplicación host</title>
            <content>
              <definitionTable>
                <definedTerm>Runspace01</definedTerm>
                <definition>
                  <para>Muestra cómo usar la clase <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> para ejecutar el cmdlet <externalLink><linkText>Get-Process</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=113324</linkUri></externalLink> de forma sincrónica. El cmdlet <externalLink><linkText>Get-Process</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=113324</linkUri></externalLink> devuelve objetos <codeEntityReference>T:System.Diagnostics.Process</codeEntityReference> para cada proceso que se ejecuta en el equipo local.</para>
                </definition>
                <definedTerm>Runspace02</definedTerm>
                <definition>
                  <para>Muestra cómo usar la clase <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> para ejecutar los cmdlets <externalLink><linkText>Get-Process</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=113324</linkUri></externalLink> y <externalLink><linkText>Sort-Object</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkID=113403</linkUri></externalLink> de forma sincrónica. El cmdlet <externalLink><linkText>Get-Process</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=113324</linkUri></externalLink> devuelve objetos <codeEntityReference>T:System.Diagnostics.Process</codeEntityReference> para cada proceso que se ejecuta en el equipo local y el cmdlet Sort-Object ordena los objetos según su propiedad <codeEntityReference>P:System.Diagnostics.Process.Id</codeEntityReference>. Los resultados de estos comandos se muestran mediante un control <codeEntityReference>DataGridView</codeEntityReference>.</para>
                </definition>
                <definedTerm>Runspace03</definedTerm>
                <definition>
                  <para>Muestra cómo usar la clase <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> para ejecutar un script de forma sincrónica y cómo controlar los errores de no terminación. El script recibe una lista de nombres de procesos y después los recupera. Los resultados del script, incluidos los errores de no terminación generados al ejecutarlo, se muestran en una ventana de consola.</para>
                </definition>
                <definedTerm>Runspace04</definedTerm>
                <definition>
                  <para>Muestra cómo usar la clase <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> para ejecutar comandos y cómo capturar los errores de terminación que se producen al ejecutar dichos comandos. Se ejecutan dos comandos; al último, se le pasa un argumento de parámetro que no es válido. Como resultado, no se devuelve ningún objeto y se produce un error de terminación.</para>
                </definition>
                <definedTerm>Runspace05</definedTerm>
                <definition>
                  <para>Muestra cómo agregar un complemento a un objeto <codeEntityReference>T:System.Management.Automation.Runspaces.InitialSessionState</codeEntityReference> para que el cmdlet del complemento esté disponible al abrir el espacio de ejecución. El complemento proporciona un cmdlet Get-Proc (definido por el <legacyLink xlink:href="7b48bf80-cbf0-4cb1-8d5b-3b8d06196598">ejemplo GetProcessSample01</legacyLink>) que se ejecuta de forma sincrónica mediante un objeto <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference>.</para>
                </definition>
                <definedTerm>Runspace06</definedTerm>
                <definition>
                  <para>Muestra cómo agregar un módulo a un objeto <codeEntityReference>T:System.Management.Automation.Runspaces.InitialSessionState</codeEntityReference> para que el módulo se cargue al abrir el espacio de ejecución. El módulo proporciona un cmdlet Get-Proc (definido por el <legacyLink xlink:href="481f557d-3344-4d33-b2da-4736a0165181">ejemplo GetProcessSample02</legacyLink>) que se ejecuta de forma sincrónica mediante un objeto <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference>.</para>
                </definition>
                <definedTerm>Runspace07</definedTerm>
                <definition>
                  <para>Muestra cómo crear un espacio de ejecución y lo usa para ejecutar dos cmdlets de forma sincrónica mediante un objeto <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference>.</para>
                </definition>
                <definedTerm>Runspace08</definedTerm>
                <definition>
                  <para>Muestra cómo agregar comandos y argumentos a la canalización de un objeto <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> y cómo ejecutar los comandos de forma sincrónica.</para>
                </definition>
                <definedTerm>Runspace09</definedTerm>
                <definition>
                  <para>Muestra cómo agregar un script a la canalización de un objeto <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> y cómo ejecutar la secuencia de comandos de forma asincrónica. Los eventos se usan para controlar la salida del script.</para>
                </definition>
                <definedTerm>Runspace10</definedTerm>
                <definition>
                  <para>Muestra cómo crear un estado de sesión inicial predeterminado, cómo agregar un cmdlet a <codeEntityReference>T:System.Management.Automation.Runspaces.InitialSessionState</codeEntityReference>, cómo crear un espacio de ejecución que use el estado de sesión inicial y cómo ejecutar el comando con un objeto <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference>.</para>
                </definition>
                <definedTerm>Runspace11</definedTerm>
                <definition>
                  <para>Muestra cómo usar la clase <codeEntityReference>T:System.Management.Automation.ProxyCommand</codeEntityReference> para crear un comando de proxy que llama a un cmdlet existente, pero restringe el conjunto de parámetros disponibles. El comando de proxy se agrega entonces a un estado de sesión inicial que se usa para crear un espacio de ejecución restringido. Esto significa que el usuario puede acceder a la funcionalidad del cmdlet solo mediante el comando de proxy.</para>
                </definition>
                <definedTerm>PowerShell01</definedTerm>
                <definition>
                  <para>Muestra cómo crear un espacio de ejecución restringido mediante un objeto <codeEntityReference>T:System.Management.Automation.Runspaces.InitialSessionState</codeEntityReference>.</para>
                </definition>
                <definedTerm>PowerShell02</definedTerm>
                <definition>
                  <para>Muestra cómo usar un grupo de espacio de ejecución para ejecutar varios comandos simultáneamente.</para>
                </definition>
              </definitionTable>
            </content>
          </section>
          <section>
            <title>Ejemplos de host</title>
            <content>
              <definitionTable>
                <definedTerm>Host01</definedTerm>
                <definition>
                  <para>Muestra cómo implementar una aplicación host que usa un host personalizado. En este ejemplo se crea un espacio de ejecución que usa el host personalizado y, después, se usa la API <codeEntityReference>T:System.Management.Automation.PowerShell</codeEntityReference> para ejecutar un script que llame al código "exit". La aplicación host examina después el resultado de la secuencia de comandos e imprime los resultados.</para>
                </definition>
                <definedTerm>Host02</definedTerm>
                <definition>
                  <para>Muestra cómo escribir una aplicación host que usa el tiempo de ejecución <token>mshshort</token> junto con una implementación de host personalizado. La aplicación host establece la referencia cultural del host como alemán, ejecuta el cmdlet <externalLink><linkText>Get-Process</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=113324</linkUri></externalLink>, muestra los resultados tal como los vería con pwrsh.exe e imprime los datos y la hora actuales en alemán.</para>
                </definition>
                <definedTerm>Host03</definedTerm>
                <definition>
                  <para>Muestra cómo crear una aplicación host basada en consola interactiva que lee los comandos de la línea de comandos, los ejecuta y muestra sus resultados en la consola.</para>
                </definition>
                <definedTerm>Host04</definedTerm>
                <definition>
                  <para>Muestra cómo crear una aplicación host basada en consola interactiva que lee los comandos de la línea de comandos, los ejecuta y muestra sus resultados en la consola. Esta aplicación host también permite mostrar avisos para que el usuario pueda especificar varias opciones.</para>
                </definition>
                <definedTerm>Host05</definedTerm>
                <definition>
                  <para>Muestra cómo crear una aplicación host basada en consola interactiva que lee los comandos de la línea de comandos, los ejecuta y muestra sus resultados en la consola. Esta aplicación host también admite llamadas a equipos remotos mediante los cmdlets <externalLink><linkText>Enter-PsSession</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=135210</linkUri></externalLink> y <externalLink><linkText>Exit-PsSession</linkText><linkUri>http://go.microsoft.com/fwlink/?LinkId=135212</linkUri></externalLink>.</para>
                </definition>
                <definedTerm>Host06</definedTerm>
                <definition>
                  <para>Muestra cómo crear una aplicación host basada en consola interactiva que lee los comandos de la línea de comandos, los ejecuta y muestra sus resultados en la consola. Además, este ejemplo utiliza las API de Tokenizer para especificar el color del texto que el usuario ha escrito.</para>
                </definition>
              </definitionTable>
            </content>
          </section>
          <section>
            <title>Ejemplos de proveedor</title>
            <content>
              <definitionTable>
                <definedTerm>AccessDBProviderSample01</definedTerm>
                <definition>
                  <para>Muestra cómo declarar una clase de proveedor que se deriva directamente de la clase <codeEntityReference>T:System.Management.Automation.Provider.CmdletProvider</codeEntityReference>. Se incluye aquí solo por cuestiones de integridad.</para>
                </definition>
                <definedTerm>AccessDBProviderSample02</definedTerm>
                <definition>
                  <para>Muestra cómo sobrescribir los métodos <codeEntityReference>M:System.Management.Automation.Provider.DriveCmdletProvider.NewDrive(System.Management.Automation.PSDriveInfo)</codeEntityReference> y <codeEntityReference>M:System.Management.Automation.Provider.DriveCmdletProvider.RemoveDrive(System.Management.Automation.PSDriveInfo)</codeEntityReference> para admitir llamadas a los cmdlets New-PSDrive y Remove-PSDrive. La clase de proveedor de este ejemplo se deriva de la clase <codeEntityReference>T:System.Management.Automation.Provider.DriveCmdletProvider</codeEntityReference>.</para>
                </definition>
                <definedTerm>AccessDBProviderSample03</definedTerm>
                <definition>
                  <para>Muestra cómo sobrescribir los métodos <codeEntityReference>M:System.Management.Automation.Provider.ItemCmdletProvider.GetItem(System.String)</codeEntityReference> y <codeEntityReference>M:System.Management.Automation.Provider.ItemCmdletProvider.SetItem(System.String,System.Object)</codeEntityReference> para admitir llamadas a los cmdlets Get-Item y Set-Item. La clase de proveedor en este ejemplo se deriva de la clase <codeEntityReference>T:System.Management.Automation.Provider.ItemCmdletProvider</codeEntityReference>.</para>
                </definition>
                <definedTerm>AccessDBProviderSample04</definedTerm>
                <definition>
                  <para>Muestra cómo sobrescribir los métodos de contenedor para admitir llamadas a los cmdlets Copy-Item, Get-ChildItem, New-Item y Remove-Item. Estos métodos deberían implementarse cuando el almacén de datos contengan elementos que son contenedores. Un contenedor es un grupo de elementos secundarios con un elemento primario común. La clase de proveedor de este ejemplo se deriva de la clase <codeEntityReference>T:System.Management.Automation.Provider.ItemCmdletProvider</codeEntityReference>.</para>
                </definition>
                <definedTerm>AccessDBProviderSample05</definedTerm>
                <definition>
                  <para>Muestra cómo sobrescribir los métodos de contenedor para admitir llamadas a los cmdlets Move-Item y Join-Path. Estos métodos deberían implementarse cuando el usuario necesite mover elementos dentro de un contenedor y si el almacén de datos tiene contenedores anidados. La clase de proveedor de este ejemplo se deriva de la clase <codeEntityReference>T:System.Management.Automation.Provider.NavigationCmdletProvider</codeEntityReference>.</para>
                </definition>
                <definedTerm>AccessDBProviderSample06</definedTerm>
                <definition>
                  <para>Muestra cómo sobrescribir métodos de contenido para admitir llamadas a los cmdlets Clear-Content, Get-Content y Set-Content. Estos métodos deberían implementarse cuando el usuario necesite administrar el contenido de los elementos en el almacén de datos. La clase de proveedor de este ejemplo se deriva de la clase <codeEntityReference>T:System.Management.Automation.Provider.NavigationCmdletProvider</codeEntityReference>; también se implementa la interfaz <codeEntityReference>T:System.Management.Automation.Provider.IContentCmdletProvider</codeEntityReference>.</para>
                </definition>
              </definitionTable>
            </content>
          </section>
        </sections>
      </section>
    </sections>
  </section>
  <relatedTopics />
</developerConceptualDocument>



<!--HONumber=May16_HO4-->


