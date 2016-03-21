# Recurso de DSC Script

 
> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso **Script** de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para ejecutar bloques de script de Windows PowerShell en nodos de destino.

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

## Ejemplo
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

<!--HONumber=Feb16_HO4-->
