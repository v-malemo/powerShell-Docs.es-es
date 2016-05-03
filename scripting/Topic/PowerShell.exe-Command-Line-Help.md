---
title: Ayuda de línea de comandos de PowerShell.exe
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1ab7b93b-6785-42c6-a1c9-35ff686a958f
---
# Ayuda de línea de comandos de PowerShell.exe
Inicia una sesión de Windows PowerShell. Puede usar PowerShell.exe para iniciar una sesión de Windows PowerShell desde la línea de comandos de otra herramienta, como Cmd.exe, o usarlo en la línea de comandos de Windows PowerShell para iniciar una nueva sesión. Use los parámetros para personalizar la sesión.

## Sintaxis

```
PowerShell[.exe]
       [-EncodedCommand <Base64EncodedCommand>]
       [-ExecutionPolicy <ExecutionPolicy>]
       [-InputFormat {Text | XML}] 
       [-Mta]
       [-NoExit]
       [-NoLogo]
       [-NonInteractive] 
       [-NoProfile] 
       [-OutputFormat {Text | XML}] 
       [-PSConsoleFile <FilePath> | -Version <Windows PowerShell version>]
       [-Sta]
       [-WindowStyle <style>]
       [-File <FilePath> [<Args>]]
       [-Command { - | <script-block> [-args <arg-array>]
                     | <string> [<CommandParameters>] } ]

PowerShell[.exe] -Help | -? | /?
```

## Parámetros

### -EncodedCommand <Base64EncodedCommand>
Acepta una versión de cadena codificada en base 64 de un comando. Use este parámetro para enviar comandos a Windows PowerShell que requieran comillas complejas o llaves.

### -ExecutionPolicy <ExecutionPolicy>
Establece la directiva de ejecución predeterminada para la sesión actual y la guarda en la variable de entorno $env:PSExecutionPolicyPreference. Este parámetro no modifica la directiva de ejecución de Windows PowerShell establecida en el Registro. Para más información sobre las directivas de ejecución de Windows PowerShell, incluida una lista de los valores válidos, consulte about_Execution_Policies (http://go.microsoft.com/fwlink/?LinkID=135170).

### -File <FilePath> [<Parameters>]
Ejecuta el script especificado en el ámbito local ("origen de puntos"), para que las funciones y variables que crea el script estén disponibles en la sesión actual. Escriba la ruta de acceso del archivo de script y los parámetros. **File** debe ser el último parámetro del comando, puesto que todos los caracteres escritos después del nombre de parámetro **File** se interpretan como la ruta de acceso de archivo de script seguido de los parámetros de los parámetros del script y sus valores.

Puede incluir los parámetros de un script, así como los valores de parámetro, en el valor del parámetro **File**. Por ejemplo: `-File .\Get-Script.ps1 -Domain Central`

Normalmente, los parámetros de modificador de un script se incluyen o se omiten. Por ejemplo, el siguiente comando usa el parámetro **All** del archivo de script Get-Script.ps1: `-File .\Get-Script.ps1 -All`

En raras ocasiones, es posible que deba proporcionar un valor booleano para un parámetro de modificador. Para proporcionar un valor booleano para un parámetro de modificador en el valor del parámetro **File**, escriba el nombre y el valor de parámetro entre llaves, como en el ejemplo siguiente: `-File .\Get-Script.ps1 {-All:$False}`

### -InputFormat {Text | XML}
Describe el formato de los datos que se envían a Windows PowerShell. Los valores válidos son "Text" (cadenas de texto) o "XML" (formato CLIXML serializado).

### -Mta
Inicia Windows PowerShell con un contenedor multiproceso. [!INCLUDE[ps_sdk_paramintrodps3](../Token/ps_sdk_paramintrodps3_md.md)] En [!INCLUDE[psversion3](../Token/psversion3_md.md)], el valor predeterminado es el contenedor uniproceso (STA). En [!INCLUDE[psversion2](../Token/psversion2_md.md)], el valor predeterminado es el contenedor multiproceso (MTA).

### -NoExit
No se cierra después de ejecutar comandos de inicio.

### -NoLogo
Oculta la pancarta de copyright al inicio.

### -NonInteractive
No presenta un aviso interactivo al usuario.

### -NoProfile
No carga el perfil de Windows PowerShell.

### -OutputFormat {Text | XML}
Determina cómo se formatea la salida de Windows PowerShell. Los valores válidos son "Text" (cadenas de texto) o "XML" (formato CLIXML serializado).

### -PSConsoleFile <FilePath>
Carga el archivo de consola de Windows PowerShell especificado. Escriba la ruta de acceso y el nombre del archivo de consola. Para crear un archivo de consola, use el cmdlet [Export-Console](https://technet.microsoft.com/en-us/library/4bab1c02-9e61-4aaf-9957-11d1934ef4ef) en Windows PowerShell.

### -Sta
Inicia Windows PowerShell con un contenedor uniproceso. En [!INCLUDE[psversion3](../Token/psversion3_md.md)], el valor predeterminado es el contenedor uniproceso (STA). En [!INCLUDE[psversion2](../Token/psversion2_md.md)], el valor predeterminado es el contenedor multiproceso (MTA).

### -Version <Windows PowerShell Version>
Inicia la versión especificada de Windows PowerShell. La versión que especifique debe estar instalada en el sistema. Si [!INCLUDE[psversion3](../Token/psversion3_md.md)] está instalado en el equipo, los valores válidos son "2.0" y "3.0". El valor predeterminado es "3.0".

Si [!INCLUDE[psversion3](../Token/psversion3_md.md)] no está instalado, el único valor válido es "2.0". Otros valores se omiten.

Para más información, consulte "Instalación de Windows PowerShell" en [Introducción a Windows PowerShell [antiguo MSDN]](https://technet.microsoft.com/en-us/library/69555d95-b481-43e1-86e7-b46d68b3e2dd).

### -WindowStyle <Window style>
Establece el estilo de ventana de la sesión. Los valores válidos son Normal, Minimizada, Maximizada y Oculta.

### -Command
Ejecuta los comandos especificados (y todos los parámetros) como si se hubiesen escrito en el símbolo del sistema de Windows PowerShell y, después, se cierra, a menos que se especifique el parámetro NoExit.

El valor del comando puede ser "-", una cadena o un bloque de scripts. Si el valor de Command es "-", el texto del comando se lee de la entrada estándar.

Los bloques de scripts deben incluirse entre llaves ({}). Puede especificar un bloque de scripts solo cuando ejecute PowerShell.exe en Windows PowerShell. Los resultados del script se devuelven al shell principal como objetos XML deserializados, no como objetos activos.

Si el valor de Command es una cadena, **Command** debe ser el último parámetro del comando, porque los caracteres escritos después del comando se interpretan como argumentos del comando.

Para escribir una cadena que ejecute un comando de Windows PowerShell, use el formato:

```
"& {<command>}"
```

donde las comillas indican una cadena y el operador de invocación (&) hace que se ejecute el comando.

### -Help, -?, /?
Muestra este mensaje. Si está escribiendo un comando de PowerShell.exe en Windows PowerShell, anteponga a los parámetros del comando un guion (-), no una barra diagonal (/). Puede usar un guion o una barra diagonal en Cmd.exe.

> [!NOTE]
> Nota de solución de problemas: En Windows PowerShell 2.0, al iniciar algunos programas en la consola de Windows PowerShell, se produce un error LastExitCode 0xc0000142.

## EJEMPLOS

```
PowerShell -PSConsoleFile sqlsnapin.psc1

PowerShell -Version 2.0 -NoLogo -InputFormat text -OutputFormat XML

PowerShell -Command {Get-EventLog -LogName security}

PowerShell -Command "& {Get-EventLog -LogName security}"

# To use the -EncodedCommand parameter:
$command = "dir 'c:\program files' "
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encodedCommand = [Convert]::ToBase64String($bytes)
powershell.exe -encodedCommand $encodedCommand
```



<!--HONumber=Apr16_HO2-->


