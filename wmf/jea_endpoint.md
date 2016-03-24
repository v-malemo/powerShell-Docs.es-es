# Crear un punto de conexión de JEA y conectarse a este
Para crear un punto de conexión de JEA, debe crear y registrar un archivo de configuración de sesión de PowerShell especialmente configurado, que se puede generar con el cmdlet **New-PSSessionConfigurationFile**.

```powershell
New-PSSessionConfigurationFile -SessionType RestrictedRemoteServer -TranscriptDirectory "C:\ProgramData\JEATranscripts" -RunAsVirtualAccount -RoleDefinitions @{ 'CONTOSO\NonAdmin_Operators' = @{ RoleCapabilities = 'Maintenance' }} -Path "$env:ProgramData\JEAConfiguration\Demo.pssc" 
```

Se creará un archivo de configuración de sesión como el siguiente: 
```powershell
@{

# Version number of the schema used for this document
SchemaVersion = '2.0.0.0'

# ID used to uniquely identify this document
GUID = 'a384fdd3-5830-4a2c-ac86-cdd1822c3afd'

# Author of this document
Author = 'Administrator'

# Description of the functionality provided by these settings
# Description = ''

# Session type defaults to apply for this session configuration. Can be 'RestrictedRemoteServer' (recommended), 'Empty', or 'Default'
SessionType = 'RestrictedRemoteServer'

# Directory to place session transcripts for this session configuration
TranscriptDirectory = 'C:\ProgramData\JEATranscripts'

# Whether to run this session configuration as the machine's (virtual) administrator account
RunAsVirtualAccount = $true

# Groups associated with machine's (virtual) administrator account
# RunAsVirtualAccountGroups = 'Remote Desktop Users', 'Remote Management Users'

# Scripts to run when applied to a session
# ScriptsToProcess = 'C:\ConfigData\InitScript1.ps1', 'C:\ConfigData\InitScript2.ps1'

# User roles (security groups), and the Role Capabilities that should be applied to them when applied to a session
RoleDefinitions = @{
    'CONTOSO\NonAdmin_Operators' = @{
        'RoleCapabilities' = 'Maintenance' } }

} 
```
Al crear un punto de conexión de JEA, se deben establecer los parámetros siguientes del comando (y las claves correspondientes en el archivo):
1.  SessionType en RestrictedRemoteServer.
2.  RunAsVirtualAccount en **$true**.
3.  TranscriptPath en el directorio donde se guardarán las transcripciones de "consentimiento temporal" después de cada sesión.
4.  RoleDefinitions en una tabla hash que defina los grupos que tienen acceso a las "funcionalidades de los roles".  Este campo define **quién** puede hacer **qué** en este punto de conexión.   Las funcionalidades de los roles son archivos especiales que explicaremos en breve.


El campo RoleDefinitions define los grupos que tienen acceso a las "funcionalidades de los roles".  Una funcionalidad de rol es un archivo que define un conjunto de funcionalidades que se expondrán a los usuarios que se conecten.  Puede crear funcionalidades de rol con el comando **New-PSRoleCapabilityFile**.

```powershell
New-PSRoleCapabilityFile -Path "$env:ProgramFiles\WindowsPowerShell\Modules\DemoModule\RoleCapabilities\Maintenance.psrc" 
```

Se generará una funcionalidad de rol de plantilla con el siguiente aspecto:
```
@{

# ID used to uniquely identify this document
GUID = '9287a34f-3f0e-4fbe-9dd7-f1361ba9fd65'

# Author of this document
Author = 'Administrator'

# Description of the functionality provided by these settings
# Description = ''

# Company associated with this document
CompanyName = 'Unknown'

# Copyright statement for this document
Copyright = '(c) 2015 Administrator. All rights reserved.'

# Modules to import when applied to a session
# ModulesToImport = 'MyCustomModule', @{ ModuleName = 'MyCustomModule'; ModuleVersion = '1.0.0.0'; GUID = '4d30d5f0-cb16-4898-812d-f20a6c596bdf' }

# Aliases to make visible when applied to a session
# VisibleAliases = 'Item1', 'Item2'

# Cmdlets to make visible when applied to a session
# VisibleCmdlets = 'Invoke-Cmdlet1', @{ Name = 'Invoke-Cmdlet2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }

# Functions to make visible when applied to a session
# VisibleFunctions = 'Invoke-Function1', @{ Name = 'Invoke-Function2'; Parameters = @{ Name = 'Parameter1'; ValidateSet = 'Item1', 'Item2' }, @{ Name = 'Parameter2'; ValidatePattern = 'L*' } }

# External commands (scripts and applications) to make visible when applied to a session
# VisibleExternalCommands = 'Item1', 'Item2'

# Providers to make visible when applied to a session
# VisibleProviders = 'Item1', 'Item2'

# Scripts to run when applied to a session
# ScriptsToProcess = 'C:\ConfigData\InitScript1.ps1', 'C:\ConfigData\InitScript2.ps1'

# Aliases to be defined when applied to a session
# AliasDefinitions = @{ Name = 'Alias1'; Value = 'Invoke-Alias1'}, @{ Name = 'Alias2'; Value = 'Invoke-Alias2'}

# Functions to define when applied to a session
# FunctionDefinitions = @{ Name = 'MyFunction'; ScriptBlock = { param($MyInput) $MyInput } }

# Variables to define when applied to a session
# VariableDefinitions = @{ Name = 'Variable1'; Value = { 'Dynamic' + 'InitialValue' } }, @{ Name = 'Variable2'; Value = 'StaticInitialValue' }

# Environment variables to define when applied to a session
# EnvironmentVariables = @{ Variable1 = 'Value1'; Variable2 = 'Value2' }

# Type files (.ps1xml) to load when applied to a session
# TypesToProcess = 'C:\ConfigData\MyTypes.ps1xml', 'C:\ConfigData\OtherTypes.ps1xml'

# Format files (.ps1xml) to load when applied to a session
# FormatsToProcess = 'C:\ConfigData\MyFormats.ps1xml', 'C:\ConfigData\OtherFormats.ps1xml'

# Assemblies to load when applied to a session
# AssembliesToLoad = 'System.Web', 'System.OtherAssembly, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'

} 

```
Para poder usarse en una configuración de sesión de JEA, las funcionalidades de rol deben guardarse como un módulo de PowerShell válido en un directorio denominado "RoleCapabilities". Un módulo puede tener varios archivos de funcionalidad de rol, si lo desea.

Para empezar a configurar los cmdlets, las funciones, los alias y los scripts a los un usuario puede acceder cuando se conecta a una sesión de JEA, agregue sus propias reglas al archivo de funcionalidad de rol siguiendo las plantillas comentadas. Para obtener una visión más profunda de la configuración de las funcionalidades de rol, consulte la [guía de experiencias](http://aka.ms/JEA) completa.

Por último, cuando termine de personalizar la configuración de la sesión y las funcionalidades de rol relacionadas, ejecute **Register-PSSessionConfiguration** para registrar esta configuración de la sesión y crear el punto de conexión.

```powershell
Register-PSSessionConfiguration -Name Maintenance -Path "C:\ProgramData\JEAConfiguration\Demo.pssc" 
```

## Conectarse a un punto de conexión de JEA
La conexión a un punto de conexión de JEA funciona igual que la conexión a otros puntos de conexión de PowerShell.  Solo tiene que indicar el nombre del punto de conexión de JEA como el parámetro "ConfigurationName" de **New-PSSession**, **Invoke-Command** o **Enter-PSSession**.

```powershell
Enter-PSSession -ConfigurationName Maintenance -ComputerName localhost
```
Después de conectarse a la sesión de JEA, estará limitado a ejecutar la comandos de la lista de permitidos en las funcionalidades de rol a las que tenga acceso. Si intenta ejecutar cualquier comando no permitido para su rol, se producirá un error.<!--HONumber=Mar16_HO2-->
