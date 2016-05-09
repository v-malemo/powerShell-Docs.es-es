# Recurso de DSC Script

 
> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso **Script** de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para ejecutar bloques de script de Windows PowerShell en nodos de destino. El recurso `Script` tiene las propiedades `GetScript`, `SetScript` y `TestScript`. Estas propiedades se deben establecer en los bloques de script que se ejecutarán en cada nodo de destino. 

El bloque de script `GetScript` debe devolver una tabla hash que representa el estado del nodo actual. No es necesario devolver nada. DSC no hace nada con la salida de este bloque de script.

El bloque de script `TestScript` debe determinar si es necesario modificar el nodo actual. Debe devolver `$true` si el nodo está actualizado. Debe devolver `$false` si la configuración del nodo está obsoleta y debe actualizarse por el bloque de script `SetScript`. El bloque de script `TestScript` se invoca mediante el DSC.

El bloque de script `SetScript` debe modificar el nodo. Se invoca mediante el DSC si el bloque `TestScript` devuelve `$false`.

Si necesita usar variables desde el script de configuración en los bloques de script `GetScript`, `TestScript` o `SetScript`, use el ámbito `$using:` (consulte el ejemplo siguiente).


## Sintaxis

```
Script [string] #ResourceName
{
    GetScript = [string]
    SetScript = [string]
    TestScript = [string]
    [ Credential = [PSCredential] ]
    [ DependsOn = [string[]] ]
}
```

## Propiedades

|  Propiedad  |  Descripción   | 
|---|---| 
| GetScript| Proporciona un script de bloque de Windows PowerShell que se ejecuta cuando se invoca el cmdlet [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407379.aspx). Este bloque debe devolver una tabla hash.| 
| SetScript| Proporciona un bloque de script de Windows PowerShell. Cuando se invoca el cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx), el bloque **TestScript** se ejecuta primero. Si el bloque **TestScript** devuelve **$false**, se ejecutará el bloque **SetScript**. Si el bloque **TestScript** devuelve **$true**, no se ejecutará el bloque **SetScript**.| 
| TestScript| Proporciona un bloque de script de Windows PowerShell. Cuando se invoca el cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx), se ejecuta este bloque. Si devuelve **$false**, se ejecutará el bloque SetScript. Si devuelve **$true**, no se ejecutará el bloque SetScript. El bloque **TestScript** también se ejecuta cuando se invoca el cmdlet [Test-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx). Sin embargo, en este caso, el bloque **SetScript** no se ejecutará, independientemente del valor que devuelva el bloque TestScript. El bloque **TestScript** debe devolver True si la configuración real coincide con la configuración de estado deseado actual, y False si no coincide. (La configuración de estado deseado actual es la última configuración establecida en el nodo que está usando DSC).| 
| Credential| Indica las credenciales que se usan para ejecutar este script, si se necesitan credenciales.| 
| DependsOn| Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es **ResourceName** y su tipo es **ResourceType**, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.

## Ejemplo 1
```powershell
Script ScriptExample
{
    SetScript = { 
        $sw = New-Object System.IO.StreamWriter("C:\TempFolder\TestFile.txt")
        $sw.WriteLine("Some sample string")
        $sw.Close()
    }
    TestScript = { Test-Path "C:\TempFolder\TestFile.txt" }
    GetScript = { <# This must return a hash table #> }          
}
```

## Ejemplo 2
```powershell
$version = Get-Content 'version.txt'
Script UpdateConfigurationVersion
{
    GetScript = { 
        $currentVersion = Get-Content (Join-Path -Path $env:SYSTEMDRIVE -ChildPath 'version.txt')
        return @{ 'Version' = $currentVersion }
    }          
    TestScript = { 
        $state = GetScript
        if( $state['Version'] -eq $using:version )
        {
            Write-Verbose -Message ('{0} -eq {1}' -f $state['Version'],$using:version)
            return $true
        }
        Write-Verbose -Message ('Version up-to-date: {0}' -f $using:version)
        return $false
    }
    SetScript = { 
        $using:version | Set-Content -Path (Join-Path -Path $env:SYSTEMDRIVE -ChildPath 'version.txt')
    }
}
```

Este recurso está escribiendo la versión de la configuración en un archivo de texto. Esta versión está disponible en el equipo cliente, pero no en cualquiera de los nodos, por lo que debe pasarse a cada uno de los bloques de script del recurso `Script` con el ámbito `using` de PowerShell. Al generar el archivo MOF del nodo, el valor de la variable `$version` se lee desde un archivo de texto en el equipo cliente. DSC reemplaza las variables de `$using:version` en cada bloque de script con el valor de la variable `$version`.



<!--HONumber=Apr16_HO2-->


