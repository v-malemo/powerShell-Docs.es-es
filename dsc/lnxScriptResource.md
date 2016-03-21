# Recurso nxScript de DSC para Linux

El recurso **nxScript** de la configuración de estado deseado (DSC) de PowerShell ofrece un mecanismo para ejecutar scripts de Linux en un nodo de Linux.

## Sintaxis

```
nxScript <string> #ResourceName
{
    GetScript = <string>
    SetScript = <string>
    TestScript = <string>
    [ User = <string> ]
    { Group = <string> ]
    [ DependsOn = <string[]> ]

}
```

## Propiedades

|  Propiedad |  Descripción | 
|---|---|
| GetScript| Proporciona un script que se ejecuta cuando se invoca el cmdlet [Get-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521625.aspx). El script debe comenzar con el par de caracteres shebang, como por ejemplo: #!/bin/bash.| 
| SetScript| Proporciona un script. Cuando se invoca el cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx), el bloque **TestScript** se ejecuta primero. Si el bloque **TestScript** devuelve un código de salida distinto de 0, se ejecutará el bloque **SetScript**. Si el bloque **TestScript** devuelve un código de salida de 0, no se ejecutará el bloque **SetScript**. El script debe comenzar con el par de caracteres shebang, como por ejemplo: `#!/bin/bash`.| 
| TestScript| Proporciona un script. Cuando se invoca el cmdlet [Start-DscConfiguration](https://technet.microsoft.com/en-us/library/dn521623.aspx), se ejecuta este script. Si devuelve un código de salida distinto de 0, se ejecutará el bloque SetScript. Si devuelve un código de salida de 0, no se ejecutará el bloque **SetScript**. El bloque **TestScript** también se ejecuta cuando se invoca el cmdlet [Test-DscConfiguration](https://technet.microsoft.com/en-us/library/dn407382.aspx). Sin embargo, en este caso, el bloque **SetScript** no se ejecutará, independientemente del código de salida que se devuelva del bloque **TestScript**. El bloque **TestScript** debe devolver un código de salida de 0 si la configuración real coincide con la configuración de estado deseado actual, y un código de salida distinto de 0 si no coincide. (La configuración de estado deseado actual es la última configuración establecida en el nodo que está usando DSC). El script debe comenzar con el par de caracteres shebang, como por ejemplo: 1#!/bin/bash.1| 
| Usuario| El usuario con el que se ejecutará el script.| 
| Grupo| El grupo con el que se ejecutará el script.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento **ID** del bloque del script de configuración del recurso que quiere ejecutar primero es **ResourceName** y su tipo es **ResourceType**, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 

## Ejemplo

En el ejemplo siguiente se muestra el uso del recurso **nxScript** para realizar una administración de configuración adicional.

```
Import-DSCResource -Module nx 

Node $node {
nxScript KeepDirEmpty{

    GetScript = @"
#!/bin/bash
ls /tmp/mydir/ | wc -l
"@

    SetScript = @"
#!/bin/bash
rm -rf /tmp/mydir/*
"@

    TestScript = @'
#!/bin/bash
filecount=`ls /tmp/mydir | wc -l`
if [ $filecount -gt 0 ]
then
    exit 1
else
    exit 0
fi
'@
} 
}
```
<!--HONumber=Feb16_HO4-->
