---
título: Recursos compuestos: uso de una configuración DSC como un recurso ms.date: 16-05-2016 palabras clave: powershell, descripción de DSC:  
ms.topic: autor del artículo: eslesar administrador: dongill ms.prod: powershell
---

# Recursos compuestos: uso de una configuración DSC como un recurso

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

En situaciones del mundo real, las configuraciones pueden acabar siendo largas y complejas, con llamadas a muchos recursos diferentes y estableciendo una gran cantidad de propiedades. Para ayudar a abordar esta complejidad, puede utilizar una configuración de estado deseado (DSC) de Windows PowerShell como recurso para otras configuraciones. Es lo que se conoce como un recurso compuesto. Un recurso compuesto es una configuración DSC que toma parámetros. Los parámetros de la configuración actúan como las propiedades del recurso. La configuración se guarda como un archivo con una extensión **.schema.psm1** y ocupa el lugar tanto del esquema MOF como del recurso de script de un recurso de DSC típico (para más información sobre los recursos de DSC, consulte [Recursos de configuración de estado deseado de Windows PowerShell](resources.md).

## Creación del recurso compuesto

En nuestro ejemplo, se creará una configuración que invoca un número de recursos existentes para configurar las máquinas virtuales. En lugar de especificar los valores que se establecerán en bloques de configuración, la configuración toma un número de parámetros que se utilizan en los bloques de configuración.

```powershell
Configuration xVirtualMachine
{
    param
    (
        # Name of VMs
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String[]] $VMName,

        # Name of Switch to create
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $SwitchName,

        # Type of Switch to create
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $SwitchType,

        # Source Path for VHD
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VHDParentPath,

        # Destination path for diff VHD
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VHDPath,

        # Startup Memory for VM
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VMStartupMemory,

        # State of the VM
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [String] $VMState
    )

    # Import the module that defines custom resources
    Import-DscResource -Module xComputerManagement,xHyper-V

    # Install the Hyper-V role
    WindowsFeature HyperV
    {
        Ensure = "Present"
        Name = "Hyper-V"
    }

    # Create the virtual switch
    xVMSwitch $SwitchName
    {
        Ensure = "Present"
        Name = $SwitchName
        Type = $SwitchType
        DependsOn = "[WindowsFeature]HyperV"
    }

    # Check for Parent VHD file
    File ParentVHDFile
    {
        Ensure = "Present"
        DestinationPath = $VHDParentPath
        Type = "File"
        DependsOn = "[WindowsFeature]HyperV"
    }

    # Check the destination VHD folder
    File VHDFolder
    {
        Ensure = "Present"
        DestinationPath = $VHDPath
        Type = "Directory"
        DependsOn = "[File]ParentVHDFile"
    }

    # Creae VM specific diff VHD
    foreach ($Name in $VMName)
    {
        xVHD "VHD$Name"
        {
            Ensure = "Present"
            Name = $Name
            Path = $VHDPath
            ParentPath = $VHDParentPath
            DependsOn = @("[WindowsFeature]HyperV",
                          "[File]VHDFolder")
        }
    }

    # Create VM using the above VHD
    foreach($Name in $VMName)
    {
        xVMHyperV "VMachine$Name"
        {
            Ensure = "Present"
            Name = $Name
            VhDPath = (Join-Path -Path $VHDPath -ChildPath $Name)
            SwitchName = $SwitchName
            StartupMemory = $VMStartupMemory
            State = $VMState
            MACAddress = $MACAddress
            WaitForIP = $true
            DependsOn = @("[WindowsFeature]HyperV",
                          "[xVHD]VHD$Name")
        }
    }
}
```

### Guardar la configuración como un recurso compuesto

Para usar la configuración con parámetros como un recurso de DSC, guárdela en una estructura de directorios similar a la de cualquier otro recurso basado en MOF y asígnele un nombre con una extensión **.schema.psm1**. En este ejemplo, el archivo se denominará **xVirtualMachine.schema.psm1**. También deberá crear un manifiesto llamado **xVirtualMachine.psd1** que contenga la línea siguiente. Tenga en cuenta que esto es además de **MyDscResources.psd1**, el manifiesto del módulo para todos los recursos de la carpeta **MyDscResources**.

```powershell
RootModule = 'xVirtualMachine.schema.psm1'
```

Cuando haya terminado, la estructura de carpetas debería ser la siguiente.

```
$env: psmodulepath
    |- MyDscResources
           MyDscResources.psd1
        |- DSCResources
            |- xVirtualMachine
                |- xVirtualMachine.psd1
                |- xVirtualMachine.schema.psm1
```

El recurso ahora es detectable mediante el cmdlet Get-DscResource y sus propiedades las puede detectar cualquier cmdlet o mediante la función de autocompletar **CTRL+BARRA ESPACIADORA** en Windows PowerShell ISE.

## Uso del recurso compuesto

A continuación, se crea una configuración que llama al recurso compuesto. Esta configuración llama al recurso compuesto xVirtualMachine para crear una máquina virtual y después llama al recurso **xComputer** para cambiar su nombre.

```powershell

configuration RenameVM
{

    Import-DscResource -Module TestCompositeResource
    Node localhost
    {
        xVirtualMachine VM
        {
            VMName = "Test"
            SwitchName = "Internal"
            SwitchType = "Internal"
            VhdParentPath = "C:\Demo\VHD\RTM.vhd"
            VHDPath = "C:\Demo\VHD"
            VMStartupMemory = 1024MB
            VMState = "Running"
        }
    }

    Node "192.168.10.1"
    {
        xComputer Name
        {
            Name = "SQL01"
            DomainName = "fourthcoffee.com"
        }
    }
}
```

## Consulte también
### Conceptos
* [Escribir un recurso de DSC personalizado con MOF](authoringResourceMOF.md)
* [Introducción a la configuración de estado deseado de Windows PowerShell](overview.md)



<!--HONumber=Jun16_HO1-->


