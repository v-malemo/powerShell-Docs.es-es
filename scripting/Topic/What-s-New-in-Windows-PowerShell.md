---
title: Novedades de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1476722e-947e-425d-a86c-50037488dc6e
---
# Novedades de Windows PowerShell
[!INCLUDE[wps_1](../Token/wps_1_md.md)] 5.0 incluye nuevas características importantes que amplían y mejoran su uso, y permiten controlar y administrar entornos basados en Windows de forma más sencilla y completa.

[!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 es compatible con versiones anteriores. Los cmdlets, proveedores, módulos, complementos, scripts, funciones y perfiles diseñados para [!INCLUDE[psversion4](../Token/psversion4_md.md)], [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion2](../Token/psversion2_md.md)] suelen funcionar en [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 sin tener que cambiar nada.

[!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 está instalado de forma predeterminada en [!INCLUDE[winthreshold_server_1](../Token/winthreshold_server_1_md.md)] y en [!INCLUDE[winthreshold_client_1](../Token/winthreshold_client_1_md.md)]. Para instalar [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 en [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], [!INCLUDE[winblue_client_ent_2](../Token/winblue_client_ent_2_md.md)] o [!INCLUDE[winblue_client_pro_2](../Token/winblue_client_pro_2_md.md)], descargue e instale [Windows Management Framework 5.0 Preview](http://go.microsoft.com/fwlink/?LinkID=395058). Procure leer los detalles de la descarga y cumplir todos los requisitos del sistema antes de instalar Windows Management Framework 5.0 Preview.

## En este tema

-   [Actualizaciones de DSC de Windows PowerShell 4.0 en KB 3000850](#BKMK_3000850)

-   [Nuevas características de Windows PowerShell 5.0](#BKMK_new50)

-   [Nuevas características de Windows PowerShell 4.0](#BKMK_wps4)

-   [Nuevas características de Windows PowerShell 3.0](#BKMK_wps3)

## <a name="BKMK_3000850"></a>Actualizaciones de Windows PowerShell 4.0 en el paquete acumulativo de actualizaciones de noviembre de 2014 (KB 3000850)
Muchas actualizaciones y mejoras en la configuración de estado deseado (DSC) de [!INCLUDE[wps_2](../Token/wps_2_md.md)] en [!INCLUDE[wps_2](../Token/wps_2_md.md)] 4.0 están disponibles en el [paquete acumulativo de actualizaciones de noviembre de 2014 de Windows RT 8.1, Windows 8.1 y Windows Server 2012 R2](https://support.microsoft.com/kb/3000850/) (KB 3000850). Para determinar si KB 3000850 está instalado en el sistema, ejecute `Get-Hotfix -Id KB3000850` en [!INCLUDE[wps_2](../Token/wps_2_md.md)].

-   Actualizaciones para los cmdlets existentes en el módulo [PSDesiredStateConfiguration](https://technet.microsoft.com/library/dn391651(v=wps.640).aspx)

    -   [Get-DscResource](http://technet.microsoft.com/library/dn521625.aspx) es más rápido (especialmente en el ISE).

    -   [Start-DscConfiguration](http://technet.microsoft.com/library/dn521623.aspx) tiene un nuevo parámetro, –UseExisting, que vuelve a aplicar la última configuración aplicada.

    -   [Start-DscConfiguration](http://technet.microsoft.com/library/dn521623.aspx) -Force se ha corregido.

    -   [Get-DscLocalConfigurationManager](http://technet.microsoft.com/library/dn407378.aspx) muestra más información útil sobre el estado del motor.

    -   [Test-DscConfiguration](http://technet.microsoft.com/library/dn407382.aspx) devuelve ahora el nombre del equipo junto con True o False.

    -   [New-DscChecksum](http://technet.microsoft.com/library/dn521622.aspx) admite ahora rutas UNC.

-   Nuevos cmdlets en el módulo [PSDesiredStateConfiguration](http://technet.microsoft.com/library/dn391651(v=wps.640).aspx)

    -   [Update-DscConfiguration](http://technet.microsoft.com/library/mt143541(v=wps.630).aspx): realiza una comprobación del servidor de extracción a petición.

    -   [Stop-DscConfiguration](http://technet.microsoft.com/library/mt143542(v=wps.630).aspx): detiene una configuración que ya se está ejecutando.

    -   [Remove-DscConfigurationDocument](http://technet.microsoft.com/library/mt143544(v=wps.630).aspx): permite quitar los documentos de configuración en diversas etapas (pendientes, anteriores o actuales).

-   Mejoras en el lenguaje

    -   DependsOn admite ahora recursos compuestos.

    -   DependsOn admite ahora números en los nombres de instancia de recurso.

    -   Las expresiones de nodo que se evalúan como vacías ya no producen errores.

    -   Se ha corregido un error que se producía al evaluar una expresión de nodo como vacía.

    -   Las configuraciones que llaman a configuraciones funcionan ahora en la consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)].

-   Mejoras en el modo de extracción

    -   El modo de extracción admite ahora todos los archivos ZIP.

    -   **AllowModuleOverwrite** funciona ahora correctamente.

-   Mejoras en la resistencia

    -   El nuevo **DebugMode** permite volver a cargar los módulos de recursos.

    -   Si se produce un error de configuración, el archivo pending.mof no se elimina.

    -   El Administrador de configuración local (LCM) es ahora más resistente cuando los valores de metaconfiguración dañados.

-   Mejoras de diagnóstico

    -   Se muestra una advertencia cuando el LCM establece el temporizador en una configuración diferente a la especificada.

    -   Los archivos de registro de errores contienen ahora la pila de llamadas de los recursos de [!INCLUDE[wps_2](../Token/wps_2_md.md)].

-   Mejoras de escalabilidad

    -   El recurso LocalConfigurationManager tiene una propiedad nueva, **ActionAfterReboot**.

        -   ContinueConfiguration (valor predeterminado): reanuda automáticamente una configuración cuando se reinicia un nodo de destino.

        -   StopConfiguration (valor predeterminado): no reanuda automáticamente una configuración cuando se reinicia un nodo de destino.

    -   Una ejecución de coherencia puede llevarse a cabo con mayor frecuencia que una operación de extracción o viceversa.

    -   Compatibilidad de versiones: DSC puede ahora reconocer un documento generado en un cliente más reciente (incluido con [WMF 5.0 Preview](http://go.microsoft.com/fwlink/?LinkID=395058)).

-   Mejoras en la prevención de errores

    -   La versión del módulo se aplica ahora antes de aplicar una configuración.

    -   **DebugPreference** está ahora establecido correctamente para las llamadas Get-, Set- o Test-TargetResource.

-   Mejoras en la administración de credenciales

    -   Ahora se usa un certificado, si se especifican ambas opciones **Certificate** y **PSDscAllowPlainTextPassword**.

    -   Las credenciales se descifran, incluso para Get-TargetResource.

    -   Las credenciales de metaconfiguración se cifran y se descifran.

    -   Los objetos PSCredentials se descifran cuando están en un objeto incrustado.

-   Mejoras en los recursos integrados

    -   Recurso de paquete

        -   Ya no instala el paquete incorrecto (ya sea de los orígenes web o local).

        -   Ahora admite HTTPS.

    -   Ahora se ofrece compatibilidad con HTTPS en el [recurso de paquete](http://technet.microsoft.com/library/dn282132.aspx).

    -   El [recurso de archivo](http://technet.microsoft.com/library/dn249917.aspx) admite ahora las credenciales.

## <a name="BKMK_new50"></a>Nuevas características de Windows PowerShell 5.0

-   [Nuevas características de Windows PowerShell](#BKMK_newcore)

-   [Nuevas características de configuración de estado deseado de Windows PowerShell](#BKMK_newDSC)

-   [Nuevas características de Windows PowerShell ISE](#BKMK_newISE)

-   [Nuevas características de servicios web de Windows PowerShell](#BKMK_newOData)

-   [Correcciones de errores importantes en Windows PowerShell 5.0](#BKMK_5bugfix)

### <a name="BKMK_newcore"></a>Nuevas características de Windows PowerShell

-   A partir de [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0, se admite el desarrollo mediante clases, a través de una semántica y una sintaxis formales que son similares a las de otros lenguajes de programación orientados a objetos. **Class**, **Enum** y otras palabras clave se agregaron al nuevo lenguaje de [!INCLUDE[wps_2](../Token/wps_2_md.md)] para admitir la nueva característica. Para obtener información sobre el uso de las clases, vea about_Classes.

-   [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 presenta una nueva secuencia de información estructurada que puede usar para transmitir datos estructurados entre un script y los autores de la llamada (o el entorno de hospedaje). Ahora puede usar Write-Host para emitir la salida en la secuencia de información. Las secuencias de información también funcionan con PowerShell.Streams, los trabajos, los trabajos programados y los flujos de trabajo. Las características siguientes admiten la secuencia de información.

    -   Un nuevo cmdlet Write-Information que permite especificar cómo [!INCLUDE[wps_2](../Token/wps_2_md.md)] administra los datos de la secuencia de información de un comando. Write-Host es un contenedor para Write-Information. Write-Information también es una actividad de flujo de trabajo admitida.

    -   Dos nuevos parámetros comunes, InformationVariable y InformationAction, permiten determinar cómo se muestran las secuencias de información desde un comando. Los valores válidos para InformationAction son Continuar sin mensajes, Detener, Continuar, Consultar, Omitir o Suspender, siendo Continuar el valor predeterminado. InformationVariable especifica una cadena como el nombre de una variable en la que desea guardar los datos de Write-Host de un comando.

    -   Una nueva variable de preferencia, InformationPreference, especifica su preferencia predeterminada para los datos de la secuencia de información en una sesión de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. El valor predeterminado es Continuar.

    -   Se agregaron dos nuevos parámetros de flujo de trabajo comunes, PSInformation e InformationAction.

    -   Cuando usa el comando Format-Table, las columnas de la tabla se formatean ahora automáticamente evaluando los primeros 300 ms de datos que pasan a través de la secuencia.

-   En colaboración con [Microsoft Research](http://research.microsoft.com/), se ha agregado un nuevo cmdlet ConvertFrom-String. ConvertFrom-String permite extraer y analizar objetos estructurados del contenido de las cadenas de texto. Para obtener más información, consulte ConvertFrom-String.

-   Un nuevo cmdlet Convert-String formatea automáticamente el texto según un ejemplo que se proporciona en un parámetro -Example.

-   Un nuevo módulo, Microsoft.PowerShell.Archive, incluye cmdlets que permiten comprimir archivos y carpetas en archivos de almacenamiento (también conocidos como ZIP), extraer los archivos de archivos ZIP existentes y actualizar archivos ZIP con versiones más recientes de los archivos comprimidos que contienen.

-   Un nuevo módulo, PackageManagement, permite detectar e instalar paquetes de software en Internet. El módulo PackageManagement (conocido anteriormente como OneGet) es un administrador o multiplexor de administradores de paquetes existentes (también denominados proveedores de paquetes) destinado a unificar la administración de paquetes de Windows con una única interfaz de Windows PowerShell.

-   Un nuevo módulo, PowerShellGet, permite buscar, instalar, publicar y actualizar los módulos y recursos de DSC en la [Galería de PowerShell](http://www.powershellgallery.com/), o en un repositorio de módulos interno que puede configurar ejecutando el cmdlet Register-PSRepository.

-   Una nueva palabra clave del lenguaje, **Hidden**, se agregó para especificar que un miembro (una propiedad o un método) no se muestra en los resultados de Get-Member de forma predeterminada (a menos que se agregue el parámetro -Force). Las propiedades o los métodos marcados como ocultos no se muestran en los resultados de IntelliSense, a menos que esté en un contexto donde el miembro deba ser visible; por ejemplo, la variable automática $This debería mostrar los miembros ocultos en el método de clase.

-   New-Item, Remove-Item y Get-ChildItem se han mejorado para admitir la creación y administración de [vínculos simbólicos](http://en.wikipedia.org/wiki/Symbolic_link). El parámetro **ItemType** de New-Item acepta un nuevo valor, **SymbolicLink**. Ahora puede crear vínculos simbólicos en una única línea ejecutando el cmdlet New-Item.

-   Get-ChildItem también tiene un nuevo parámetro -Depth, que se usa con el parámetro –Recurse para limitar la recursión. Por ejemplo, Get-ChildItem –Recurse –Depth 2 devuelve resultados de la carpeta actual, de todas las carpetas secundarias dentro de la carpeta actual y de todas las carpetas dentro de las carpetas secundarias.

-   Copy-Item permite ahora copiar archivos o carpetas de una sesión de [!INCLUDE[wps_2](../Token/wps_2_md.md)] en otra, lo que significa que puede copiar archivos en sesiones conectadas a equipos remotos (incluidos los equipos que ejecutan [Windows Nano Server](http://blogs.technet.com/b/windowsserver/archive/2015/04/08/microsoft-announces-nano-server-for-modern-apps-and-cloud.aspx) y que, por tanto, no tienen ninguna otra interfaz). Para copiar archivos, especifique los identificadores de PSSession como el valor de los nuevos parámetros -FromSession y -ToSession, y agregue –Path y –Destination para especificar la ruta de acceso de origen y el destino, respectivamente. Por ejemplo, Copy-Item -Path c:\myFile.txt -ToSession $s -Destination d:\destinationFolder.

-   La transcripción de [!INCLUDE[wps_2](../Token/wps_2_md.md)] se mejoró para aplicarse a todas las aplicaciones de hospedaje (como [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE) además de al host de consola (**powershell.exe**). Las opciones de transcripción (incluida la habilitación de una transcripción de todo el sistema) pueden configurarse habilitando la opción de Directiva de grupo **Activar la transcripción de PowerShell**, que se encuentra en Plantillas administrativas/Componentes de Windows/[!INCLUDE[wps_2](../Token/wps_2_md.md)].

-   La nueva característica de seguimiento detallado de scripts permite habilitar el seguimiento detallado y el análisis del uso de scripting de [!INCLUDE[wps_2](../Token/wps_2_md.md)] en un sistema. Después de habilitar el seguimiento detallado de scripts, [!INCLUDE[wps_2](../Token/wps_2_md.md)] registra todos los bloques de scripts en el registro de eventos de Seguimiento de eventos para Windows (ETW), **Microsoft-Windows-PowerShell/Operational**.

-   A partir de [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0, los nuevos cmdlets de sintaxis de mensajes de cifrado admiten el cifrado y descifrado de contenido mediante el formato estándar IETF para proteger los mensajes de forma criptográfica según se documenta en [RFC5652](http://tools.ietf.org/html/rfc5652). Los cmdlets Get-CmsMessage, Protect-CmsMessage y Unprotect-CmsMessage se han agregado al módulo [Microsoft.PowerShell.Security](http://technet.microsoft.com/library/hh849807.aspx).

-   Los nuevos cmdlets del módulo [Microsoft.PowerShell.Utility](http://technet.microsoft.com/library/hh849958.aspx), Get-Runspace, Debug-Runspace, Get-RunspaceDebug, Enable-RunspaceDebug y Disable-RunspaceDebug, permiten establecer opciones de depuración, así como iniciar y detener la depuración, en un espacio de ejecución. Para depurar espacios de ejecución arbitrarios, es decir, espacios de ejecución que no son el predeterminado de una consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)] o una sesión de [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE, [!INCLUDE[wps_2](../Token/wps_2_md.md)] permite establecer puntos de interrupción en un script y hacer que los puntos de interrupción agregados detengan la ejecución del script hasta que se pueda conectar un depurador para depurar el script del espacio de ejecución. Se ha agregado soporte técnico de depuración anidado para los espacios de ejecución arbitrarios al depurador de scripts de [!INCLUDE[wps_2](../Token/wps_2_md.md)] para los espacios de ejecución.

-   Un nuevo cmdlet Format-Hex se ha agregado al módulo [Microsoft.PowerShell.Utility](http://technet.microsoft.com/library/hh849958.aspx). Format-Hex permite ver datos de texto o binarios en formato hexadecimal.

-   Los cmdlets Get-Clipboard y Set-Clipboard se han agregado al módulo [Microsoft.PowerShell.Utility](http://technet.microsoft.com/library/hh849958.aspx); estos facilitan la transferencia de contenido hacia y desde la sesión de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. Los cmdlets Clipboard admiten imágenes, archivos de audio, listas de archivos y texto.

-   Se agregó un nuevo cmdlet, Clear-RecycleBin, al módulo [Microsoft.PowerShell.Management](http://technet.microsoft.com/library/hh849827(v=wps.640).aspx); este cmdlet vacía la Papelera de reciclaje de una unidad fija, que incluye las unidades externas. De forma predeterminada, se le pedirá que confirme un comando Clear-RecycleBin, porque la propiedad ConfirmImpact del cmdlet está establecida en ConfirmImpact.High.

-   Un nuevo cmdlet, New-TemporaryFile, permite crear un archivo temporal como parte del scripting. De forma predeterminada, el nuevo archivo temporal se crea en C:\Users\<user name>\AppData\Local\Temp.

-   Los cmdlets Out-File, Add-Content y Set-Content tienen ahora un nuevo parámetro –NoNewline, que omite una nueva línea después de la salida.

-   El cmdlet New-Guid aprovecha la clase Guid de .NET Framework para generar un GUID, que resulta útil al escribir scripts o recursos de DSC.

-   Dado que la información de la versión de archivo puede ser confusa, especialmente después de aplicar una revisión a un archivo, las nuevas propiedades de script FileVersionRaw y ProductVersionRaw están disponibles para los objetos FileInfo. Por ejemplo, puede ejecutar el siguiente comando para mostrar los valores de estas propiedades de PowerShell.exe, donde $pid contiene el identificador de proceso de una sesión en ejecución de Windows PowerShell: Get-Process -Id $pid -FileVersionInfo | Format-List *version* -Force

-   Los nuevos cmdlets Enter-PSHostProcess y Exit-PSHostProcess le permiten depurar scripts de [!INCLUDE[wps_2](../Token/wps_2_md.md)] en procesos independientes del proceso actual que se ejecuta en la consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. Ejecute Enter-PSHostProcess para introducir, o establecer la asociación con, un identificador de proceso específico y luego ejecute Get-Runspace para devolver los espacios de ejecución activos dentro del proceso. Ejecute Exit-PSHostProcess para desasociarse del proceso cuando acabe de depurar el script dentro del proceso.

-   Un nuevo cmdlet Wait-Debugger se ha agregado al módulo [Microsoft.PowerShell.Utility](http://technet.microsoft.com/library/hh849958.aspx). Puede ejecutar Wait-Debugger para detener un script en el depurador antes de ejecutar la siguiente instrucción del script.

-   El depurador del flujo de trabajo de [!INCLUDE[wps_2](../Token/wps_2_md.md)] admite ahora la finalización con comando o tabulación, y permite depurar funciones de flujo de trabajo anidadas. Ahora puede presionar **Ctrl+Interrumpir** para introducir el depurador en un script en ejecución, tanto en sesiones locales como remotas, así como en un script de flujo de trabajo.

-   Se ha agregado un cmdlet Debug-Job al módulo [Microsoft.PowerShell.Core](http://technet.microsoft.com/library/hh849695.aspx) para depurar scripts de trabajos en ejecución de flujo de trabajo de [!INCLUDE[wps_2](../Token/wps_2_md.md)], trabajos en segundo plano y trabajos que se ejecutan en sesiones remotas.

-   Se ha agregado un nuevo estado, AtBreakpoint, para los trabajos de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. El estado AtBreakpoint se aplica cuando un trabajo ejecuta un script que incluye puntos de interrupción establecidos y el script alcanza un punto de interrupción. Si un trabajo se detiene en un punto de interrupción de depuración, debe depurar el trabajo ejecutando el cmdlet Debug-Job.

-   [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 implementa la compatibilidad con varias versiones de un único módulo de [!INCLUDE[wps_2](../Token/wps_2_md.md)] en la misma carpeta en $PSModulePath. Se agregado una propiedad RequiredVersion a la clase ModuleSpecification para ayudarle a obtener la versión deseada de un módulo; esta propiedad es mutuamente exclusiva con la propiedad ModuleVersion. RequiredVersion se admite ahora como parte del valor del parámetro FullyQualifiedName de los cmdlets Get-Module, Import-Module y Remove-Module.

-   Ahora puede realizar la validación de la versión de módulo ejecutando el cmdlet Test-ModuleManifest.

-   Los resultados del cmdlet Get-Command muestran ahora una columna Version; se ha agregado una nueva propiedad Version a la clase CommandInfo. Get-Command muestra comandos de varias versiones del mismo módulo. La propiedad Version también forma parte de las clases derivadas de CmdletInfo: CmdletInfo y ApplicationInfo.

-   Get-Command tiene un nuevo parámetro, -ShowCommandInfo, que devuelve información de ShowCommand como PSObjects. Esta funcionalidad es especialmente útil cuando se ejecuta Show-Command en [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE mediante la comunicación remota de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. El parámetro –ShowCommandInfo reemplaza la función Get-SerializedCommand existente en el módulo Microsoft.PowerShell.Utility, pero el script Get-SerializedCommand sigue estando disponible para admitir el scripting de nivel inferior.

-   Un nuevo cmdlet Get-ItemPropertyValue permite obtener el valor de una propiedad sin usar la notación de puntos. Por ejemplo, en las versiones anteriores de [!INCLUDE[wps_2](../Token/wps_2_md.md)], puede ejecutar el comando siguiente para obtener el valor de la propiedad ApplicationBase de la clave del Registro PowerShellEngine: **(Get-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\PowerShell\3\PowerShellEngine -Name ApplicationBase).ApplicationBase**. A partir de [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0, puede ejecutar **Get-ItemPropertyValue -Path HKLM:\SOFTWARE\Microsoft\PowerShell\3\PowerShellEngine -Name ApplicationBase**.

-   La consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)] usa ahora el color de sintaxis, como en [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE.

-   Un nuevo módulo NetworkSwitch contiene cmdlets que permiten aplicar la configuración de modificador, LAN virtual (VLAN) y básica del puerto del modificador de red de capa 2 a los modificadores de red certificados con el logotipo de [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)].

-   El parámetro FullyQualifiedName se agregó a los cmdlets Import-Module y Remove-Module para permitir el almacenamiento de varias versiones de un mismo módulo.

-   Save-Help, Update-Help, Import-PSSession, Export-PSSession, y Get-Command tienen un nuevo parámetro, FullyQualifiedModule, de tipo ModuleSpecification. Agregue este parámetro para especificar un módulo con su nombre completo.

-   El valor de **$PSVersionTable.PSVersion** se ha actualizado a 5.0.

### <a name="BKMK_newDSC"></a>Nuevas características de configuración de estado deseado de Windows PowerShell

-   Las mejoras en el lenguaje de [!INCLUDE[wps_2](../Token/wps_2_md.md)] permiten definir los recursos de configuración de estado deseado (DSC) de [!INCLUDE[wps_2](../Token/wps_2_md.md)] mediante clases. Import-DscResource es ahora una verdadera palabra clave dinámica; [!INCLUDE[wps_2](../Token/wps_2_md.md)] analiza el módulo raíz del módulo especificado y busca las clases que contienen el atributo DscResource. Ahora puede usar clases para definir recursos de DSC, en los que no se requiere un archivo MOF ni una subcarpeta DSCResource en la carpeta del módulo. Un archivo de módulo de [!INCLUDE[wps_2](../Token/wps_2_md.md)] puede contener varias clases de recursos de DSC.

-   Se agregó un nuevo parámetro, ThrottleLimit, a los siguientes cmdlets en el módulo PSDesiredStateConfiguration. Agregue el parámetro ThrottleLimit para especificar el número de dispositivos o equipos de destino en los que quiere que el comando funcione al mismo tiempo.

    -   Get-DscConfiguration

    -   Get-DscConfigurationStatus

    -   Get-DscLocalConfigurationManager

    -   Restore-DscConfiguration

    -   Test-DscConfiguration

    -   Compare-DscConfiguration

    -   Publish-DscConfiguration

    -   Set-DscLocalConfigurationManager

    -   Start-DscConfiguration

    -   Update-DscConfiguration

-   Con los informes de errores de DSC centralizados, no solo se registra información de error completa en el registro de eventos, sino que también se envía a una ubicación central para su posterior análisis. Puede usar esta ubicación central para almacenar los errores de configuración de DSC que se han producido para cualquier servidor de su entorno. Después de definir el servidor de informes en la metaconfiguración, todos los errores se envían al servidor de informes y, luego, se almacenan en una base de datos. Puede configurar esta funcionalidad independientemente de si se configura un nodo de destino para extraer las configuraciones de un servidor de extracción.

-   Las mejoras en [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE facilitan la creación de recursos de DSC. Ahora puede hacer lo siguiente.

    -   Enumeración de todos los recursos de DSC en un bloque de **configuración** o un bloque de **nodo** escribiendo **Ctrl+Space** en una línea en blanco dentro del bloque.

    -   Finalización automática de propiedades de recurso de tipo **enumeración**.

    -   Finalización automática de la propiedad **DependsOn** de los recursos de DSC, en función de otras instancias de recurso de la configuración.

    -   Finalización con tabulación mejorada de los valores de propiedad de recurso.

-   Un usuario puede ejecutar ahora un recurso en un conjunto de credenciales especificado agregando el atributo **PSDscRunAsCredential** a un bloque de nodo. Por ejemplo, PSDscRunAsCredential = Get-Credential Contoso\DscUser. Esta funcionalidad es útil para crear configuraciones que ejecuten Windows Installer e instaladores ejecutables, accedan al subárbol del Registro por usuario o realicen otras tareas fuera del contexto del usuario actual.

-   Se agregó compatibilidad con la versión de 32 bits (basada en x86) para la palabra clave **Configuration**.

-   [!INCLUDE[wps_2](../Token/wps_2_md.md)] ahora ofrece compatibilidad con la ayuda personalizada para configuraciones de DSC, que se define mediante la adición de [CmdletBinding()] a la función de configuración generada.

-   Un nuevo atributo **DscLocalConfigurationManager** designa un bloque de configuración como una metaconfiguración, que se usa para configurar el Administrador de configuración local de DSC. Este atributo restringe una configuración para que únicamente contenga elementos que configuren el Administrador de configuración local de DSC. Durante el procesamiento, esta configuración genera un archivo *.meta.mof que se envía a los nodos de destino apropiados mediante el cmdlet Set-DscLocalConfigurationManager.

-   Ahora se admiten configuraciones parciales en [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0. Puede entregar documentos de configuración a un nodo en fragmentos. Para que un nodo reciba varios fragmentos de un documento de configuración, su Administrador de configuración local debe estar establecido para especificar los fragmentos esperados.

-   La sincronización entre equipos es una novedad de DSC en [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0. Con los recursos integrados WaitFor* (**WaitForAll**, **WaitForAny** y **WaitForSome**), ahora puede especificar las dependencias entre equipos durante las ejecuciones de configuración, sin orquestación externa. Estos recursos ofrecen sincronización de nodo a nodo a través de conexiones CIM en el protocolo WS-Man. Una configuración puede esperar el cambio de estado del recurso específico de otro equipo.

-   Just Enough Administration (JEA), una nueva característica de seguridad de delegación, aprovecha DSC y los espacios de ejecución restringidos de [!INCLUDE[wps_2](../Token/wps_2_md.md)] para ayudar a las empresas a evitar la pérdida de datos o riesgos causados por empleados, ya sea intencionada o involuntariamente. Para obtener más información sobre JEA, incluida la ubicación donde puede descargar el recurso DSC de xJEA, consulte [Just Enough Administration, Step by Step](http://blogs.technet.com/b/privatecloud/archive/2014/05/14/just-enough-administration-step-by-step.aspx) (Just Enough Administration, paso a paso).

-   Se agregaron los siguientes cmdlets nuevos al módulo PSDesiredStateConfiguration.

    -   Un nuevo cmdlet Get-DscConfigurationStatus obtiene información de alto nivel acerca del estado de configuración desde un nodo de destino. Puede obtener el estado de todas las configuraciones o solo de la última.

    -   Un nuevo cmdlet Compare-DscConfiguration compara una configuración especificada con el estado real de uno o varios nodos de destino.

    -   Un nuevo cmdlet Publish-DscConfiguration copia un archivo MOF de configuración en un nodo de destino, pero no aplica la configuración. La configuración se aplica durante el siguiente paso de coherencia o cuando se ejecuta el cmdlet Update-DscConfiguration.

    -   Un nuevo cmdlet Test-DscConfiguration permite comprobar si una configuración resultante coincide con la configuración deseada, para lo cual devuelve True si la configuración coincide con la configuración deseada o False si no coincide.

    -   Un nuevo cmdlet Update-DscConfiguration fuerza el procesamiento de una configuración. Si el Administrador de configuración Local está en modo de extracción, el cmdlet obtiene la configuración del servidor de extracción antes de aplicarla.

### <a name="BKMK_newISE"></a>Nuevas características de Windows PowerShell ISE

-   Ahora puede editar scripts y archivos de [!INCLUDE[wps_2](../Token/wps_2_md.md)] remoto en una copia local de [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE. Para ello, debe ejecutar Enter-PSSession para iniciar una sesión remota en el equipo que almacena los archivos que desea editar y, después, ejecutar **PSEdit <path and file name on the remote computer>**. Esta característica facilita la edición de archivos de [!INCLUDE[wps_2](../Token/wps_2_md.md)] que están almacenados en la opción de instalación Server Core de Windows Server, donde [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE no puede ejecutarse.

-   El cmdlet Start-Transcript se admite ahora en [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE.

-   Ahora puede depurar scripts remotos en [!INCLUDE[wps_2](../Token/wps_2_md.md)] ISE.

-   Un nuevo comando de menú, **Break-All** (Ctrl+B) entra en el depurador para los scripts que se ejecutan de forma local y remota.

### <a name="BKMK_newOData"></a>Nuevas características de servicios web de Windows PowerShell (Extensión IIS Management OData)

-   A partir de [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0, puede generar un conjunto de cmdlets [!INCLUDE[wps_2](../Token/wps_2_md.md)] en función de la funcionalidad expuesta por un extremo de OData determinado, mediante la ejecución del cmdlet Export-ODataEndpointProxy, que se encuentra en el nuevo módulo [Microsoft.PowerShell.OdataUtils](http://technet.microsoft.com/library/dn818507(v=wps.640).aspx).

### <a name="BKMK_5bugfix"></a>Correcciones de errores importantes en Windows PowerShell 5.0

-   [!INCLUDE[wps_2](../Token/wps_2_md.md)] 5.0 incluye una nueva implementación de COM, que ofrece importantes mejoras de rendimiento para el trabajo con objetos COM. Para ver una demostración en vídeo del efecto, consulte [Com_Perf_Improvements](http://1drv.ms/1qu3UPZ).

-   Se realizaron mejoras de rendimiento significativas en la primera finalización con tabulación en una sesión de [!INCLUDE[wps_2](../Token/wps_2_md.md)], lo que reduce el tiempo de finalización con tabulación en casi 500 ms.

## <a name="BKMK_wps4"></a>Nuevas características de Windows PowerShell 4.0
[!INCLUDE[psversion4](../Token/psversion4_md.md)] es compatible con versiones anteriores. Los cmdlets, proveedores, módulos, complementos, scripts, funciones y perfiles diseñados para [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion2](../Token/psversion2_md.md)] funcionan en [!INCLUDE[psversion4](../Token/psversion4_md.md)] sin tener que cambiar nada.

[!INCLUDE[psversion4](../Token/psversion4_md.md)] está instalado de forma predeterminada en [!INCLUDE[winblue_client_1](../Token/winblue_client_1_md.md)] y en [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)]. Para instalar [!INCLUDE[psversion4](../Token/psversion4_md.md)] en Windows 7 con SP1 o Windows Server 2008 R2, descargue e instale [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855). Procure leer los detalles de la descarga y cumplir todos los requisitos del sistema antes de instalar Windows Management Framework 4.0.

-   [Nuevas características de Windows PowerShell](#BKMK_core)

-   [Nuevas características del Entorno de scripting integrado (ISE) de Windows PowerShell](#BKMK_ise)

-   [Nuevas características del flujo de trabajo de Windows PowerShell](#BKMK_workflow)

-   [Nuevas características de servicios web de Windows PowerShell](#BKMK_psws)

-   [Nuevas características de Windows PowerShell Web Access](#BKMK_powwa)

-   [Correcciones de errores importantes en Windows PowerShell 4.0](#BKMK_bugs)

[!INCLUDE[psversion4](../Token/psversion4_md.md)] incluye las siguientes características nuevas.

### <a name="BKMK_core"></a>Nuevas características de Windows PowerShell

-   **La configuración de estado deseado (DSC) de [!INCLUDE[wps_2](../Token/wps_2_md.md)]** es un nuevo sistema de administración de [!INCLUDE[psversion4](../Token/psversion4_md.md)] que permite implementar y administrar los datos de configuración de servicios de software y el entorno en el que se ejecutan estos servicios. Para más información sobre DSC, consulte [Introducción a la configuración de estado deseado de Windows PowerShell](assetId:///c134aa32-b085-4656-9a89-955d8ff768d0).

-   **Save-Help** permite guardar ahora la ayuda relativa a los módulos instalados en equipos remotos. Puede usar Save-Help para descargar la Ayuda del módulo de un cliente conectado a Internet (en el que no tienen por qué estar instalados todos los módulos de los que desea obtener ayuda). Luego, puede copiar la Ayuda guardada en una carpeta compartida remota o en un equipo remoto sin acceso a Internet.

-   El depurador de [!INCLUDE[wps_2](../Token/wps_2_md.md)] se mejoró para permitir la depuración de los flujos de trabajo de [!INCLUDE[wps_2](../Token/wps_2_md.md)], así como de los scripts que se ejecutan en equipos remotos. Ahora, los flujos de trabajo de [!INCLUDE[wps_2](../Token/wps_2_md.md)] se pueden depurar en el nivel de script tanto desde la línea de comandos de [!INCLUDE[wps_2](../Token/wps_2_md.md)] como desde [!INCLUDE[ise_2](../Token/ise_2_md.md)]. Ahora, los scripts de [!INCLUDE[wps_2](../Token/wps_2_md.md)] (y también los flujos de trabajo de scripts) se pueden depurar en sesiones remotas. Las sesiones de depuración remotas se conservan en sesiones remotas de [!INCLUDE[wps_2](../Token/wps_2_md.md)] que se desconectan y, luego, se vuelven a conectar.

-   Gracias al parámetro **RunNow** de **Register-ScheduledJob** y **Set-ScheduledJob**, ya no es necesario definir la fecha y hora de inicio inmediatas en los trabajos con el parámetro **Trigger**.

-   Ahora, **Invoke-RestMethod** e **Invoke-WebRequest** permiten definir todos los encabezados con el parámetro Headers. Aunque este parámetro siempre ha existido, era uno de los diversos parámetros para cmdlets web que provocaban errores o excepciones.

-   **Get-Module** tiene un nuevo parámetro, **FullyQualifiedName**, del tipo **ModuleSpecification[]**. Ahora, el parámetro **FullyQualifiedName** de Get-Module permite especificar un módulo mediante su nombre, versión y, opcionalmente, su GUID.

-   La configuración de directiva de ejecución predeterminada en [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)] es **RemoteSigned**. En [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], no hay ningún cambio en la configuración predeterminada.

-   A partir de [!INCLUDE[psversion4](../Token/psversion4_md.md)], es posible invocar métodos mediante nombres de método dinámicos. Así, se puede usar una variable para almacenar un nombre de método y, luego, invocar dinámicamente el método llamando a esa variable.

-   Los trabajos de flujo de trabajo asincrónicos ya no se eliminan cuando ha transcurrido el tiempo de espera especificado en el parámetro común de flujo de trabajo **PSElapsedTimeoutSec**.

-   Se ha agregado un nuevo parámetro (**RepeatIndefinitely**) a los cmdlets **New-JobTrigger** y **Set-JobTrigger**. Con este parámetro, ya no es necesario especificar un valor **TimeSpan.MaxValue** para el parámetro **RepetitionDuration** para que un trabajo programado se ejecute de forma repetida durante un período indefinido.

-   Se ha agregado un parámetro **Passthru** a los cmdlets **Enable-JobTrigger** y **Disable-JobTrigger**. El parámetro Passthru muestra los objetos que el comando crea o modifica.

-   Ahora, los nombres de parámetro para especificar un grupo de trabajo en los cmdlets **Add-Computer** y **Remove-Computer** son coherentes, ya que ambos cmdlets usan el parámetro **WorkgroupName**.

-   Se ha agregado un nuevo parámetro común, **PipelineVariable**. PipelineVariable permite guardar los resultados de un comando canalizado (o de parte de un comando canalizado) como una variable que se puede pasar a través del resto de la canalización.

-   Ahora se puede usar una sintaxis de método para filtrar colecciones. Esto significa que una colección de objetos se puede filtrar usando una sintaxis simplificada, similar a la de Where() o Where-Object, con el formato de una llamada a método. Este es un ejemplo: (Get-Process).where({$_.Name -match 'powershell'})

-   El cmdlet **Get-Process** tiene un nuevo parámetro de modificador, **IncludeUserName**.

-   Se ha agregado un nuevo cmdlet, **Get-FileHash**, que devuelve un hash de archivo en uno de los distintos formatos especificados para un archivo.

-   En [!INCLUDE[psversion4](../Token/psversion4_md.md)], si un módulo usa la clave **DefaultCommandPrefix** en su manifiesto, o si el usuario importa un módulo con el parámetro **Prefix**, la propiedad **ExportedCommands** del módulo muestra los comandos en el módulo con el prefijo. Cuando se ejecutan comandos usando la sintaxis de módulo completo (nombreDelMódulo\nombreDelComando), los nombres de comando deben incluir el prefijo.

-   El valor de **$PSVersionTable.PSVersion** se ha actualizado a 4.0.

-   El comportamiento del operador **Where()** ha cambiado. Ya no se admite que `Collection.Where('property –match name')` acepte una expresión de cadena con el formato `"Property –CompareOperator Value"`. Sin embargo, el operador **Where()** sí sigue aceptando expresiones de cadena con formato de bloque de script.

### <a name="BKMK_ise"></a>Nuevas características del Entorno de scripting integrado (ISE) de Windows PowerShell

-   [!INCLUDE[ise_2](../Token/ise_2_md.md)] admite la depuración tanto de [!INCLUDE[ps_workflow_2](../Token/ps_workflow_2_md.md)] como de scripts remotos.

-   Se ha agregado compatibilidad con IntelliSense para los proveedores y configuraciones de configuración deseada de estado de [!INCLUDE[wps_2](../Token/wps_2_md.md)].

### <a name="BKMK_workflow"></a>Nuevas características del flujo de trabajo de Windows PowerShell

-   Se ha agregado compatibilidad con un nuevo parámetro común **PipelineVariable** en el contexto de las canalizaciones iterativas, como las que se usan en System Center Orchestrator; es decir, canalizaciones que ejecutan comandos simplemente de izquierda a derecha, en contraposición a la ejecución intercalada mediante streaming.

-   El enlace de parámetros ha mejorado notablemente y ahora funciona fuera de escenarios de finalización con tabulación, como sucede con los comandos que no existen en el espacio de ejecución actual.

-   Se ha agregado compatibilidad en [!INCLUDE[ps_workflow_2](../Token/ps_workflow_2_md.md)] para las actividades de contenedor personalizadas. Si un parámetro de actividad es de tipo **Activity** o **Activity[]** (o es una colección genérica de actividades) y el usuario ha especificado un bloque de script como argumento, [!INCLUDE[ps_workflow_2](../Token/ps_workflow_2_md.md)] convierte el bloque de script a XAML, como sucede con la compilación de script a flujo de trabajo de [!INCLUDE[wps_2](../Token/wps_2_md.md)] habitual.

-   Después de un bloqueo, el flujo de trabajo de [!INCLUDE[wps_2](../Token/wps_2_md.md)] vuelve a conectarse automáticamente a los nodos administrados.

-   Ahora, puede limitar las instrucciones de actividad de **Foreach -Parallel** con la propiedad **ThrottleLimit**.

-   El parámetro común **ErrorAction** tiene un nuevo valor válido, **Suspend**, exclusivo para flujos de trabajo.

-   Ahora, un extremo de flujo de trabajo se cierra automáticamente si no hay sesiones activas, trabajos en curso o trabajos pendientes. Esta característica conserva los recursos en el equipo que actúa como servidor de flujo de trabajo cuando se cumplen las condiciones de cierre automático.

### <a name="BKMK_psws"></a>Nuevas características de servicios web de Windows PowerShell

-   Si se produce un error en los servicios web de [!INCLUDE[wps_2](../Token/wps_2_md.md)] (PSWS, también denominado Extensión IIS Management OData) mientras se ejecuta un cmdlet, los mensajes de error que recibe el autor de la llamada son más detallados. Además, los códigos de error siguen las [directrices de códigos de error de la API de REST de Azure](http://msdn.microsoft.com/library/windowsazure/dd179357.aspx).

-   Ahora, un extremo puede definir la versión de API, así como exigir el uso de una versión específica de la API. Si se produce una discrepancia de versión entre el cliente y el servidor, los errores se muestran tanto en el cliente como en el servidor.

-   La administración del esquema de distribución se ha simplificado, ya que ahora los valores de los campos que faltan en el esquema se generan automáticamente. Los valores se generan aunque el esquema de distribución no exista, lo que constituye un punto de partida de gran ayuda.

-   El tratamiento de tipos en PSWS se mejoró y ahora admite tipos que usan un constructor diferente al constructor predeterminado, con un comportamiento similar al de **PSTypeConverter** en [!INCLUDE[wps_2](../Token/wps_2_md.md)]. Esto permite usar tipos complejos con PSWS.

-   Ahora, PSWS permite expandir una instancia asociada mientras una consulta se ejecuta. El coste de transferencia de contenido binario de gran volumen (como imágenes, audio o vídeos) es significativo, de modo que es mejor transferir datos binarios sin codificación. PSWS usa secuencias de recurso con nombre para realizar transferencias sin codificación. La secuencia de recurso con nombre es una propiedad de una entidad de tipo **Edm.Stream**. Cada secuencia de recurso con nombre tiene un URI diferente para las operaciones GET o UPDATE.

-   Ahora, las acciones de OData proporcionan un mecanismo para invocar métodos distintos a Crear, Leer, Actualizar y Eliminar en un recurso. Se puede invocar una acción enviando una solicitud HTTP POST al URI definido para la acción. Los parámetros de la acción se definen en el cuerpo de la solicitud POST.

-   A fin de mantener la coherencia con las directrices de Microsoft Azure, es necesario simplificar todas las direcciones URL. Un cambio incluido en **Key As Segment** permite representar claves sencillas como segmentos. Tenga en cuenta que, como antes, las referencias en las que se usan varios valores de clave requieren valores separados por comas en la notación entre paréntesis.

-   Antes de esta versión de PSWS, la única forma de llevar a cabo operaciones de creación, actualización y eliminación consistía en invocar Post, Put o Delete en un recurso de primer nivel. La novedad en esta versión de PSWS es que las operaciones de recursos incluidos permiten a los usuarios lograr los mismos resultados cuando el acceso al mismo recurso es menos directo, como si ese recurso fuera un recurso incluido.

### <a name="BKMK_powwa"></a>Nuevas características de Windows PowerShell Web Access

-   Puede desconectarse de sesiones existentes y volver a conectarse a ellas en la consola de [!INCLUDE[pswa_2](../Token/pswa_2_md.md)] basada en Web. Un botón **Guardar** en la consola basada en Web permite desconectarse de una sesión sin eliminarla y volver a conectarse a ella otra vez.

-   Los parámetros predeterminados se pueden mostrar en la página de inicio de sesión. Para mostrar los parámetros predeterminados, configure los valores de todas las opciones recogidas en el área **Configuración de conexión opcional** de la página de inicio de sesión en un archivo denominado **web.config**. Puede usar el archivo **web.config** para configurar todas las opciones de conexión opcionales, salvo otro conjunto de credenciales.

-   En [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], las reglas de autorización de [!INCLUDE[pswa_2](../Token/pswa_2_md.md)] se pueden administrar de forma remota. Ahora, los cmdlets **Add-PswaAuthorizationRule** y **Test-PswaAuthorizationRule** incluyen un parámetro Credential con el que los administradores pueden administrar reglas de autorización desde un equipo remoto o en una sesión de [!INCLUDE[pswa_2](../Token/pswa_2_md.md)].

-   Ahora puede tener varias sesiones de [!INCLUDE[pswa_2](../Token/pswa_2_md.md)] en una única sesión de explorador usando una nueva pestaña de explorador en cada sesión. Ya no hay que abrir una nueva sesión de explorador para conectarse a una nueva sesión en la consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)] basada en Web.

### <a name="BKMK_bugs"></a>Correcciones de errores importantes en Windows PowerShell 4.0

-   **Get-Counter ** puede ahora devolver contadores que contienen un carácter de apóstrofo en las ediciones en francés de Windows.

-   Ahora el método **GetType** se puede ver en objetos deserializados.

-   Las instrucciones **#Requires** permiten ahora a los usuarios requerir derechos de acceso de administrador si así lo precisan.

-   El cmdlet **Import-Csv** omite ahora las líneas vacías.

-   Se solucionó un problema en el que [!INCLUDE[ise_2](../Token/ise_2_md.md)] usaba demasiado memoria al ejecutar un comando **Invoke-WebRequest**.

-   **Get-Module** muestra ahora las versiones de módulo en una columna **Version**.

-   Remove-Item –Recurse quita ahora los elementos de las subcarpetas según lo previsto.

-   Se agregó una propiedad **UserName** a los objetos de salida **Get-Process**.

-   El cmdlet **Invoke-RestMethod** devuelve ahora todos los resultados disponibles.

-   Ahora, **Add-Member** surte efecto en las tablas hash, aun cuando no se haya tenido acceso a esas tablas hash.

-   **Select-Object –Expand** ya no genera un error o una excepción si el valor de la propiedad es null o está vacío.

-   **Get-Process** se puede usar ahora en una canalización con otros comandos que obtienen la propiedad **ComputerName** de los objetos.

-   Ahora, **ConverTo-Json** y **ConvertFrom-Json** pueden aceptar términos entre comillas dobles y sus mensajes de error son localizables.

-   **Get-Job** devuelve ahora cualquier trabajo programado completado, incluso en las nuevas sesiones.

-   Se han solucionado los problemas con el montaje y desmontaje de VHD con el proveedor **FileSystem** de [!INCLUDE[psversion4](../Token/psversion4_md.md)]. Ahora, [!INCLUDE[wps_2](../Token/wps_2_md.md)] es capaz de detectar nuevas unidades de disco cuando se montan en la misma sesión.

-   Ya no es necesario cargar explícitamente los módulos **ScheduledJob** o **Workflow** para trabajar con sus tipos de trabajo.

-   Se han realizado mejoras de rendimiento en el proceso de importación de flujos de trabajo que definen los flujos de trabajo anidados. Este proceso es ahora más rápido.

## <a name="BKMK_wps3"></a>Nuevas características de Windows PowerShell 3.0
[!INCLUDE[psversion3](../Token/psversion3_md.md)] incluye las siguientes características nuevas.

-   [Flujo de trabajo de Windows PowerShell](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Workflow)

-   [Windows PowerShell Web Access](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_WebAccess)

-   [Nuevas características de Windows PowerShell ISE](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_ISE)

-   [Compatibilidad con Microsoft .NET Framework 4.0](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_NET4)

-   [Compatibilidad con el Entorno de preinstalación de Windows](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_WinPE)

-   [Sesiones desconectadas](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Disconnected)

-   [Conectividad de sesiones sólida](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Robust)

-   [Sistema de ayuda actualizable](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_UpHelp)

-   [Mejor ayuda en línea](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Online)

-   [Integración de CIM](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_CIM)

-   [Archivos de configuración de sesión](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_ConfigFile)

-   [Integración de trabajos programados y del programador de tareas](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_ScheduledJob)

-   [Mejoras en el lenguaje de Windows PowerShell](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Lang)

-   [Nuevos cmdlets principales](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Core)

-   [Mejoras en los proveedores y cmdlets principales existentes](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_Prov)

-   [Detección e importación de módulos remotos](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_REM)

-   [Mejor finalización con tabulación](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_TAB)

-   [Carga automática de módulos](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_AutoLoad)

-   [Mejoras en la experiencia de módulo](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_MOD)

-   [Detección de comandos simplificada](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_SIMPLE)

-   [Mejor compatibilidad con registros, diagnósticos y directiva de grupo](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_LOG)

-   [Mejoras de formato y salida](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_OUT)

-   [Mejor experiencia de host de consola](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_HOST)

-   [Nuevas API de cmdlets y de hospedaje](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_API)

-   [Mejoras en el rendimiento](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_PERF)

-   [Compatibilidad con RunAs y host compartido](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_RUNAS)

-   [Mejoras en el tratamiento de caracteres especiales](../Topic/What-s-New-in-Windows-PowerShell.md#BKMK_CHAR)

### <a name="BKMK_Workflow"></a>Flujo de trabajo de Windows PowerShell
[!INCLUDE[ps_workflow_1](../Token/ps_workflow_1_md.md)] reúne toda la eficacia de Windows Workflow Foundation en [!INCLUDE[mshshort](../Token/mshshort_md.md)]. Puede escribir flujos de trabajo en XAML o en el lenguaje de [!INCLUDE[mshshort](../Token/mshshort_md.md)] y ejecutarlos tal y como ejecutaría un cmdlet. El cmdlet [Get-Command](assetId:///59c6d302-6e8c-48b7-a6f6-f0172df936ad) obtiene comandos de flujo de trabajo y el cmdlet [Get-Help](assetId:///1f46eeb4-49d7-4bec-bb29-395d9b42f54a) obtiene ayuda para los flujos de trabajo.

Los flujos de trabajo son secuencias de actividades de administración de varios equipos de larga ejecución, repetibles, frecuentes y que se pueden paralelizar, interrumpir, suspender y reiniciar. Los flujos de trabajo se pueden reanudar desde una interrupción intencional o accidental, como un corte del suministro eléctrico, un reinicio de Windows o una interrupción de la red.

Los flujos de trabajo son, además, portátiles. Pueden exportarse como archivos XAML o importarse desde ellos. Puede escribir configuraciones de sesión personalizadas que permitan que usuarios delegados o subordinados ejecuten el flujo de trabajo o las actividades de un flujo de trabajo.

A continuación se enumeran las ventajas del [!INCLUDE[ps_workflow_2](../Token/ps_workflow_2_md.md)].

-   **Automatización de tareas secuenciadas y de larga ejecución.**

-   **Supervisión remota de tareas de larga ejecución**. El estado y progreso de las actividades están visibles en cualquier momento.

-   **Administración de varios equipos.** Ejecute tareas como flujos de trabajo simultáneamente en cientos de nodos administrados. [!INCLUDE[ps_workflow_2](../Token/ps_workflow_2_md.md)] incluye una biblioteca integrada de parámetros de administración comunes, como **PSComputerName**, que hacen posibles escenarios de administración de varios equipos.

-   **Ejecución de procesos complejos con una tarea única.** En un solo flujo de trabajo se pueden combinar varios scripts relacionados que implementen un escenario completo.

-   **Persistencia**: un flujo de trabajo se guarda (o se marca) en puntos específicos definidos por su autor, de forma que ese flujo de trabajo se puede reanudar desde la última tarea (o punto de comprobación) que se ha guardado, en lugar de tener que reiniciarlo desde el principio.

-   **Solidez.** Recuperación de errores automatizada. Los flujos de trabajo son inmunes a los reinicios, tanto si son planeados como si no. Por lo tanto, se puede suspender la ejecución de un flujo de trabajo y, a continuación, reanudar el flujo de trabajo desde el último punto de persistencia. Los autores de flujos de trabajo pueden designar actividades específicas para que se vuelvan a ejecutar en caso de error en uno o más nodos administrados.

-   **Capacidad para desconectarse, reconectarse y ejecutarse en sesiones desconectadas.** Los usuarios se pueden conectar y desconectar del servidor de flujo de trabajo, pero este sigue ejecutándose de manera continua. Puede cerrar la sesión en el equipo cliente o reiniciar el equipo cliente y supervisar la ejecución del flujo de trabajo desde otro equipo sin interrumpirlo.

-   **Programación.** Las tareas de flujo de trabajo se pueden programar, como cualquier script o cmdlet de [!INCLUDE[mshshort](../Token/mshshort_md.md)].

-   **Limitación del flujo de trabajo y la conexión.** La ejecución del flujo de trabajo y las conexiones a los nodos se pueden limitar, lo que permite escenarios de alta disponibilidad y escalabilidad.

### <a name="BKMK_WebAccess"></a>Windows PowerShell Web Access
[!INCLUDE[pswa_1](../Token/pswa_1_md.md)] es una característica de [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)] con la que los usuarios pueden ejecutar comandos y scripts de Windows PowerShell en una consola basada en Web. Los dispositivos que usan la consola basada en Web no precisan de la instalación de [!INCLUDE[mshshort](../Token/mshshort_md.md)], de un software de administración remota o un complemento de explorador. Lo único que se necesita es una puerta de enlace de [!INCLUDE[pswa_2](../Token/pswa_2_md.md)] correctamente configurada y un explorador del dispositivo cliente que admita JavaScript® y acepte cookies.

Para obtener más información, consulte [Windows PowerShell Web Access](http://go.microsoft.com/fwlink/p/?LinkID=221050).

### <a name="BKMK_ISE"></a>Nuevas características de Windows PowerShell ISE
En [!INCLUDE[psversion3](../Token/psversion3_md.md)], [!INCLUDE[ise_1](../Token/ise_1_md.md)], posee un gran número de características nuevas, como IntelliSense, la ventana Show-Command, un panel de consola unificado, fragmentos de código, coincidencia de llaves, expansión y contracción de secciones, guardado automático, lista de elementos recientes, copia enriquecida, copia en bloque y plena compatibilidad para escribir flujos de trabajo de scripts de Windows PowerShell. Para más información, consulte [about_Windows_PowerShell_ISE [v3]](assetId:///dfa54d47-60c6-4fff-8197-c747e8d411bb).

### <a name="BKMK_NET4"></a>Compatibilidad con Microsoft .NET Framework 4
Windows PowerShell se basa en Common Language Runtime 4.0. Los autores de cmdlets, scripts y flujos de trabajo pueden usar las nuevas clases de Microsoft .NET Framework 4 en Windows PowerShell, con características como compatibilidad e implementación de aplicaciones, Managed Extensibility Framework, informática en paralelo, redes, Windows Communication Foundation y Windows Workflow Foundation.

### <a name="BKMK_WinPE"></a>Compatibilidad con el Entorno de preinstalación de Windows
Windows PowerShell 3.0 es un componente opcional del Entorno de preinstalación de Windows (Windows PE) 4.0 para Windows 8. Windows PE es un sistema operativo mínimo que inicia un equipo que no tiene ningún sistema operativo y lo prepara para instalar Windows. Windows PE se puede usar para particionar y formatear discos duros, copiar imágenes de disco en un equipo e iniciar el programa de instalación de Windows desde un recurso compartido de red. Windows PowerShell 3.0, a su vez, se puede usar en Windows PE para administrar la implementación, los diagnósticos y los escenarios de recuperación.

### <a name="BKMK_Disconnected"></a>Sesiones desconectadas
A partir de Windows PowerShell 3.0, las sesiones persistentes administradas por el usuario ("PSSessions") que se creen con el cmdlet New-PSSession se guardarán en el equipo remoto. Ya no son dependientes de la sesión en la que se han creado.

Ahora puede desconectarse de una sesión sin interrumpir los comandos que se ejecutan en ella. Puede cerrar la sesión y apagar el equipo. Más adelante, puede volver a conectarse a la sesión desde una sesión diferente en el mismo equipo o en otro diferente.

El parámetro **ComputerName** del cmdlet [Get-PSSession](assetId:///b2b10531-d0df-4746-b877-e75c09955cb6) obtiene ahora todas las sesiones de usuario que se conectan al equipo, aunque se hayan iniciado en una sesión diferente en otro equipo. Puede conectarse a las sesiones, obtener los resultados de comandos, iniciar nuevos comandos y, tras ello, desconectarse de la sesión.

Se han incorporado nuevos cmdlets para dar cabida a la característica de sesiones desconectadas (como [Disconnect-PSSession](assetId:///f8f95111-612f-4cba-9098-77904b0473d8), [Connect-PSSession](assetId:///b803dd29-f208-4079-80d4-db04d778f060) y Receive-PSSession) y, de igual modo, se han agregado nuevos parámetros a los cmdlets con los que se administran PSSessions, como el parámetro **InDisconnectedSession** del cmdlet [Invoke-Command](assetId:///906b4b41-7da8-4330-9363-e7164e5e6970).

La característica de sesiones desconectadas es posible únicamente cuando los equipos en los extremos tanto de origen ("cliente") como de finalización ("servidor") de la conexión ejecutan Windows PowerShell 3.0.

### <a name="BKMK_Robust"></a>Conectividad de sesiones sólida
Windows PowerShell 3.0 detecta pérdidas inesperadas de conectividad entre el cliente y el servidor e intenta restablecerla y reanudar la ejecución automáticamente. Si no se puede restablecer la conexión cliente-servidor en el tiempo asignado, se notifica al usuario y la sesión se desconecta. Durante el intento de reconexión, Windows PowerShell ofrece información continuamente al usuario.

Si la sesión desconectada se inició con InvokeCommand, Windows PowerShell crea un trabajo relativo a la sesión desconectada para que sea más fácil volver a conectarse y reanudar la ejecución.

Estas características proporcionan una experiencia de comunicación remota más confiable y recuperable y, asimismo, permiten a los usuarios realizar tareas de larga ejecución que requieren sesiones sólidas, como los flujos de trabajo.

### <a name="BKMK_UpHelp"></a>Sistema de ayuda actualizable
Ahora puede descargar archivos de ayuda actualizados correspondientes a los cmdlets de sus módulos. El cmdlet [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545) identifica los archivos de ayuda más recientes, los descarga de Internet, los desempaqueta y valida y, por último, los instala en el directorio específico del idioma pertinente del módulo.

Para usar los archivos de ayuda actualizados, basta con escribir `Get-Help`. No es necesario reiniciar Windows ni [!INCLUDE[mshshort](../Token/mshshort_md.md)]. Para actualizar la ayuda de los módulos del directorio $psHome, inicie [!INCLUDE[mshshort](../Token/mshshort_md.md)] con la opción "Ejecutar como administrador".

Para admitir usuarios que no tienen acceso a Internet y usuarios detrás de firewalls, el nuevo cmdlet [Save-Help](assetId:///aed94f90-b73f-4e25-a25d-7c18d9f161fa) descarga los archivos de ayuda en un directorio del sistema de archivos, como un recurso compartido de archivo. De este modo, podrán usar el cmdlet [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545) para obtener archivos de ayuda actualizados desde el recurso compartido de archivo.

El cmdlet [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545) se puede usar para actualizar los archivos de ayuda de todos los módulos (o solo de algunos en particular) en los idiomas de interfaz de usuario compatibles. También puede incluir un comando [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545) en su perfil de [!INCLUDE[mshshort](../Token/mshshort_md.md)]. [!INCLUDE[mshshort](../Token/mshshort_md.md)] descarga los archivos de ayuda de un módulo no más de una vez al día de forma predeterminada.

Los módulos [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)] y [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)] no incluyen archivos de ayuda. Para descargar los archivos de ayuda más recientes, escriba `Update-Help`. Para más información, escriba `Get-Help` (sin parámetros) o consulte [about_Updatable_Help](assetId:///10bba75c-f4ac-4ca1-bbf3-8f34dd521ffe).

Si los archivos de ayuda de un cmdlet no están instalados en el equipo, el cmdlet [Get-Help](assetId:///1f46eeb4-49d7-4bec-bb29-395d9b42f54a) muestra ahora la ayuda generada automáticamente. La ayuda generada automáticamente incluye la sintaxis e instrucciones de comandos para usar el cmdlet [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545) para descargar archivos de ayuda.

Cualquier autor de módulos puede incluir compatibilidad con la ayuda actualizable en su módulo. Puede incluir archivos de ayuda en el módulo y usar la ayuda actualizable para actualizarlos u omitir los archivos de ayuda y usar la ayuda actualizable para instalarlos. Para más información sobre cómo admitir la ayuda actualizable, consulte [Supporting Updatable Help](http://go.microsoft.com/FWLink/?LinkID=242129) (Compatibilidad con la ayuda actualizable) en MSDN.

### <a name="BKMK_Online"></a>Mejor ayuda en línea
La ayuda en línea de [!INCLUDE[mshshort](../Token/mshshort_md.md)] es un recurso muy valioso para todos los usuarios, pero es especialmente importante para los usuarios que no tienen instalado o no pueden instalar archivos de ayuda actualizados.

Para obtener ayuda en línea sobre cualquier cmdlet de [!INCLUDE[mshshort](../Token/mshshort_md.md)], escriba lo siguiente:

```
Get-Help <cmdlet-name> -Online
```

[!INCLUDE[mshshort](../Token/mshshort_md.md)] abre la versión en línea de un tema de ayuda en el explorador de Internet predeterminado.

Ahora, la característica **Get-Help -Online** de [!INCLUDE[psversion3](../Token/psversion3_md.md)] es más eficaz aún, puesto que funciona incluso cuando los archivos de ayuda del cmdlet no están instalados en el equipo. La característica **Get-Help -Online** obtiene el URI del tema de ayuda en línea de la propiedad HelpUri de los cmdlets y las funciones avanzadas.

```
PS C:\>(Get-Command Get-ScheduledJob).HelpUri
http://go.microsoft.com/fwlink/?LinkID=223923
```

A partir de [!INCLUDE[psversion3](../Token/psversion3_md.md)], los autores de cmdlets de C# pueden rellenar la propiedad **HelpUri** creando un atributo **HelpUri** en la clase del cmdlet. Los autores de funciones avanzadas pueden definir una propiedad **HelpUri** en el atributo **CmdletBinding**. El valor de la propiedad **HelpUri** debe empezar por "http" o "https".

También puede incluir un valor de **HelpUri** en el primer vínculo relacionado de un archivo de ayuda de cmdlet basado en XML o la directiva .Link de la ayuda basada en comentarios de una función.

Para más información sobre cómo admitir la ayuda en línea, consulte [Supporting Online Help](http://go.microsoft.com/fwlink/?LinkId=242132) (Compatibilidad con la ayuda en línea) en MSDN.

### <a name="BKMK_CIM"></a>Integración de CIM
[!INCLUDE[psversion3](../Token/psversion3_md.md)] incluye compatibilidad con el Modelo de información común (CIM), que proporciona definiciones comunes de información de administración de sistemas, redes, aplicaciones y servicios, lo que permite intercambiar información de administración entre sistemas heterogéneos. La compatibilidad con CIM de [!INCLUDE[psversion3](../Token/psversion3_md.md)] incluye la posibilidad de crear cmdlets de Windows PowerShell basados en clases CIM nuevas o existentes, comandos basados en archivos XML de definición de cmdlets, compatibilidad con la API de .NET Framework de CIM, cmdlets de administración de CIM y proveedores de WMI 2.0.

### <a name="BKMK_ConfigFile"></a>Archivos de configuración de sesión
A partir de [!INCLUDE[psversion3](../Token/psversion3_md.md)], puede diseñar una configuración de sesión personalizada con un archivo. Este nuevo archivo de configuración de sesión permite definir el entorno de sesiones en el que se usa la configuración de sesión, incluido los módulos, scripts y archivos de formato que se cargan en las sesiones, los cmdlets y elementos de lenguaje que pueden usar los usuarios, los módulos y scripts que pueden ejecutar y las variables que pueden ver.

Se puede diseñar una sesión en la que los usuarios solo pueden ejecutar los cmdlets de un determinado módulo, o una sesión en la que los usuarios tienen acceso completo a todos los módulos y acceso a scripts que realizan tareas avanzadas.

En versiones anteriores de [!INCLUDE[mshshort](../Token/mshshort_md.md)], un control a este nivel solo lo poseían aquellos que podían escribir un programa de C# o un script de inicio complejo. Ahora, cualquier miembro del grupo Administradores del equipo puede personalizar una configuración de sesión con un archivo de configuración.

Use el cmdlet [New-PSSessionConfigurationFile](assetId:///5f3e3633-6e90-479c-aea9-ba45a1954866) para crear un archivo de configuración de sesión. Use los cmdlets [Register-PSSessionConfiguration](assetId:///e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) o [Set-PSSessionConfiguration](assetId:///b21fbad3-1759-4260-b206-dcb8431cd6ea) para aplicar el archivo de configuración de sesión a una configuración de sesión.

Para más información, consulte [about_Session_Configuration_Files](assetId:///c7217447-1ebf-477b-a8ef-4dbe9a1473b8) y [New-PSSessionConfigurationFile](assetId:///5f3e3633-6e90-479c-aea9-ba45a1954866).

### <a name="BKMK_ScheduledJob"></a>Integración de trabajos programados y del Programador de tareas
Ahora se pueden programar trabajos en segundo plano de [!INCLUDE[mshshort](../Token/mshshort_md.md)] y administrarlos en [!INCLUDE[mshshort](../Token/mshshort_md.md)] y en el Programador de tareas.

Los trabajos programados de [!INCLUDE[mshshort](../Token/mshshort_md.md)] constituyen una mezcla muy práctica de trabajos en segundo plano de [!INCLUDE[mshshort](../Token/mshshort_md.md)] y tareas del Programador de tareas.

Como sucede con los trabajos en segundo plano de [!INCLUDE[mshshort](../Token/mshshort_md.md)], los trabajos programados se ejecutan de forma asincrónica en segundo plano. Las instancias de trabajos programados que se han completado se pueden administrar con cmdlets de trabajo como, por ejemplo, [Start-Job](assetId:///2bc04935-0deb-4ec0-b856-d7290cca6442) y [Get-Job](assetId:///1352c534-7193-46ca-9ab1-0c5219a661ad).

Al igual que las tareas del Programador de tareas, los trabajos programados se pueden ejecutar según una programación periódica o puntual, o en respuesta a una acción o un evento. Los trabajos programados se pueden ver y administrar en el Programador de tareas, habilitar y deshabilitar según sea necesario y ejecutar o usar como plantillas. También se pueden definir las condiciones en las que los trabajos de inician.

Además, los trabajos programados vienen con un conjunto personalizado de cmdlets para administrarlos. Los cmdlets permiten crear, editar, administrar, deshabilitar y volver a habilitar trabajos programados, crear desencadenadores de trabajos programados y establecer opciones de trabajo programado.

Para más información sobre los trabajos programados, consulte [about_Scheduled_Jobs](assetId:///3b546629-703c-4939-b44f-52dd567bce92).

### <a name="BKMK_Lang"></a>Mejoras en el lenguaje de Windows PowerShell
[!INCLUDE[psversion3](../Token/psversion3_md.md)] incluye muchas características destinadas a hacer que su lenguaje sea más sencillo y más fácil de usar, así como a evitar errores comunes. Entre estas mejoras están la enumeración de propiedades, las propiedades de recuento y longitud en objetos escalares, nuevos operadores de redirección, el modificador de ámbito $Using, la variable automática PSItem, un formato de scripts flexible, atributos de variables, argumentos de atributos simplificados, nombres de comandos numéricos, el operador Stop-Parsing, una mejor expansión de matriz, nuevos operadores de bits, diccionarios ordenados, conversión de PSCustomObject y una ayuda mejor basada en comentarios.

### <a name="BKMK_Core"></a>Nuevos cmdlets principales
Se han agregado nuevos cmdlets a la instalación principal de Windows PowerShell, incluidos cmdlets para administrar trabajos programados, sesiones desconectadas, la integración de CIM y el sistema de ayuda actualizable.

|||
|-|-|
|Add-JobTrigger|New-JobTrigger|
|Connect-PSSession|New-PSSessionConfigurationFile|
|ConvertFrom-Json|New-PSTransportOption|
|ConvertTo-Json|New-PSWorkflowExecutionOption|
|Disable-JobTrigger|New-PSWorkflowSession|
|Disable-ScheduledJob|New-ScheduledJobOption|
|Disconnect-PSSession|New-WinEvent|
|Enable-JobTrigger|Receive-PSSession|
|Enable-ScheduledJob|Register-CimIndicationEvent|
|Get-CimAssociatedInstance|Register-ScheduledJob|
|Get-CimClass|Remove-CimInstance|
|Get-CimInstance|Remove-CimSession|
|Get-CimSession|Remove-TypeData|
|Get-ControlPanelItem|Rename-Computer|
|Get-IseSnippet|Resume-Job|
|Get-JobTrigger|Save-Help|
|Get-ScheduledJob|Set-CimInstance|
|Get-ScheduledJobOption|Set-JobTrigger|
|Get-TypeData|Set-ScheduledJob|
|Import-IseSnippet|Set-ScheduledJobOption|
|Invoke-AsWorkflow|Show-Command|
|Invoke-CimMethod|Show-ControlPanelItem|
|Invoke-RestMethod|Suspend-Job|
|Invoke-WebRequest|Test-PSSessionConfigurationFile|
|New-CimInstance|Unblock-File|
|New-CimSession|Unregister-ScheduledJob|
|New-CimSessionOption|Update-Help|
|New-IseSnippet||

### <a name="BKMK_Prov"></a>Mejoras en los proveedores y cmdlets principales existentes
[!INCLUDE[psversion3](../Token/psversion3_md.md)] incluye nuevas características para los cmdlets existentes, incluida la sintaxis simplificada, y nuevos parámetros para los siguientes cmdlets: cmdlets de equipo, cmdlets de CSV, Get-ChildItem, Get-Command, Get-Content, Get-History, Measure-Object, cmdlets de seguridad, Select-Object, Select-String, Split-Path, Start-Process, Tee-Object, Test-Connection, Add-Member y cmdlets de WMI.

Los proveedores de Windows PowerShell también se han mejorado considerablemente, incluida la posibilidad del proveedor de certificados de administrar certificados de Capa de sockets seguros (SSL) para el hospedaje web, la compatibilidad con credenciales, unidades de red persistentes y flujos de datos alternativos en las unidades del sistema de archivos.

### <a name="BKMK_REM"></a>Detección e importación de módulos remotos
[!INCLUDE[psversion3](../Token/psversion3_md.md)] amplía las capacidades de detección de módulos, importación y comunicación remota implícita en los equipos remotos. Los cmdlets de módulo obtienen los módulos de equipos remotos y los importa al equipo local o remoto usando la comunicación remota de Windows PowerShell. La nueva compatibilidad con sesiones CIM permite usar CIM y WMI para administrar equipos sin Windows importando al equipo local comandos que se ejecutan de forma implícita en el equipo remoto.

Para obtener más información, vea los temas de ayuda de los cmdlets [Get-Module](assetId:///2cccd4c4-9a21-4c77-b691-984ee57242e1) e [Import-Module](assetId:///af616c24-e122-4098-930e-1e3ea2080ade).

### <a name="BKMK_TAB"></a>Mejor finalización con tabulación
Ahora, la finalización con tabulación en la consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)] completa los nombres de los cmdlets, parámetros, valores de parámetro, enumeraciones, tipos de .NET Framework, objetos COM, directorios ocultos y otros muchos elementos. La característica de finalización con tabulación se ha reescrito completamente según un analizador y árbol de sintaxis abstracta nuevos para dar cabida a más escenarios, incluidos los árboles de análisis en memoria y la finalización con tabulación de línea media.

### <a name="BKMK_AutoLoad"></a>Carga automática de módulos
Ahora, el cmdlet [Get-Command](assetId:///59c6d302-6e8c-48b7-a6f6-f0172df936ad) obtiene todas las funciones y cmdlets de todos los módulos instalados en el equipo, aun cuando el módulo no se haya importado a la sesión actual.

Cuando obtenga el cmdlet que necesita, puede usarlo inmediatamente sin importar módulos. Ahora, los módulos de [!INCLUDE[mshshort](../Token/mshshort_md.md)] se importan automáticamente cuando se usa un cmdlet en el módulo. Por lo tanto, ya no es necesario buscar el módulo e importarlo para usar sus cmdlets.

La importación automática de módulos se desencadena al usar el cmdlet en un comando, al ejecutar **Get-Command** para un cmdlet sin comodines o al ejecutar [Get-Help](assetId:///1f46eeb4-49d7-4bec-bb29-395d9b42f54a) para un cmdlet sin comodines.

Para habilitar, deshabilitar y configurar la importación automática de módulos, use la variable de preferencia **$PSModuleAutoLoadingPreference**.

Para obtener más información, consulte [about_Modules [v4]](assetId:///94f57429-a539-4aee-bb0d-205cd7e801f9), [about_Preference_Variables [v4]](assetId:///31344314-be29-4286-b039-afa5460cbe8b) y los temas de ayuda de los cmdlets [Get-Command](assetId:///59c6d302-6e8c-48b7-a6f6-f0172df936ad) e [Import-Module](assetId:///af616c24-e122-4098-930e-1e3ea2080ade).

### <a name="BKMK_MOD"></a>Mejoras en la experiencia de módulo
Windows PowerShell 3.0 incorpora compatibilidad de características avanzadas en los módulos, incluidas las siguientes características nuevas:

1.  Registro de módulos para módulos individuales (LogPipelineExecutionDetails) y la nueva configuración de directiva de grupo "Activar registro de módulos".

2.  Objetos de módulo extendidos que exponen los valores del manifiesto del módulo.

3.  Nueva propiedad **ExportedCommands** de los módulos (incluidos los módulos anidados), que combina comandos de todos los tipos.

4.  Mejor detección de los módulos (no importados) disponibles, incluido permitir los parámetros **Path** y **ListAvailable** en el mismo comando.

5.  Nueva clave **DefaultCommandPrefix** en los manifiestos del módulo, que evita conflictos de nombres sin tener que cambiar el código del módulo.

6.  Mejora de los requisitos de módulo, incluidos los módulos necesarios completos con la versión y el GUID y la importación automática de los módulos necesarios.

7.  Funcionamiento optimizado y más suave del cmdlet [New-ModuleManifest](assetId:///512adced-f42f-4e88-ba7c-834fc9e5d047).

8.  Nuevo parámetro **Module** para #Requires.

9. Cmdlet [Import-Module](assetId:///af616c24-e122-4098-930e-1e3ea2080ade) mejorado con los parámetros **MinimumVersion** y **RequiredVersion**.

### <a name="BKMK_SIMPLE"></a>Detección de comandos simplificada
Ya no necesitará importar todos los módulos para detectar los comandos disponibles en la sesión. En [!INCLUDE[psversion3](../Token/psversion3_md.md)], el cmdlet [Get-Command](assetId:///59c6d302-6e8c-48b7-a6f6-f0172df936ad) obtiene todos los comandos de todos los módulos instalados. Además, si usa un comando, el módulo que exporta el comando se importa automáticamente en la sesión.

El nuevo cmdlet [Show-Command](assetId:///65bba50b-91a8-49d5-80a2-a30fc684ba41) está diseñado específicamente para principiantes. Puede buscar comandos en una ventana. Puede ver todos los comandos o filtrar por módulo, importar un módulo haciendo clic en un botón o usar cuadros de texto y listas desplegables para construir un comando válido y, a continuación, copiarlo o ejecutarlo sin salir de la ventana.

### <a name="BKMK_LOG"></a>Mejor compatibilidad con jregistros, diagnósticos y directivas de grupo
[!INCLUDE[psversion3](../Token/psversion3_md.md)] mejora la compatibilidad del registro y seguimiento de comandos y módulos al admitir registros de Seguimiento de eventos para Windows (ETW), un propiedad **LogPipelineExecutionDetails** editable de los módulos y la configuración de la directiva de grupo "Activar registro de módulos". Ahora puede obtener valores de parámetro de los detalles de registro mostrando las propiedades del registro.

### <a name="BKMK_OUT"></a>Mejoras de formato y salida
Las nuevas mejoras de formato y salida disparan la eficacia de todos los usuarios de [!INCLUDE[wps_2](../Token/wps_2_md.md)]. Estas mejoras incluyen la redirección de salida de todas las secuencias, un cmdlet Update-Type mejorado que agrega tipos dinámicamente sin archivos Format.ps1xml, ajuste de líneas en la salida, propiedades de formato predeterminado de objetos personalizados, el tipo **PSCustomObject**, un mejor formato de los objetos WMI y objetos heterogéneos y compatibilidad para detectar sobrecargas de método.

### <a name="BKMK_HOST"></a>Mejor experiencia de host de consola
El programa host de la consola de [!INCLUDE[wps_2](../Token/wps_2_md.md)] presenta nuevas características en [!INCLUDE[psversion3](../Token/psversion3_md.md)], como el contenedor uniproceso, de forma predeterminada. La nueva opción "Ejecutar con PowerShell" del Explorador de archivos permite ejecutar scripts en una sesión sin restricciones, simplemente haciendo clic con el botón derecho. La nueva lógica de inicio del host de consola inicia Windows PowerShell con mayor rapidez, mientras que las nuevas fuentes permiten personalizar la experiencia de ventana de consola ya conocida.

Para más información, consulte [about_Run_With_PowerShell](assetId:///c9d9ca5f-eff9-4409-be9d-e43b5b4087eb).

### <a name="BKMK_API"></a>Nuevas API de cmdlets y de hospedaje
Las nuevas API de cmdlet y API de hospedaje incluyen API públicas de árbol de sintaxis avanzada (AST) y API para la paginación de canalizaciones, canalizaciones anidadas, finalización con tabulación de grupos de espacio de ejecución, Windows RT, el atributo de cmdlet Obsolete y las propiedades Verb y Noun del objeto FunctionInfo.

### <a name="BKMK_PERF"></a>Mejoras en el rendimiento
Las notables mejoras de rendimiento de Windows PowerShell se deben al nuevo analizador de lenguaje, que se basa en Dynamic Runtime Language (DLR) de .NET Framework 4, además de a la compilación de scripts en tiempo de ejecución, a mejoras en la confiabilidad del motor y a los cambios en el algoritmo de [Get-ChildItem](assetId:///75cf79bb-4db6-4a67-8c36-3d20754e2190) que mejoran su rendimiento, especialmente al buscar recursos de compartidos de red.

### <a name="BKMK_RUNAS"></a>Compatibilidad con RunAs y host compartido
[!INCLUDE[psversion3](../Token/psversion3_md.md)] incluye compatibilidad con las características RunAs y de host compartido.

La característica *RunAs*, diseñada para el flujo de trabajo de Windows PowerShell, permite a los usuarios de una configuración de sesión crear sesiones que se ejecutan con el permiso de una cuenta de usuario compartida. Esto permite a los usuarios con menos privilegios ejecutar determinados comandos y scripts con permisos de administrador, al tiempo que reduce la necesidad de agregar usuarios con menos experiencia al grupo Administradores.

La característica **SharedHost** permite que varios usuarios en diferentes equipos puedan conectarse a una sesión de flujo de trabajo simultáneamente y supervisar el progreso de un flujo de trabajo. Los usuarios pueden iniciar un flujo de trabajo en un equipo y, a continuación, conectarse a la sesión de flujo de trabajo en otro equipo sin desconectar la sesión desde el equipo original. Los usuarios deben tener los mismos permisos y usar la misma configuración de sesión. Para más información, consulte "Ejecutar un flujo de trabajo de Windows PowerShell" en Introducción al flujo de trabajo de Windows PowerShell.

### <a name="BKMK_CHAR"></a>Mejoras en el tratamiento de caracteres especiales
A fin de mejorar la capacidad de [!INCLUDE[psversion3](../Token/psversion3_md.md)] de interpretar y tratar correctamente los caracteres especiales, el parámetro **LiteralPath** (que trata los caracteres especiales en las rutas de acceso) es válido en prácticamente todos los cmdlets que tengan el parámetro **Path**, como los nuevos cmdlets [Update-Help](assetId:///93e1d870-ace6-432b-8778-8920291d7545) y [Save-Help](assetId:///aed94f90-b73f-4e25-a25d-7c18d9f161fa). El analizador incluye también una lógica especial para mejorar el tratamiento del carácter de acento grave (`) y los corchetes en los nombres y rutas de acceso de archivo.

## Consulte también
[about_Windows_PowerShell_4.0](http://technet.microsoft.com/en-us/library/hh847833(v=wps.630).aspx)
[about_Windows_PowerShell_5.0](assetId:///6d56fa88-371e-40c9-b2de-64a2a0cd49da)
[Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=107116)



<!--HONumber=Apr16_HO1-->


