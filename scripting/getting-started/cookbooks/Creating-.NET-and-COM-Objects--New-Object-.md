---
title: Crear objetos .NET y COM (New-Object)
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 2057b113-efeb-465e-8b44-da2f20dbf603
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: e986e85a5d7416d8deaec06c0784263ee09fc9ff

---

# Crear objetos .NET y COM (New-Object)
Existen componentes de software con interfaces de .NET Framework y COM que permiten realizar muchas tareas de administración del sistema. Windows PowerShell le permite usar estos componentes, por lo que no está limitado a las tareas que pueden realizarse mediante cmdlets. Muchos de los cmdlets de la versión inicial de Windows PowerShell no funcionan en equipos remotos. Demostraremos cómo superar esta limitación al administrar registros de eventos mediante el uso de la clase **System.Diagnostics.EventLog** de .NET Framework directamente desde Windows PowerShell.

### Uso de New\-Object para el acceso del registro de eventos
La biblioteca de clases de .NET Framework incluye una clase denominada **System.Diagnostics.EventLog** que se puede usar para administrar registros de eventos. Puede crear una nueva instancia de una clase .NET Framework mediante el cmdlet **New\-Object** con el parámetro **TypeName**. Por ejemplo, el comando siguiente crea una referencia de registro de eventos:

```
PS> New-Object -TypeName System.Diagnostics.EventLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
```

Aunque el comando creó una instancia de la clase EventLog, la instancia no incluye ningún dato. Eso es porque no especificamos un registro de eventos concreto. ¿Cómo se consigue un registro de eventos real?

#### Uso de constructores con New\-Object
Para hacer referencia a un registro de eventos específico, debe especificar el nombre del registro. **New\-Object** tiene un parámetro **ArgumentList**. Los argumentos que se pasan como valores a este parámetro se usan en un método de inicio especial del objeto. El método se llama *constructor* porque se usa para construir el objeto. Por ejemplo, para obtener una referencia al registro de aplicaciones, especifique la cadena "Application" como argumento:

```
PS> New-Object -TypeName System.Diagnostics.EventLog -ArgumentList Application

Max(K) Retain OverflowAction        Entries Name
------ ------ --------------        ------- ----
16,384      7 OverwriteOlder          2,160 Application
```

> [!NOTE]
> Puesto que la mayoría de las clases principales de .NET Framework se incluyen en el espacio de nombres del sistema, Windows PowerShell intentará buscar clases que especifique en el espacio de nombres del sistema si no encuentra a una coincidencia para el nombre de tipo especificado. Esto significa que puede especificar Diagnostics.EventLog en lugar de System.Diagnostics.EventLog.

#### Almacenar objetos en Variables
Es posible que desee almacenar una referencia a un objeto, para poder usarla en el shell actual. Aunque Windows PowerShell permite realizar muchas tareas con las canalizaciones, lo que reduce la necesidad de variables, almacenar referencias a objetos en variables puede facilitar la manipulación de esos objetos en determinadas ocasiones.

Windows PowerShell le permite crear variables que básicamente se denominan objetos. La salida de cualquier comando válido de Windows PowerShell puede almacenarse en una variable. Los nombres de variable siempre comienzan por $. Si desea almacenar la referencia del registro de aplicaciones en una variable denominada $AppLog, escriba el nombre de la variable, seguido de un signo igual y, a continuación, escriba el comando usado para crear el objeto de registro de aplicaciones:

```
PS> $AppLog = New-Object -TypeName System.Diagnostics.EventLog -ArgumentList Application
```

Si luego escribe $AppLog, puede ver que contiene el registro de aplicaciones:

```
PS> $AppLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
  16,384      7 OverwriteOlder          2,160 Application
```

#### Acceso al registro de eventos remoto con New\-Object
Los comandos usados en la sección anterior van dirigidos al equipo local; el cmdlet **Get\-EventLog** puede hacerlo. Para acceder al registro de aplicaciones en un equipo remoto, debe proporcionar el nombre del registro y un nombre de equipo (o dirección IP) como argumentos.

```
PS> $RemoteAppLog = New-Object -TypeName System.Diagnostics.EventLog Application,192.168.1.81
PS> $RemoteAppLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
     512      7 OverwriteOlder            262 Application
```

Ahora que tenemos una referencia a un registro de eventos almacenado en la variable $RemoteAppLog, ¿qué tareas podemos realizar en él?

#### Borrar un registro de eventos con los métodos de objeto
Los objetos suelen tener métodos que se puedan llamar para realizar tareas. **Get\-Member** se puede usar para mostrar los métodos asociados a un objeto. El siguiente comando y la salida seleccionada muestran algunos de los métodos de la clase EventLog:

```
PS> $RemoteAppLog | Get-Member -MemberType Method

   TypeName: System.Diagnostics.EventLog

Name                      MemberType Definition
----                      ---------- ----------
...
Clear                     Method     System.Void Clear()
Close                     Method     System.Void Close()
...
GetType                   Method     System.Type GetType()
...
ModifyOverflowPolicy      Method     System.Void ModifyOverflowPolicy(Overfl...
RegisterDisplayName       Method     System.Void RegisterDisplayName(String ...
...
ToString                  Method     System.String ToString()
WriteEntry                Method     System.Void WriteEntry(String message),...
WriteEvent                Method     System.Void WriteEvent(EventInstance in...
```

El método **Clear()** se puede usar para borrar el registro de eventos. Al llamar a un método, siempre debe seguir el nombre del método con paréntesis, aunque este no requiera argumentos. Esto permite que Windows PowerShell distinga entre el método y una propiedad potencial con el mismo nombre. Escriba lo siguiente para llamar al método **Clear**:

```
PS> $RemoteAppLog.Clear()
```

Escriba lo siguiente para mostrar el registro. Verá que se ha borrado el registro de eventos y ahora tiene 0 entradas en lugar de 262:

```
PS> $RemoteAppLog

  Max(K) Retain OverflowAction        Entries Name
  ------ ------ --------------        ------- ----
     512      7 OverwriteOlder              0 Application
```

### Creación de objetos COM con New\-Object
Puede usar **New\-Object** para trabajar con componentes del Modelo de objetos componentes (COM). Los componentes van desde las distintas bibliotecas incluidas con Windows Script Host (WSH) hasta las aplicaciones de ActiveX, como Internet Explorer, que están instaladas en la mayoría de los sistemas.

**New\-Object** usa contenedores R\-CW de .NET Framework para crear objetos COM, por lo que tiene las mismas limitaciones que .NET Framework al llamar a objetos COM. Para crear un objeto COM, debe especificar el parámetro **ComObject** con el identificador de programación o *ProgID* de la clase COM que quiere usar. Una explicación completa de las limitaciones del uso de COM y la indicación de qué ProgID están disponibles en un sistema está fuera del ámbito de este manual, pero la mayoría de los objetos conocidos de entornos como WSH pueden usarse en Windows PowerShell.

Puede crear los objetos WSH especificando estos ProgID: **WScript.Shell**, **WScript.Network**, **Scripting.Dictionary** y **Scripting.FileSystemObject**. Los siguientes comandos crean estos objetos:

```
New-Object -ComObject WScript.Shell
New-Object -ComObject WScript.Network
New-Object -ComObject Scripting.Dictionary
New-Object -ComObject Scripting.FileSystemObject
```

Aunque la mayor parte de la funcionalidad de estas clases está disponible de otras maneras en Windows PowerShell, algunas tareas como la creación de accesos directos son aún más fáciles con las clases de WSH.

### Crear un acceso directo de escritorio con WScript.Shell
Una tarea que se puede realizar rápidamente con un objeto COM es crear un acceso directo. Supongamos que desea crear un acceso directo en el escritorio que se vincule a la carpeta principal de Windows PowerShell. Primero debe crear una referencia a **WScript.Shell**, que se almacenará en una variable denominada **$WshShell**:

```
$WshShell = New-Object -ComObject WScript.Shell
```

Get\-Member funciona con objetos COM, por lo que es posible escribir lo siguiente para explorar los miembros del objeto:

```
PS> $WshShell | Get-Member

   TypeName: System.__ComObject#{41904400-be18-11d3-a28b-00104bd35090}

Name                     MemberType            Definition
----                     ----------            ----------
AppActivate              Method                bool AppActivate (Variant, Va...
CreateShortcut           Method                IDispatch CreateShortcut (str...
...
```

**Get\-Member** tiene un parámetro **InputObject** opcional que puede usar, en lugar de las canalizaciones, para proporcionar datos de entrada a **Get\-Member**. Obtendría la misma salida que se ha mostrado si usara el comando **Get\-Member \-InputObject $WshShell**. Si usa **InputObject**, trata su argumento como un solo elemento. Esto significa que si hay varios objetos en una variable, **Get\-Member** los trata como una matriz de objetos. Por ejemplo:

```
PS> $a = 1,2,"three"
PS> Get-Member -InputObject $a
TypeName: System.Object[]
Name               MemberType    Definition
----               ----------    ----------
Count              AliasProperty Count = Length
...
```

El método **WScript.Shell CreateShortcut** acepta un solo argumento, la ruta de acceso al archivo de acceso directo que se va a crear. Podríamos escribir la ruta de acceso completa al escritorio, pero hay una manera más fácil. El escritorio suele representarse con una carpeta denominada Desktop dentro de la carpeta principal del usuario actual. Windows PowerShell tiene una variable **$Home** que contiene la ruta de acceso a esta carpeta. Podemos especificar la ruta de acceso a la carpeta principal mediante esta variable y, después, agregar el nombre de la carpeta Desktop y el nombre del acceso directo que crearemos escribiéndolo:

```
$lnk = $WshShell.CreateShortcut("$Home\Desktop\PSHome.lnk")
```

Si se usa algo parecido a un nombre de variable entre comillas dobles, Windows PowerShell intenta realizar la sustitución por un valor coincidente. Si se usan comillas simples, Windows PowerShell no intenta sustituir el valor de la variable. Por ejemplo, intente escribir los siguientes comandos:

```
PS> "$Home\Desktop\PSHome.lnk"
C:\Documents and Settings\aka\Desktop\PSHome.lnk
PS> '$Home\Desktop\PSHome.lnk'
$Home\Desktop\PSHome.lnk
```

Ahora tenemos una variable denominada **$lnk** que contiene una nueva referencia de acceso directo. Si quiere ver sus miembros, puede canalizarla a **Get\-Member**. La salida siguiente muestra los miembros que debemos usar para terminar de crear el acceso directo:

<pre>PS> $lnk | Get-Member TypeName: System.__ComObject#{f935dc23-1cf0-11d0-adb9-00c04fd58a0b} Name             MemberType   Definition ----             ----------   ---------- ... Save             Method       void Save () ... TargetPath       Property     string TargetPath () {get} {set} ...</pre>

Es preciso especificar **TargetPath**, que es la carpeta de aplicación de Windows PowerShell y, luego, guardar el acceso directo **$lnk** llamando al método **Save**. La ruta de acceso de la carpeta de la aplicación de Windows PowerShell se almacena en la variable **$PSHome**, por lo que podemos escribir lo siguiente para hacerlo:

<pre>$lnk.TargetPath = $PSHome $lnk.Save()</pre>

### Usar Internet Explorer desde Windows PowerShell
Muchas aplicaciones (incluida la familia de aplicaciones de Microsoft Office e Internet Explorer) pueden automatizarse mediante COM. Internet Explorer muestra algunas de las técnicas y problemas típicos que implica trabajar con aplicaciones basadas en COM.

Una instancia de Internet Explorer se crea especificando el ProgID de Internet Explorer, **InternetExplorer.Application**:

```
$ie = New-Object -ComObject InternetExplorer.Application
```

Este comando inicia Internet Explorer, pero no hace que sea visible. Si escribe Get\-Process, puede ver que se está ejecutando un proceso denominado iexplore. De hecho, si sale de Windows PowerShell, el proceso se seguirá ejecutando. Debe reiniciar el equipo o usar una herramienta como el Administrador de tareas para finalizar el proceso de iexplore.

> [!NOTE]
> Los objetos COM que se inician como procesos independientes, denominados normalmente *ejecutables de ActiveX*, pueden mostrar o no una ventana de interfaz de usuario al iniciarse. Si crean una ventana, pero no la hacen visible, como Internet Explorer, el enfoque se moverá generalmente al escritorio de Windows y debe hacer que la ventana sea visible para interactuar con ella.

Si escribe **$ie | Get-Member\-, podrá ver las propiedades y los métodos de Internet Explorer. Para ver la ventana de Internet Explorer, establezca la propiedad Visible en $true escribiendo:

```
$ie.Visible = $true
```

A continuación, puede navegar a una dirección web específica mediante el método Navigate:

```
$ie.Navigate("http://www.microsoft.com/technet/scriptcenter/default.mspx")
```

Con otros miembros del modelo de objetos de Internet Explorer, es posible recuperar el contenido de texto de la página web. El siguiente comando mostrará el texto HTML en el cuerpo de la página web actual:

```
$ie.Document.Body.InnerText
```

Para cerrar Internet Explorer desde PowerShell, llame a su método Quit():

```
$ie.Quit()
```

Se forzará el cierre. La variable $ie ya no contiene una referencia válida, aunque parece ser un objeto COM. Si intenta usarla, obtendrá un error de automatización:

```
PS> $ie | Get-Member
Get-Member : Exception retrieving the string representation for property "Appli
cation" : "The object invoked has disconnected from its clients. (Exception fro
m HRESULT: 0x80010108 (RPC_E_DISCONNECTED))"
At line:1 char:16
+ $ie | Get-Member <<<<
```

Puede quitar la referencia restante con un comando como $ie \= $null, o bien escribir lo siguiente para quitarla completamente:

```
Remove-Variable ie
```

> [!NOTE]
> No hay un estándar común para determinar si los ejecutables de ActiveX se cierran o se siguen ejecutando cuando se quita una referencia a uno de ellos. Que la aplicación se cierre o no dependerá de las circunstancias, como, por ejemplo, si la aplicación es visible, si está ejecutando en ella un documento editado e incluso si Windows PowerShell todavía se está ejecutando. Por este motivo, debe probar el comportamiento de finalización de cada ejecutable de ActiveX que quiere usar en Windows PowerShell.

### Obtener advertencias sobre los objetos COM ajustados por .NET Framework
En algunos casos, un objeto COM puede tener un *contenedor R\-CW* de .NET Framework asociado, y lo usará **New\-Object**. Dado que el comportamiento del contenedor RCW puede ser diferente del comportamiento del objeto COM normal, **New\-Object** tiene un parámetro **Strict** para advertirle del acceso de dicho contenedor. Si especifica el parámetro **Strict** y, a continuación, crea un objeto COM que usa un RCW, recibirá un mensaje de advertencia:

```
PS> $xl = New-Object -ComObject Excel.Application -Strict
New-Object : The object written to the pipeline is an instance of the type "Mic
rosoft.Office.Interop.Excel.ApplicationClass" from the component's primary inte
rop assembly. If this type exposes different members than the IDispatch members
, scripts written to work with this object might not work if the primary intero
p assembly is not installed.
At line:1 char:17
+ $xl = New-Object  <<<< -ComObject Excel.Application -Strict
```

Aunque el objeto se creará de todos modos, se le advertirá que no es un objeto COM estándar.




<!--HONumber=Jun16_HO4-->


