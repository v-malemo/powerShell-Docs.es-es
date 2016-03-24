# Seguimiento y registro de scripts

Mientras que Windows PowerShell ya tiene la opción de Directiva de grupo **LogPipelineExecutionDetails** para registrar la invocación de cmdlets, el lenguaje de scripting de PowerShell tiene muchas características que quizás quiera registrar y/o auditar. La nueva característica de seguimiento detallado de scripts permite habilitar el seguimiento detallado y el análisis del uso de scripting de Windows PowerShell en un sistema. Después de habilitar el seguimiento detallado de scripts, Windows PowerShell registra todos los bloques de script en el registro de eventos ETW, **Microsoft-Windows-PowerShell/Operational**. Si un bloque de script crea otro bloque de script (por ejemplo, un script que llame al cmdlet Invoke-Expression en una cadena), también se registra este bloque de script resultante.

El registro de estos eventos se puede habilitar a través de la opción de Directiva de grupo **Activar el registro de bloque de script de PowerShell** (en Plantillas administrativas -> Componentes de Windows -> Windows PowerShell).

Los eventos son:

| Canal | Operativo                                 |
|---------|---------------------------------------------|
| Nivel   | Verbose                                     |
| Código de operación  | Crear                                      |
| Tarea    | CommandStart                                |
| Palabra clave | Espacio de ejecución                                    |
| Id. de evento | Engine_ScriptBlockCompiled (0x1008 = 4104)  |
| Mensaje | Creando texto de bloque de script (%1 de %2): </br> %3 </br> Id. de bloque de script: %4 |


El texto insertado en el mensaje es la extensión del bloque de script compilado. El identificador es un GUID que se conserva durante la vida del bloque de script.

Cuando se habilita el registro detallado, la característica escribe los marcadores de inicio y fin:

| Canal | Operativo                                            |
|---------|--------------------------------------------------------|
| Nivel   | Verbose                                                |
| Código de operación  | Abrir (/Cerrar)                                         |
| Tarea    | CommandStart (/ CommandStop)                           |
| Palabra clave | Espacio de ejecución                                               |
| Id. de evento | ScriptBlock\_Invoke\_Start\_Detail (0x1009 = 4105) / </br> ScriptBlock\_Invoke\_Complete\_Detail (0x100A = 4106) |
| Mensaje | Invocación iniciada (/completada) del id. de bloque de script: %1 </br> Id. de espacio de ejecución: %2 |

El identificador es el GUID que representa el bloque de script (que puede estar correlacionado con Id. de evento 0x1008) y el identificador de espacio de ejecución representa el espacio de ejecución en el que se ejecutó este bloque de script.

Los signos de porcentaje del mensaje de invocación representan las propiedades de ETW estructuradas. Mientras se reemplazan por los valores reales en el texto del mensaje, una manera más segura de acceder a ellos es recuperar el mensaje con el cmdlet Get-WinEvent y, luego, usara la matriz **Properties** del mensaje.

Este es un ejemplo de cómo esta funcionalidad puede ayudarle a descubrir un intento malicioso para cifrar y ofuscar un script:

```powershell
## Malware
function SuperDecrypt
{
    param($script)
    $bytes = [Convert]::FromBase64String($script)
             
    ## XOR “encryption”
    $xorKey = 0x42
    for($counter = 0; $counter -lt $bytes.Length; $counter++)
    {
        $bytes[$counter] = $bytes[$counter] -bxor $xorKey
    }
    [System.Text.Encoding]::Unicode.GetString($bytes)
}

$decrypted = SuperDecrypt "FUIwQitCNkInQm9CCkItQjFCNkJiQmVCEkI1QixCJkJlQg=="
Invoke-Expression $decrypted
```

Si se ejecuta, generará las entradas de registro siguientes:

```
Compiling Scriptblock text (1 of 1):
function SuperDecrypt
{
    param($script)
    $bytes = [Convert]::FromBase64String($script)
    ## XOR "encryption"
    $xorKey = 0x42
    for($counter = 0; $counter -lt $bytes.Length; $counter++)
    {
        $bytes[$counter] = $bytes[$counter] -bxor $xorKey
    }
    [System.Text.Encoding]::Unicode.GetString($bytes)

}
ScriptBlock ID: ad8ae740-1f33-42aa-8dfc-1314411877e3

Compiling Scriptblock text (1 of 1):
$decrypted = SuperDecrypt "FUIwQitCNkInQm9CCkItQjFCNkJiQmVCEkI1QixCJkJlQg=="
ScriptBlock ID: ba11c155-d34c-4004-88e3-6502ecb50f52

Compiling Scriptblock text (1 of 1):
Invoke-Expression $decrypted
ScriptBlock ID: 856c01ca-85d7-4989-b47f-e6a09ee4eeb3

Compiling Scriptblock text (1 of 1):
Write-Host 'Pwnd'
ScriptBlock ID: 5e618414-4e77-48e3-8f65-9a863f54b4c8
```

Si la longitud del bloque de script supera la que ETW es capaz de contener en un único evento, Windows PowerShell divide el script en varias partes. A continuación, se muestra código de ejemplo para volver a combinar un script a partir de sus mensajes de registro:

```powershell
$created = Get-WinEvent -FilterHashtable @{ ProviderName="Microsoft-Windows-PowerShell"; Id = 4104 } | Where-Object { $_.<...> }
$sortedScripts = $created | sort { $_.Properties[0].Value }
$mergedScript = -join ($sortedScripts | % { $_.Properties[2].Value })
```

Al igual que con todos los sistemas de registro que tienen un búfer de retención limitado (es decir, registros de ETW), un ataque contra esta infraestructura es inundar el registro de eventos falsos para ocultar las pruebas anteriores. Para protegerse de este ataque, asegúrese de que tiene algún tipo de colección de registro de eventos configurado (es decir, Colección de reenvío de eventos de Windows, [Spotting the Adversary with Windows Event Log Monitoring](http://www.nsa.gov/ia/_files/app/Spotting_the_Adversary_with_Windows_Event_Log_Monitoring.pdf)) para desplazar los registros de eventos del equipo tan pronto como sea posible.
<!--HONumber=Mar16_HO2-->
