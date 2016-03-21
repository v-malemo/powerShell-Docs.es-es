# Escribir un recurso de DSC personalizado con clases de PowerShell

> Se aplica a: Windows PowerShell 5.0

Con la introducción de las clases de PowerShell en Windows PowerShell 5.0, ahora puede definir un recurso de DSC mediante la creación de una clase. La clase define el esquema y la implementación del recurso, así que no hay necesidad de crear un archivo MOF independiente. La estructura de carpetas de un recurso basado en clases también es más sencilla, porque no es necesaria una carpeta **DSCResources**.

En un recurso de DSC basados en clases, el esquema se define como propiedades de la clase que se pueden modificar con atributos para especificar el tipo de propiedad. El recurso se implementa con los métodos **Get()**, **Set()** y **Test()** (equivalentes a las funciones **Get-TargetResource**, **Set-TargetResource** y **Test-TargetResource** en un recurso de script).

En este tema, se creará un recurso simple denominado **FileResource** que administra un archivo en una ruta de acceso especificada.

Para más información sobre los recursos DSC, consulte [Crear recursos de configuración de estado deseado de Windows PowerShell personalizados](authoringResource.md).

## Estructura de carpetas de un recurso de clase

Para implementar un recurso de DSC personalizado con una clase de PowerShell, cree la siguiente estructura de carpetas. La clase se define en **MyDscResource.psm1** y el manifiesto del módulo se define en **MyDscResource.psd1**.

```
$env: psmodulepath (folder)
    |- MyDscResource (folder)
        |- MyDscResource.psm1 
           MyDscResource.psd1 
```

### Módulos anidados

Como alternativa, puede dividir los recursos de varios archivos `.psm1` e incluirlos como módulos anidados.
Es razonable, cuando tiene una gran cantidad de recursos y colocarlos en un archivo resultaría difícil de administrar.

```
$env: psmodulepath (folder)
    |- MyDscResource (folder)
        |- MyDscResourceA.psm1
           MyDscResourceB.psm1 
           MyDscResource.psd1 
```

Puede colocar una clase en cada archivo, o varias de ellas. 
Puede ser útil para agrupar los recursos por subárea dentro de un módulo anidado.
Desde el punto de vista del usuario, no hay ninguna diferencia en el uso.
Todos los recursos se mostrarán en el módulo `MyDscResource`.
Considere estos módulos anidados como detalles de implementación y úselos para su comodidad.

## Crear la clase

Utilice la palabra clave class para crear una clase de PowerShell. Para especificar que una clase es un recurso de DSC, use el atributo **DscResource()**. El nombre de la clase es el nombre del recurso de DSC.

```powershell
[DscResource()]
class FileResource {
}
```

### Declarar propiedades

El esquema de recursos de DSC se define como propiedades de la clase. Se declaran tres propiedades como se indica a continuación.

```powershell
[DscProperty(Key)]
[string]$Path

[DscProperty(Mandatory)]
[Ensure] $Ensure

[DscProperty(Mandatory)]
[string] $SourcePath

[DscProperty(NotConfigurable)]
[Nullable[datetime]] $CreationTime
```

Observe que las propiedades se modifican mediante atributos. El significado de los atributos es el que se muestra a continuación:

- **DscProperty(Key)**: la propiedad es obligatoria. Se trata de una clave. Los valores de todas las propiedades marcadas como claves deben combinarse para identificar de forma única la instancia de un recurso dentro de una configuración.
- **DscProperty(Mandatory)**: la propiedad es obligatoria.
- **DscProperty(NotConfigurable)**: la propiedad es de solo lectura. Las propiedades marcadas con este atributo no se pueden establecer mediante una configuración, pero se rellenan con el método **Get()** cuando existe.
- **DscProperty()**: la propiedad se puede configurar, pero no es obligatoria.

Las propiedades **$Path** y **$SourcePath** son cadenas. **$CreationTime** es una propiedad [DateTime](https://technet.microsoft.com/en-us/library/system.datetime.aspx). La propiedad **$Ensure** es un tipo de enumeración, que se define como se indica a continuación.

```powershell
enum Ensure 
{ 
    Absent 
    Present 
}
```

### Implementar los métodos

Los métodos **Get()**, **Set()** y **Test()** son análogos a las funciones **Get-TargetResource**, **Set-TargetResource** y **Test-TargetResource** de un recurso de script.

Este código también incluye la función CopyFile(), una función auxiliar que copia el archivo de **$SourcePath** a **$Path**. 

```powershell

    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)

        if ($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if ($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#
        This method is equivalent of the Test-TargetResource script function.
        It should return True or False, showing whether the resource
        is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if ($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ErrorAction Ignore

        if ($item -eq $null)
        {
            $present = $false
        }
        elseif ($item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif ($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }

        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if (-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = New-Object -TypeName System.IO.FileInfo($this.Path)

        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            <#
                Use CreateDirectory instead of New-Item to avoid code
                to handle the non-terminating error
            #>
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if (Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        # DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
```

### El archivo completo
A continuación se muestra el archivo de clases completo.

```powershell
enum Ensure
{
    Absent
    Present
}

<#
   This resource manages the file in a specific path.
   [DscResource()] indicates the class is a DSC resource
#>

[DscResource()]
class FileResource
{
    <#
       This property is the fully qualified path to the file that is
       expected to be present or absent.

       The [DscProperty(Key)] attribute indicates the property is a
       key and its value uniquely identifies a resource instance.
       Defining this attribute also means the property is required
       and DSC will ensure a value is set before calling the resource.

       A DSC resource must define at least one key property.
    #>
    [DscProperty(Key)]
    [string]$Path

    <#
        This property indicates if the settings should be present or absent
        on the system. For present, the resource ensures the file pointed
        to by $Path exists. For absent, it ensures the file point to by
        $Path does not exist.

        The [DscProperty(Mandatory)] attribute indicates the property is
        required and DSC will guarantee it is set.

        If Mandatory is not specified or if it is defined as
        Mandatory=$false, the value is not guaranteed to be set when DSC
        calls the resource.  This is appropriate for optional properties.
    #>
    [DscProperty(Mandatory)]
    [Ensure] $Ensure

    <#
       This property defines the fully qualified path to a file that will
       be placed on the system if $Ensure = Present and $Path does not
        exist.

       NOTE: This property is required because [DscProperty(Mandatory)] is
        set.
    #>
    [DscProperty(Mandatory)]
    [string] $SourcePath

    <#
       This property reports the file's create timestamp.

       [DscProperty(NotConfigurable)] attribute indicates the property is
       not configurable in DSC configuration.  Properties marked this way
       are populated by the Get() method to report additional details
       about the resource when it is present.

    #>
    [DscProperty(NotConfigurable)]
    [Nullable[datetime]] $CreationTime

    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)
        if ($this.ensure -eq [Ensure]::Present)
        {
            if (-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if ($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#
        This method is equivalent of the Test-TargetResource script function.
        It should return True or False, showing whether the resource
        is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if ($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
        {
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate resources.
        This method returns an instance of this class with the updated key
         properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }

        return $this
    }

    <#
        Helper method to check if the file exists and it is file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ea Ignore
        if ($item -eq $null)
        {
            $present = $false
        }
        elseif ($item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif ($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }

        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if (-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo] $destFileInfo = new-object System.IO.FileInfo($this.Path)
        if (-not $destFileInfo.Directory.Exists)
        {
            Write-Verbose -Message "Creating directory $($destFileInfo.Directory.FullName)"

            <#
                Use CreateDirectory instead of New-Item to avoid code
                 to handle the non-terminating error
            #>
            [System.IO.Directory]::CreateDirectory($destFileInfo.Directory.FullName)
        }

        if (Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $($this.SourcePath) to $($this.Path)"

        # DSC engine catches and reports any error that occurs
        Copy-Item -LiteralPath $this.SourcePath -Destination $this.Path -Force
    }
} # This module defines a class for a DSC "FileResource" provider.
```


## Crear un manifiesto

Para que un recurso basado en clases esté disponible para el motor de DSC, debe incluir una instrucción **DscResourcesToExport** en el archivo de manifiesto que indica el módulo que se exporten los recursos. 

```powershell
@{

# Script module or binary module file associated with this manifest.
RootModule = 'MyDscResource.psm1'

DscResourcesToExport = @('FileResource')

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '81624038-5e71-40f8-8905-b1a87afe22d7'

# Author of this module
Author = 'Microsoft Corporation'

# Company or vendor of this module
CompanyName = 'Microsoft Corporation'

# Copyright statement for this module
Copyright = '(c) 2014 Microsoft. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '5.0'

# Name of the Windows PowerShell host required by this module
# PowerShellHostName = ''
} 
```

Si utiliza **módulos anidados** para dividir los recursos en varios archivos, debe colocar la lista de módulos anidados en la clave `NestedModules`.

```powershell
@{

# Don't specify RootModule

# Script module or binary module file associated with this manifest.
NestedModules = @('MyDscResourceA.psm1', 'MyDscResourceB.psm1')

DscResourcesToExport = @('MyDscResourceA', 'MyDscResourceB')

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '81624038-5e71-40f8-8905-b1a87afe22d7'

# Author of this module
Author = 'Microsoft Corporation'

# Company or vendor of this module
CompanyName = 'Microsoft Corporation'

# Copyright statement for this module
Copyright = '(c) 2014 Microsoft. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the Windows PowerShell engine required by this module
PowerShellVersion = '5.0'

# Name of the Windows PowerShell host required by this module
# PowerShellHostName = ''
} 
```

## Probar el recurso

Después de guardar la clase y los archivos de manifiesto en la estructura de carpetas tal y como se describió anteriormente, puede crear una configuración que utilice el nuevo recurso. Para obtener información sobre cómo ejecutar una configuración DSC, consulte [Establecer configuraciones](enactingConfigurations.md). La configuración siguiente comprobará si existe el archivo en `c:\test\test.txt` y, si no es así, copia el archivo desde `c:\test.txt` (debe crear `c:\test.txt` antes de ejecutar la configuración).

```powershell
Configuration Test
{
    Import-DSCResource -module MyDscResource
    FileResource file
    {
        Path = "C:\test\test.txt"
        SourcePath = "c:\test.txt"
        Ensure = "Present"
    } 
}
Test
Start-DscConfiguration -Wait -Force Test
```

## Consulte también
### Conceptos
[Crear recursos de configuración de estado deseado de Windows PowerShell personalizados](authoringResource.md)
<!--HONumber=Mar16_HO1-->
