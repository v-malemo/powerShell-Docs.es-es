# Recursos de DSC Registry

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso **Registry** de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para administrar valores y claves del Registro en un nodo de destino.

## Sintaxis

```
Registry [string] #ResourceName
{
    Key = [string]
    ValueName = [string]
    [ Ensure = [string] { Absent | Present }  ]
    [ Force =  [bool]   ]
    [ Hex = [bool] ]
    [ DependsOn = [string[]] ]
    [ ValueData = [string[]] ]
    [ ValueType = [string] { Binary | Dword | ExpandString | MultiString | Qword | String }  ]
}
```

## Propiedades
|  Propiedad  |  Descripción   | 
|---|---| 
| Clave| Indica la ruta de acceso de la clave del Registro para la que quiere garantizar un estado específico. Esta ruta de acceso debe incluir el subárbol.| 
| ValueName| Indica el nombre del valor del Registro.| 
| Ensure| Indica si existen la clave y valor. Para asegurarse de que existan, establezca esta propiedad en "Present". Para asegurarse de que no existan, establezca esta propiedad en "Absent". El valor predeterminado es "Present".| 
| Force| Si la clave del Registro especificada existe, __Force__ la sobrescribirá con el nuevo valor.| 
| Hex| Indica si los datos se expresarán en formato hexadecimal. Si se especifica, los datos de valores DWORD o QWORD se muestran en formato hexadecimal. No es válido para otros tipos. El valor predeterminado es __$false__.| 
| DependsOn| Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 
| ValueData| Los datos para el valor del Registro.| 
| ValueType| Indica el tipo del valor. Los tipos admitidos son: 
<ul><li>Cadena (REG_SZ)</li>


<li>Binario (REG-BINARY)</li>


<li>Dword de 32 bits (REG_DWORD)</li>


<li>Qword de 64 bits (REG_QWORD)</li>


<li>Cadena múltiple (REG_MULTI_SZ)</li>


<li>Cadena expandible (REG_EXPAND_SZ)</li></ul>

## Ejemplo
```powershell
Registry RegistryExample
{
    Ensure = "Present"  # You can also set Ensure to "Absent"
    Key = "HKEY_LOCAL_MACHINE\SOFTWARE\ExampleKey"
    ValueName = "TestValue"
    ValueData = "TestData"
}
```

<!--HONumber=Feb16_HO4-->
