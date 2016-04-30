# Configuración de un servidor de incorporación de cambios SMB de DSC

>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

Un servidor de incorporación de cambios [SMB](https://technet.microsoft.com/en-us/library/hh831795.aspx) de DSC es un recurso compartido de archivos SMB que pone los archivos de configuración de DSC o los recursos de DSC
a disposición de los nodos de destino cuando estos nodos los solicitan.

Para utilizar un servidor de incorporación de cambios SMB para DSC, debe:
- Configurar un recurso compartido de archivos SMB en un servidor que ejecute PowerShell 4.0 o posterior
- Configurar un cliente que ejecute PowerShell 4.0 o posterior para que se extraiga de ese recurso compartido SMB

## Usar el recurso xSmbShare para crear un recurso compartido de archivos SMB

Hay varias maneras de configurar un recurso compartido de archivos SMB, pero echemos un vistazo a cómo puede hacerlo mediante DSC.

### Instalar el recurso xSmbShare

Llame al cmdlet [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) para instalar el módulo **xSmbShare**
>**Nota**: **Install-Module** está incluido en el módulo **PowerShellGet**, que se incluye en PowerShell 5.0. Puede descargar el módulo **PowerShellGet** para PowerShell 3.0 y 4.0
>en [PackageManagement PowerShell Modules Preview](https://www.microsoft.com/en-us/download/details.aspx?id=49186) (Vista previa de los módulos de PowerShell PackageManagement). **xSmbShare** contiene el recurso de DSC **xSmbShare**, que se puede usar
para crear un recurso compartido de archivos SMB.

### Crear el directorio y el recurso compartido de archivos

La configuración siguiente utiliza el recurso [File](fileResource.md) para crear el directorio para el recurso compartido, y el recurso **xSmbShare** para configurar el recurso compartido SMB:

```powershell
Configuration SmbShare {

Import-DscResource -ModuleName PSDesiredStateConfiguration
Import-DscResource -ModuleName xSmbShare
 
    Node localhost {
 
        File CreateFolder {
 
            DestinationPath = 'C:\DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
 
        }
 
        xSMBShare CreateShare {
 
            Name = 'DscSmbShare'
            Path = 'C:\DscSmbShare'
            FullAccess = 'admininstrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
 
        }
        
    }
 
}
```

La configuración crea el directorio `C:\DscSmbShare` si aún no existe y, a continuación, utiliza ese directorio como un recurso compartido de archivos SMB. El acceso **FullAccess** debería proporcionarse a cualquier
cuenta que necesite escribir al recurso compartido de archivos o eliminarse de él, y el acceso **ReadAccess** debe proporcionarse a cualquier nodo de cliente que obtenga las configuraciones o los recursos de DSC desde el recurso compartido (
esto se debe a que DSC se ejecuta como la cuenta del sistema de forma predeterminada, por lo que tiene el mismo equipo debe tener acceso al recurso compartido).


### Conceder al sistema de archivos acceso al cliente de incorporación de cambios

Al conceder el acceso **ReadAccess** a un nodo de cliente, se permite que ese nodo acceda al recurso compartido SMB, pero no a los archivos o a las carpetas de dentro de ese recurso compartido. Tiene que conceder explícitamente a los nodos de cliente acceso a las
carpetas y subcarpetas del recurso compartido SMB. Podemos hacer esto con DSC al agregarlo con el recurso **cNtfsPermissionEntry**, que se encuentra en el módulo [CNtfsAccessControl](https://www.powershellgallery.com/packages/cNtfsAccessControl/1.2.0)
. La siguiente configuración agrega un bloque **cNtfsPermissionEntry** que concede acceso ReadAndExecute al cliente de incorporación de cambios:

```powershell
Configuration DSCSMB {

Import-DscResource -ModuleName PSDesiredStateConfiguration
Import-DscResource -ModuleName xSmbShare
Import-DscResource -ModuleName cNtfsAccessControl
 
    Node localhost {
 
        File CreateFolder {
 
            DestinationPath = 'DscSmbShare'
            Type = 'Directory'
            Ensure = 'Present'
 
        }
 
        xSMBShare CreateShare {
 
            Name = 'DscSmbShare'
            Path = 'DscSmbShare'
            FullAccess = 'administrator'
            ReadAccess = 'myDomain\Contoso-Server$'
            FolderEnumerationMode = 'AccessBased'
            Ensure = 'Present'
            DependsOn = '[File]CreateFolder'
 
        }

        cNtfsPermissionEntry PermissionSet1 {
            
        Ensure = 'Present'
        Path = 'C:\DSCSMB'
        Principal = 'myDomain\Contoso-Server$'
        AccessControlInformation = @(
            cNtfsAccessControlInformation
            {
                AccessControlType = 'Allow'
                FileSystemRights = 'ReadAndExecute'
                Inheritance = 'ThisFolderSubfoldersAndFiles'
                NoPropagateInherit = $false
            }
        )
        DependsOn = '[File]CreateFolder'
        
        }
 
        
    }
 
}
```

## Colocación de configuraciones y recursos

Guarde todos los archivos MOF de configuración o los recursos de DSC que quiera que los nodos de cliente extraigan en la carpeta del recurso compartido SMB.

Todos los archivos MOF de configuración deben denominarse _ConfigurationID_.mof, donde _ConfigurationID_ es el valor de la propiedad **ConfigurationID** del LCM del nodo de destino. Para obtener más información sobre
la configuración de clientes de incorporación de cambios, consulte [Configurar un cliente de incorporación de cambios mediante un id. de configuración](pullClientConfigID.md).

>**Nota:** Debe usar identificadores de configuración si está utilizando un servidor de incorporación de cambios SMB. Los nombres de configuración no son compatibles con SMB.

Los recursos que necesite el cliente deben colocarse en la carpeta de recursos compartidos SMB como archivos `.zip` almacenados.  

## Creación de la suma de comprobación MOF
Un archivo de configuración MOF debe emparejarse con un archivo de suma de comprobación para que un LCM de un nodo de destino pueda validar la configuración. 
Para crear una suma de comprobación, llame al cmdlet [New-DSCCheckSum](https://technet.microsoft.com/en-us/library/dn521622.aspx). El cmdlet toma un parámetro **Path** que especifica la carpeta 
donde se encuentra el MOF de configuración. El cmdlet crea un archivo de suma de comprobación denominado `ConfigurationMOFName.mof.checksum`, donde `ConfigurationMOFName` es el nombre del archivo mof de configuración. 
Si hay más de un archivo MOF de configuración en la carpeta especificada, se crea una suma de comprobación para cada una de las configuraciones de la carpeta.

El archivo de suma de comprobación debe estar en el mismo directorio que el archivo MOF de configuración (de forma predeterminada, `$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration`) y tener el mismo nombre con la extensión `.checksum` anexada.

>**Nota**: Si cambia el archivo MOF de configuración de cualquier manera, también deberá volver a crear el archivo de suma de comprobación.

## Agradecimientos

Agradecimientos especiales a:

- Mike F. Robbins, cuyas publicaciones sobre el uso de SMB para DSC permitieron informar del contenido de este tema. Su blog es [Mike F Robbins](http://mikefrobbins.com/).
- Serge Nikalaichyk, que creó el módulo **cNtfsAccessControl**. El origen de este módulo se encuentra en https://github.com/SNikalaichyk/cNtfsAccessControl.

## Vea también
- [Información general sobre la configuración de estado deseado de Windows PowerShell](overview.md)
- [Establecer configuraciones](enactingConfigurations.md)
- [Configuración de un cliente de extracción mediante id. de configuración](pullClientConfigID.md)

 

<!--HONumber=Mar16_HO2-->


