---
title: Aprendizaje de los nombres de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b4d0fd22-8298-4ee6-82ae-9b6f2907c986
---
# Aprendizaje de los nombres de Windows PowerShell
El aprendizaje de nombres de comandos y parámetros de comando supone una inversión de tiempo considerable con la mayoría de las interfaces de línea de comandos. El problema es que hay muy pocos patrones, por lo que es la única manera de aprender consiste en memorizar cada comando y cada parámetro que necesite usar de forma regular.

Cuando trabaja con un nuevo comando o parámetro, por lo general no puede usar lo que ya conoce; tendrá que buscar y aprender un nuevo nombre. Si observa cómo crecen las interfaces de un pequeño conjunto de herramientas con las adiciones incrementales de funcionalidad, le resultará fácil ver por qué la estructura no es estándar. Con los nombres de comando especialmente, esto puede parecer lógico, ya que cada comando es una herramienta independiente, pero hay una manera mejor de gestionar los nombres de comando.

La mayoría de los comandos se crea para administrar elementos del sistema operativo o las aplicaciones, como los servicios o los procesos. Los comandos tienen una gran variedad de nombres que pueden o no encajar en una familia. Por ejemplo, en sistemas Windows, puede usar los comandos **net start** y **net stop** para iniciar y detener un servicio. Hay otra herramienta de control de servicio más generalizada para Windows que tiene un nombre completamente distinto, **sc**, que no encaja en el patrón de nomenclatura para los comandos de servicio **net**. Para la administración del proceso, Windows tiene el comando **tasklist** para enumerar los procesos y el comando **taskkill** para eliminar los procesos.

Los comandos que incluyen parámetros tienen especificaciones de parámetro irregulares. No se puede usar el comando **net start** para iniciar un servicio en un equipo remoto. El comando **sc** iniciará un servicio en un equipo remoto, pero para especificar el equipo remoto, se debe anteponer una barra diagonal inversa doble a su nombre. Por ejemplo, para iniciar el servicio de administrador de trabajos en cola en un equipo remoto denominado DC01, debe escribir **sc \\\\DC01 start spooler**. Para enumerar las tareas que se ejecutan en DC01, debe usar el parámetro **\/S** (para "system") y proporcionar el nombre DC01 sin barras diagonales inversas: **tasklist \/S DC01**.

Aunque existen importantes diferencias técnicas entre un servicio y un proceso, ambos son ejemplos de elementos fáciles de administrar en un equipo con un ciclo de vida bien definido. Es posible que quiera iniciar o detener un servicio o un proceso, u obtener una lista de todos los servicios o procesos que se están ejecutando actualmente. En otras palabras, aunque un servicio y un proceso son cosas diferentes, las acciones que realizamos en un servicio o un proceso suelen ser conceptualmente iguales. Además, las selecciones que realizamos para personalizar una acción mediante la especificación de parámetros también pueden ser conceptualmente similares.

Windows PowerShell aprovecha estas similitudes para reducir el número de nombres distintos que debe saber para entender y usar los cmdlets.

### Los cmdlets usan nombres verbo-sustantivo para reducir la memorización de comandos
Windows PowerShell usa un sistema de nomenclatura "verbo-sustantivo", donde cada nombre de cmdlet consta de un verbo estándar seguido de un guion y un sustantivo específico. Los verbos de Windows PowerShell no son siempre verbos ingleses, pero expresan acciones específicas en Windows PowerShell. Los sustantivos son muy similares a los de cualquier idioma, describen tipos específicos de objetos que son importantes para la administración del sistema. Es fácil demostrar cómo estos nombres de dos componentes reducen el esfuerzo de aprendizaje examinando algunos ejemplos de verbos y sustantivos.

Los sustantivos tienen menos restricciones, pero siempre que deben describir sobre qué elemento actúa un comando. Windows PowerShell presenta comandos, como **Get-Process**, **Stop-Process**, **Get-Service** y **Stop-Service**.

En el caso de dos sustantivos y dos verbos, la coherencia no simplifica tanto el aprendizaje. Sin embargo, si observa un conjunto estándar de 10 verbos y 10 sustantivos, solo tiene que comprender 20 términos, aunque estos se pueden usar para formar hasta 100 nombres de comando distintos.

Con frecuencia, puede reconocer lo que hace un comando mediante la lectura de su nombre. También suele ser bastante evidente el nombre que debe usarse para un comando nuevo. Por ejemplo, un comando para apagar el equipo podría ser **Stop-Computer**. Una comando para enumerar todos los equipos en una red podría ser **Get-Computer**. El comando que obtiene la fecha del sistema es **Get-Date**.

Puede enumerar todos los comandos que incluyan un verbo determinado con el parámetro **-Verb** para **Get-Command** (hablaremos de **Get-Command** con más detalle en la siguiente sección). Por ejemplo, para ver todos los cmdlets que usan el verbo **Get**, escriba:

```
PS> Get-Command -Verb Get
CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Get-Acl                         Get-Acl [[-Path] <String[]>]...
Cmdlet          Get-Alias                       Get-Alias [[-Name] <String[]...
Cmdlet          Get-AuthenticodeSignature       Get-AuthenticodeSignature [-...
Cmdlet          Get-ChildItem                   Get-ChildItem [[-Path] <Stri...
...
```

El parámetro **-Noun** es aún más útil, porque permite ver una familia de comandos que influyen en el mismo tipo de objeto. Por ejemplo, si desea ver qué comandos están disponibles para administrar los servicios, escriba el siguiente comando:

```
PS> Get-Command -Noun Service
CommandType     Name                            Definition
-----------     ----                            ----------
Cmdlet          Get-Service                     Get-Service [[-Name] <String...
Cmdlet          New-Service                     New-Service [-Name] <String>...
Cmdlet          Restart-Service                 Restart-Service [-Name] <Str...
Cmdlet          Resume-Service                  Resume-Service [-Name] <Stri...
Cmdlet          Set-Service                     Set-Service [-Name] <String>...
Cmdlet          Start-Service                   Start-Service [-Name] <Strin...
Cmdlet          Stop-Service                    Stop-Service [-Name] <String...
Cmdlet          Suspend-Service                 Suspend-Service [-Name] <Str... 
...
```

Un comando no es necesariamente un cmdlet, simplemente porque tiene un esquema de nomenclatura de verbo-sustantivo. Un ejemplo de comando de Windows PowerShell nativo que no es un cmdlet, pero tiene un nombre verbo-sustantivo, es el comando para borrar una ventana de la consola, Clear-Host. El comando Clear-Host es en realidad una función interna, como puede ver si ejecuta Get-Command en él:

```
PS> Get-Command -Name Clear-Host

CommandType     Name                            Definition
-----------     ----                            ----------
Function        Clear-Host                      $spaceType = [System.Managem...
```

### Los cmdlets usan parámetros estándar
Como hemos indicado anteriormente, los comandos usados en las interfaces de línea de comandos tradicionales no suelen tener nombres de parámetro coherentes. En ocasiones, los parámetros no tienen ningún nombre. Si lo tienen, suele tener un único carácter o una palabra abreviada que se pueda escribir rápidamente, pero que los nuevos usuarios no puedan comprender fácilmente.

A diferencia de la mayoría de interfaces de línea de comandos tradicionales, Windows PowerShell procesa los parámetros directamente y usa este acceso directo a los parámetros junto con instrucciones para desarrolladores para estandarizar los nombres de parámetro. Aunque esto no garantiza que todos los cmdlets cumplan siempre con los estándares, es un procedimiento recomendado.

> [!NOTE]
> Los nombres de parámetro siempre tienen un "-" antepuesto cuando los usa, para permitir que Windows PowerShell los identifique claramente como parámetros. En el ejemplo **Get-Command -Name Clear-Host**, el nombre del parámetro es **Name**, pero se escribe como -**Name**.

Estas son algunas de las características generales de los nombres y usos de los parámetros estándar.

#### Parámetro de ayuda (?)
Si especifica el parámetro **-?** para cualquier cmdlet, el cmdlet no se ejecuta. En su lugar, Windows PowerShell muestra la ayuda del cmdlet.

#### Parámetros comunes
Windows PowerShell presenta varios parámetros que se conocen como *parámetros comunes*. Dado que estos parámetros los controla el motor de Windows PowerShell, se comportarán del mismo modo siempre que cmdlet los implemente. Los parámetros comunes son **WhatIf**, **Confirm**, **Verbose**, **Debug**, **Warn**, **ErrorAction**, **ErrorVariable**, **OutVariable** y **OutBuffer**.

#### Parámetros sugeridos
Los cmdlets principales de Windows PowerShell usan nombres estándar para parámetros similares. Aunque no se exige el uso de nombres de parámetro, existen un instrucciones explícitas para usarlos a fin de promover la estandarización.

Por ejemplo, las instrucciones recomiendan denominar un parámetro que haga referencia a un equipo por su nombre, como **nombreDeEquipo**, en lugar de servidor, host, sistema, nodo u otras palabras alternativas comunes. Entre los nombres de parámetros importantes sugeridos se encuentran **Force**, **Exclude**, **Include**, **PassThru**, **Path** y **CaseSensitive**.



<!--HONumber=Apr16_HO1-->


