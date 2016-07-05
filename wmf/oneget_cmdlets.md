# Cmdlets PackageManagement
Este es el núcleo de PackageManagement para admitir la detección, la instalación y el inventario de software (SDII). Pruebe los cmdlets para estas operaciones:
-   Find-Package
-   Find-PackageProvider
-   Get-Package
-   Get-PackageProvider
-   Get-PackageSource
-   Import-PackageProvider
-   Install-Package
-   Install-PackageProvider
-   Register-PackageSource
-   Save-Package
-   Set-PackageSource
-   Uninstall-Package
-   Unregister-PackageSource

Dado que PackageManagement es un módulo de PowerShell, puede hacer lo siguiente para actualizar PackageManagement:
```powershell
PS C:\> Install-Module PackageManagement –Force
```
En este caso, deberá volver a iniciar una sesión de PowerShell para cambiar a la nueva versión de PackageManagement.

## [Cmdlet Find-Package](https://technet.microsoft.com/en-us/library/dn890709.aspx)
Este cmdlet permite la detección de paquetes de software en los orígenes de paquetes disponibles mediante los proveedores de paquetes cargados.
```powershell
# Find all available Windows PowerShell module packages from galleries registered
# with PowerShellGet provider
Find-Package -Provider PowerShellGet -Source PSGallery

# Find a package from a provider that is not yet installed
# This will bootstrap NuGet provider and then search for jquery package using NuGet
# with <http://www.nuget.org/api/v2/> as source
Find-Package -Name jquery –Provider NuGet -Source http://www.nuget.org/api/v2/

# Find package with name and version
# Here we are assuming that the user already registered nuget.org using
# Register-PackageSource. You can specify either the provider or the source, or
# neither. For the latter, performance may be less optimal as it searches through all
# the providers and registered sources.
Find-Package -Name jquery –Provider NuGet –RequiredVersion 2.1.4 -Source nuget.org
```

## [Cmdlet Find-PackageProvider](https://technet.microsoft.com/en-us/library/mt676544.aspx)
El cmdlet Find-PackageProvider busca proveedores de PackageManagement coincidentes que estén disponibles en los orígenes de paquetes registrados con PowerShellGet. Estos son los proveedores de paquetes disponibles para la instalación con el cmdlet Install-PackageProvider. De forma predeterminada, se incluyen los módulos disponibles en la Galería de PowerShell con las etiquetas "PackageManagement" y "Provider". 

Find-PackageProvider también busca proveedores de PackageManagement coincidentes que estén disponibles en el almacén de blobs de Azure de PackageManagement donde se usa el proveedor de programa previo de PackageManagement para buscarlos e instalarlos.
```powershell
#Find all available package providers in PackageManagement azure blob store as well as in PowerShellGallery.com
Find-PackageProvider

#Find all versions of a provider
Find-PackageProvider -Name "Nuget" -AllVersions

#Find a provider from a specified source
Find-PackageProvider -Name "Gistprovider" -Source "PSGallery"
```

## [Cmdlet Get-Package](https://technet.microsoft.com/en-us/library/dn890704.aspx)
Este cmdlet devuelve una lista de todos los paquetes de software instalados con PackageManagement.
```powershell
# Get all the packages installed by Programs provider
Get-Package –Provider Programs

# Get all the packages installed by NuGet provider at c:\test using the dynamic
# parameter destination
Get-Package –Provider NuGet -Destination c:\test
```

## [Cmdlet Get-PackageProvider](https://technet.microsoft.com/en-us/library/dn890703.aspx)
Los proveedores de paquetes que están cargados y listos para usar en el equipo local pueden incluirse en el inventario mediante el cmdlet.
```powershell
# Get all currently loaded package providers
Get-PackageProvider

# The following cmdlet will show all the package providers available on the machine (including those that are not loaded):
Get-PackageProvider -ListAvailable
```

## [Cmdlet Get-PackageSource](https://technet.microsoft.com/en-us/library/dn890705.aspx)
Este cmdlet obtiene una lista de los orígenes de paquetes que están registrados para un proveedor de paquetes.
```powershelll
# Get all package sources
Get-PackageSource

# Get all package sources for a specific provider
Get-PackageSource –ProviderName PowerShellGet
```

## [Cmdlet Import-PackageProvider](https://technet.microsoft.com/en-us/library/mt676545.aspx)
Este cmdlet agrega proveedores de paquetes de PackageManagement a la sesión actual.
```powershell
# Import a package provider from the local machine
Import-PackageProvider –Name MyProvider

#The -Name parameter can be either the name of the provider or the full path to the provider. Currently, we support .dll, .exe and.psm1 for the full path case. If the name of the provider is used for the -Name parameter, then additional version parameters such as -RequiredVersion, -MinimumVersion and -MaximumVersion may be specified. Otherwise, the latest version of the provider will be imported.

#If a package provider is not yet loaded to your system, we can discover and install on-demand. You can use explicit discovery and install cmdlets to do so:
 Find-PackageProvider
 Install-PackageProvider –Name MyProvider

#After installed, follow the Import-PackageProvider to load it to your system.

# Import a specific version of a package provider. PackageManagement supports installations of multiple versions of a package provider using PackageProvider cmdlets (not by bootstrapper provider). You can install another version of a package provider given that you already have one up running by:
Find-PackageProvider –Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force
Get-PackageProvider –ListAvailable
Import-PackageProvider –Name "Nuget" -RequiredVersion "2.8.5.201" -Verbose
Import-PackageProvider –Name MyProvider –RequiredVersion xxxx -force
```

##[ Cmdlet Install-Package](https://technet.microsoft.com/en-us/library/dn890711.aspx)

Este cmdlet permite la instalación de paquetes de software en los orígenes de paquetes disponibles mediante los proveedores de paquetes cargados.
```powershell
# Install a package by name.
# NuGet provider requires us to provide the dynamic parameter destination path
# when we use this provider to install. Not all providers will require you to supply
# dynamic parameters for PackageManagement cmdlets.
Install-Package -Name jquery -Source nuget.org -Destination c:\test

# Install a package by piping.
Find-Package -Name jquery –Provider NuGet | Install-Package -Destination c:\test
```

## [Cmdlet Install-PackageProvider](https://technet.microsoft.com/en-us/library/mt676543.aspx)
Este cmdlet instala uno o varios proveedores de paquetes de PackageManagement.
```powershell
# Install a package provider from the PowerShell Gallery
Install-PackageProvider –Name "Gistprovider" -Verbose

# Install a specified version of a package provider
Find-PackageProvider –Name "Nuget" -AllVersions
Install-PackageProvider -Name "Nuget" -RequiredVersion "2.8.5.201" -Force

# Find a provider and install it
Find-PackageProvider –Name "Gistprovider" | Install-PackageProvider -Verbose

# Install a provider to the current user’s module folder
Install-PackageProvider –Name Gistprovider –Verbose –Scope CurrentUser
```

## [Cmdlet Register-PackageSource](https://technet.microsoft.com/en-us/library/dn890701.aspx)
Este cmdlet agrega un origen de paquete para un proveedor de paquetes especificado.
Cada proveedor de PackageManagement puede tener uno o varios orígenes de software o repositorios. PackageManagement proporciona cmdlets de PowerShell para agregar, quitar o consultar el origen. Por ejemplo, puede registrar un origen de paquete para el proveedor de NuGet:
```powershell
Register-PackageSource -Name "NugetSource" -Location "http://www.nuget.org/api/v2" –ProviderName nuget
```

## [Cmdlet Save-Package](https://technet.microsoft.com/en-us/library/dn890708.aspx)
Este cmdlet guarda los paquetes en el equipo local sin instalarlos.
```powershell
# Saves jquery package to c:\test using NuGetProvider
# Notes that the -Path parameter must point to an existing location
Save-Package -Name jquery –Provider NuGet -Path c:\test

# Save a package by piping.
Find-Package -Name jquery -Source http://www.nuget.org/api/v2/ | Save-Package -Path c:\test
Find-Package -source c:\test
```

## [Cmdlet Set-PackageSource](https://technet.microsoft.com/en-us/library/dn890710.aspx)
Este cmdlet cambia información sobre un origen de paquete existente. 
```powershell
#Set-PackageSource changes the values for a source that has already been registered by running the Register-PackageSource cmdlet. By #running Set-PackageSource, you can change the source name and location.
Set-PackageSource  -Name nuget.org -Location  http://www.nuget.org/api/v2 -NewName nuget2 -NewLocation https://www.nuget.org/api/v2 
```

## [Cmdlet Uninstall-Package](https://technet.microsoft.com/en-us/library/dn890702.aspx)
Este cmdlet desinstala los paquetes instalados en el equipo local.
```powershell
# Uninstall jquery using nuget
Uninstall-Package -Name jquery –Provider NuGet -Destination c:\test

# Uninstall a package with by piping with Get-Package
Get-Package -Name jquery –Provider NuGet -Destination c:\test | Uninstall-Package
```

## [Cmdlet Unregister-PackageSource](https://technet.microsoft.com/en-us/library/dn890707.aspx)
```powershell
# Unregister a package source for the NuGet provider. You can use command Unregister-PackageSource, to disconnect with a repository, and Get-PackageSource, to discover what the repositories are associated with that provider.
Unregister-PackageSource  -Name "NugetSource"
```


<!--HONumber=Jun16_HO4-->


