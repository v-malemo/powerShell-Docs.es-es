# Configuración de un servidor de extracción web de DSC

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

Un servidor de extracción web de DSC es un servicio web en IIS que usa una interfaz de OData para poner a disposición de los nodos de destino los archivos de configuración DSC cuando esos nodos los soliciten.

Requisitos para usar un servidor de extracción:

* Un servidor que ejecute:
  - WMF/PowerShell 4.0 o una versión superior
  - Rol del servidor IIS
  - Servicio DSC
* Idealmente, algún medio para generar un certificado, para proteger las credenciales que se pasan al administrador de configuración local (LCM) en los nodos de destino

Puede agregar el rol del servidor IIS y el servicio DSC con el asistente para agregar roles y características en el administrador del servidor o mediante el uso de PowerShell. Los scripts de ejemplo incluidos en este tema también controlarán estos dos pasos por usted.

## Uso del recurso xWebService
La manera más fácil de configurar un servidor de extracción web es usar el recurso xWebService, incluido en el módulo xPSDesiredStateConfiguration. En los siguientes pasos se explica cómo usar el recurso en una configuración que configure el servicio web.

1. Llame al cmdlet [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) para instalar el módulo **xPSDesiredStateConfiguration**. **Nota**: **Install-Module** está incluido en el módulo **PowerShellGet**, que se incluye en PowerShell 5.0. Puede descargar el módulo **PowerShellGet** para PowerShell 3.0 y 4.0 en [PackageManagement PowerShell Modules Preview](https://www.microsoft.com/en-us/download/details.aspx?id=49186) (Vista previa de los módulos de PowerShell PackageManagement). 
1. Cree un certificado autofirmado con el asunto `"CN=PSDSCPullServerCert"` en el almacén `CERT:\LocalMachine\MY\`. Puede hacerlo con el comando `New-SelfSignedCertificate  -CertStoreLocation 'CERT:\LocalMachine\MY' -DnsName "PSDSCPullServerCert"`.
1. En PowerShell ISE, inicie (F5) el siguiente script de configuración (incluido en la carpeta Example del módulo **xPSDesiredStateConfiguration** como Sample_xDscWebService.ps1). Este script configura el servidor de extracción y un servidor de cumplimiento.
  
```powershell
configuration Sample_xDscWebService 
6 { 
7     param  
8     ( 
9         [string[]]$NodeName = 'localhost', 
10 
 
11         [ValidateNotNullOrEmpty()] 
12         [string] $certificateThumbPrint 
13     ) 
14 
 
15     Import-DSCResource -ModuleName xPSDesiredStateConfiguration 
16 
 
17     Node $NodeName 
18     { 
19         WindowsFeature DSCServiceFeature 
20         { 
21             Ensure = "Present" 
22             Name   = "DSC-Service"             
23         } 
24 
 
25         xDscWebService PSDSCPullServer 
26         { 
27             Ensure                  = "Present" 
28             EndpointName            = "PSDSCPullServer" 
29             Port                    = 8080 
30             PhysicalPath            = "$env:SystemDrive\inetpub\PSDSCPullServer" 
31             CertificateThumbPrint   = $certificateThumbPrint          
32             ModulePath              = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules" 
33             ConfigurationPath       = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration"             
34             State                   = "Started" 
35             DependsOn               = "[WindowsFeature]DSCServiceFeature"                         
36         } 
```

1. Ejecute la configuración, pase la huella digital del certificado autofirmado que creó como el parámetro **certificateThumbPrint**:

```powershell
PS:\>$myCert = Get-ChildItem CERT: | Where-Object {$_.Subject -eq 'CN=PSDSCPullServerCert'}
PS:\>Sample_xDSCService -certificateThumbprint $myCert.Thumbprint 
```

## Clave de registro
Para permitir que los nodos cliente se registren con el servidor de forma que puedan usar nombres de configuración en lugar de un identificador de configuración, una clave de registro (que es un GUID conocido tanto para el servidor como para el nodo cliente) debe colocarse en un archivo denominado `RegistrationKeys.txt`. De forma predeterminada, el servidor de extracción que este ejemplo ha creado espera que ese archivo se encuentre en `C:\Program Files\WindowsPowerShell\DscService`. Cree un archivo de texto con una única línea que especifique la clave de registro y guárdelo en la carpeta.
> **Nota**: No se admiten claves del registro en PowerShell 4.0. 

## Colocación de configuraciones y recursos
Cuando se complete la instalación del servidor de extracción, habrá una nueva carpeta en `$env:PROGRAMFILES\WindowsPowerShell` denominada "DscService". En esa carpeta, hay dos carpetas denominadas "Modules" y "Configuration". En la carpeta "Modules", coloque todos los recursos que sean necesarios para las configuraciones que los nodos extraerán de este servidor. En la carpeta "Configuration", coloque los archivos MOF de configuración de las configuraciones que los nodos vayan a extraer. Los nombres de los archivos MOF dependen del tipo de cliente de extracción. En los temas siguientes se describe en detalle cómo configurar los clientes de extracción:

* [Configuración de un cliente de extracción de DSC mediante un id. de configuración](pullClientConfigID.md)
* [Configuración de un cliente de extracción de DSC mediante nombres de configuración](pullClientConfigNames.md)
* [Configuraciones parciales](partialConfigs.md)

## Creación de la suma de comprobación MOF
Un archivo de configuración MOF debe emparejarse con un archivo de suma de comprobación para que un LCM de un nodo de destino pueda validar la configuración. Para crear una suma de comprobación, llame al cmdlet [New-DSCCheckSum](https://technet.microsoft.com/en-us/library/dn521622.aspx). El cmdlet toma un parámetro **Path** que especifica la carpeta donde se encuentra el MOF de configuración. El cmdlet crea un archivo de suma de comprobación denominado `ConfigurationMOFName.mof.checksum`, donde `ConfigurationMOFName` es el nombre del archivo mof de configuración. Si hay más de un archivo MOF de configuración en la carpeta especificada, se crea una suma de comprobación para cada una de las configuraciones de la carpeta.

El archivo de suma de comprobación debe estar en el mismo directorio que el archivo MOF de configuración (de forma predeterminada, `$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration`) y tener el mismo nombre con la extensión `.checksum` anexada.

>**Nota**: Si cambia el archivo MOF de configuración de cualquier manera, también deberá volver a crear el archivo de suma de comprobación.

## Vea también
* [Información general sobre la configuración de estado deseado de Windows PowerShell](overview.md)
* [Establecer configuraciones](enactingConfigurations.md)
* [Recuperar información del nodo desde el servidor de extracción de DSC](retrieveNodeInfo.md)
<!--HONumber=Feb16_HO4-->
