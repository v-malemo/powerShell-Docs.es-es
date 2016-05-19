# Escribir un recurso de DSC personalizado con MOF

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

En este tema se define el esquema de un recurso personalizado de configuración de estado deseado (DSC) de Windows PowerShell en un archivo MOF y se implementa el recurso en un archivo de script de Windows PowerShell. Este recurso personalizado es para la creación y el mantenimiento de un sitio web.

## Crear el esquema MOF

El esquema define las propiedades del recurso que se pueden configurar mediante un script de configuración DSC.

### Estructura de carpetas de un recurso MOF

Para implementar un recurso de DSC personalizado con un esquema MOF, cree la siguiente estructura de carpetas. El esquema MOF se define en el archivo Demo_IISWebsite.schema.mof y el script del recurso se define en Demo_IISWebsite.psm1. Opcionalmente, puede crear un archivo de manifiesto del módulo (psd1).

```
$env:PSModulePath (folder)
    |- MyDscResources (folder)
        |- DSCResources (folder)
            |- Demo_IISWebsite (folder)
                |- Demo_IISWebsite.psd1 (file, optional)
                |- Demo_IISWebsite.psm1 (file, required)
                |- Demo_IISWebsite.schema.mof (file, required)
```

Tenga en cuenta que es necesario crear una carpeta denominada DSCResources en la carpeta de nivel superior y que la carpeta de cada recurso debe tener el mismo nombre que el recurso.

### Contenido del archivo MOF

A continuación se muestra un ejemplo de un archivo MOF que se puede utilizar para un recurso de sitio web personalizado. Para seguir este ejemplo, guarde este esquema en un archivo y asigne como nombre del archivo *Demo_IISWebsite.schema.mof*..

```
[ClassVersion("1.0.0"), FriendlyName("Website")] 
class Demo_IISWebsite : OMI_BaseResource
{
  [Key] string Name;
  [Required] string PhysicalPath;
  [write,ValueMap{"Present", "Absent"},Values{"Present", "Absent"}] string Ensure;
  [write,ValueMap{"Started","Stopped"},Values{"Started", "Stopped"}] string State;
  [write] string Protocol[];
  [write] string BindingInfo[];
  [write] string ApplicationPool;
  [read] string ID;
};
```

Tenga en cuenta lo siguiente sobre el código anterior:

* `FriendlyName` define el nombre que se puede utilizar para hacer referencia a este recurso personalizado en los scripts de configuración DSC. En este ejemplo, `Website` equivale al nombre descriptivo `Archive` para el recurso integrado Archive.
* La clase que defina para el recurso personalizado debe derivarse de . `OMI_BaseResource`.
* El calificador de tipo, `[Key]`, en una propiedad indica que esta propiedad identificará de forma única la instancia del recurso. Se necesita al menos una propiedad `[Key]`.
* El calificador `[Required]` indica que la propiedad es obligatoria (debe especificarse un valor en cualquier script de configuración que use este recurso).
* El calificador `[write]` indica que esta propiedad es opcional cuando se utiliza el recurso personalizado en un script de configuración. El calificador `[read]` indica que una propiedad no se puede establecer mediante una configuración y es solo con fines informativos.
* `Values` restringe los valores que se pueden asignar a la propiedad a la lista de valores definidos en `ValueMap`. Para más información, consulte [ValueMap and Value Qualifiers](https://msdn.microsoft.com/library/windows/desktop/aa393965.aspx) (Calificadores Value y ValueMap)..
* Se recomienda incluir una propiedad denominada `Ensure` con los valores `Present` y `Absent` en el recurso como una forma de mantener un estilo coherente con los recursos integrados de DSC.
* Asigne el nombre del archivo de esquema para el recurso personalizado de la siguiente forma: `classname.schema.mof`, donde `classname` es el identificador que sigue a la palabra clave `class` en la definición del esquema.

### Escribir el script del recurso

El script del recurso implementa la lógica del recurso. En este módulo, debe incluir tres funciones llamadas **Get-TargetResource**, **Set-TargetResource** y **Test-TargetResource**. Las tres funciones deben tomar un conjunto de parámetros que sea idéntico al conjunto de propiedades definidas en el esquema MOF que creó para el recurso. En este documento, este conjunto de propiedades se conoce como "propiedades del recurso". Almacene estas tres funciones en un archivo llamado <ResourceName>.psm1. En el ejemplo siguiente, las funciones se almacenan en un archivo denominado Demo_IISWebsite.psm1.

> **Nota**: Cuando se ejecuta el mismo script de configuración en el recurso más de una vez, no se deberían obtener errores y el recurso debería permanecer en el mismo estado que si se hubiera ejecutado el script una vez. Para lograrlo, asegúrese de que las funciones **Get-TargetResource** y **Test-TargetResource** no modifiquen el recurso y de que la invocación de la función **Set-TargetResource** más de una vez en una secuencia con los mismos valores de los parámetros sea siempre equivalente a invocarla una vez.

En la implementación de la función **Get-TargetResource**, utilice los valores de propiedades clave del recurso que se proporcionan como parámetros para comprobar el estado de la instancia del recurso especificado. Esta función debe devolver una tabla hash que enumere todas las propiedades del recurso como claves y los valores reales de estas propiedades como los valores correspondientes. En el código siguiente se muestra un ejemplo.

```powershell
# DSC uses the Get-TargetResource function to fetch the status of the resource instance specified in the parameters for the target machine
function Get-TargetResource 
{
    param 
    (       
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )

        $getTargetResourceResult = $null;

        <# Insert logic that uses the mandatory parameter values to get the website and assign it to a variable called $Website #>
        <# Set $ensureResult to "Present" if the requested website exists and to "Absent" otherwise #>

        # Add all Website properties to the hash table
        # This simple example assumes that $Website is not null
        $getTargetResourceResult = @{
                                      Name = $Website.Name; 
                                        Ensure = $ensureResult;
                                        PhysicalPath = $Website.physicalPath;
                                        State = $Website.state;
                                        ID = $Website.id;
                                        ApplicationPool = $Website.applicationPool;
                                        Protocol = $Website.bindings.Collection.protocol;
                                        Binding = $Website.bindings.Collection.bindingInformation;
                                    }
  
        $getTargetResourceResult;
}
```

Según los valores especificados para las propiedades del recurso en el script de configuración, la función **Set-TargetResource** debe realizar una de las siguientes acciones:

* Crear un sitio web.
* Actualizar un sitio web existente.
* Eliminar un sitio web existente.

En el ejemplo siguiente se ilustra esto.

```powershell
# The Set-TargetResource function is used to create, delete or configure a website on the target machine. 
function Set-TargetResource 
{
    [CmdletBinding(SupportsShouldProcess=$true)]
    param 
    (       
        [ValidateSet("Present", "Absent")]
        [string]$Ensure = "Present",

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Name,

        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$PhysicalPath,

        [ValidateSet("Started", "Stopped")]
        [string]$State = "Started",

        [string]$ApplicationPool,

        [string[]]$BindingInfo,

        [string[]]$Protocol
    )
 
    <# If Ensure is set to "Present" and the website specified in the mandatory input parameters does not exist, then create it using the specified parameter values #>
    <# Else, if Ensure is set to "Present" and the website does exist, then update its properties to match the values provided in the non-mandatory parameter values #>
    <# Else, if Ensure is set to "Absent" and the website does not exist, then do nothing #>
    <# Else, if Ensure is set to "Absent" and the website does exist, then delete the website #>
}
```

Por último, la función **Test-TargetResource** debe tomar el mismo parámetro establecido que **Get-TargetResource** y **Set-TargetResource**. En la implementación de **Test-TargetResource**, compruebe el estado de la instancia del recurso que se especifica en los parámetros clave. Si el estado real de la instancia del recurso no coincide con los valores especificados en el conjunto de parámetros, devuelva **$false**. De lo contrario, devuelva **$true**..

El código siguiente implementa la función **Test-TargetResource**.

```powershell
function Test-TargetResource
{
[CmdletBinding()]
[OutputType([System.Boolean])]
param
(
[ValidateSet("Present","Absent")]
[System.String]
$Ensure,

[parameter(Mandatory = $true)]
[System.String]
$Name,

[parameter(Mandatory = $true)]
[System.String]
$PhysicalPath,

[ValidateSet("Started","Stopped")]
[System.String]
$State,

[System.String[]]
$Protocol,

[System.String[]]
$BindingData,

[System.String]
$ApplicationPool
)

#Write-Verbose "Use this cmdlet to deliver information about command processing."

#Write-Debug "Use this cmdlet to write debug information while troubleshooting."


#Include logic to 
$result = [System.Boolean]
#Add logic to test whether the website is present and its status mathes the supplied parameter values. If it does, return true. If it does not, return false.
$result 
}
```

**Nota**: Para una depuración más sencilla, utilice el cmdlet **Write-Verbose** en la implementación de las tres funciones anteriores. Este cmdlet escribe texto en la secuencia de mensajes detallados. De forma predeterminada, la secuencia de mensajes detallados no se muestra, pero se puede mostrar si se cambia el valor de la variable **$VerbosePreference** o se usa el parámetro **Verbose** en los cmdlets de DSC = new.

### Crear el manifiesto del módulo

Por último, utilice el cmdlet **New-ModuleManifest** para definir un <ResourceName>archivo. psd1 para el módulo de recursos personalizado. Al invocar este cmdlet, haga referencia al archivo del módulo de script (.psm1) que se describe en la sección anterior. Incluya **Get-TargetResource**, **Set-TargetResource** y **Test-TargetResource** en la lista de funciones que se deben exportar. A continuación se muestra un archivo de manifiesto de ejemplo.

```powershell
# Module manifest for module 'Demo.IIS.Website'
#
# Generated on: 1/10/2013
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '6AB5ED33-E923-41d8-A3A4-5ADDA2B301DE'

# Author of this module
Author = 'Contoso'

# Company or vendor of this module
CompanyName = 'Contoso'

# Copyright statement for this module
Copyright = 'Contoso. All rights reserved.'

# Description of the functionality provided by this module
Description = 'This Module is used to support the creation and configuration of IIS Websites through Get, Set and Test API on the DSC managed nodes.'

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '4.0'

# Minimum version of the common language runtime (CLR) required by this module
CLRVersion = '4.0'

# Modules that must be imported into the global environment prior to importing this module
RequiredModules = @("WebAdministration")

# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
NestedModules = @("Demo_IISWebsite.psm1")

# Functions to export from this module
FunctionsToExport = @("Get-TargetResource", "Set-TargetResource", "Test-TargetResource")

# Cmdlets to export from this module
#CmdletsToExport = '*'

# HelpInfo URI of this module
# HelpInfoURI = ''
}
```


<!--HONumber=May16_HO2-->


