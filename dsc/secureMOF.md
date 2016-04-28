# Proteger el archivo MOF

>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

DSC indica a los nodos de destino la configuración que deben tener mediante el envío de un archivo MOF con dicha información para cada nodo, donde el administrador de configuración local (LCM) implementa la configuración deseada 
. Como este archivo contiene los detalles de la configuración, es importante que esté protegido. Para ello, puede establecer el LCM para comprobar las credenciales de 
un usuario. En este tema se describe cómo transmitir esas credenciales de forma segura al nodo de destino mediante su cifrado con certificados.

>**Nota:** En este tema se describen los certificados usados para el cifrado. Para el cifrado, un certificado autofirmado es suficiente, porque la clave privada se mantiene siempre secreta y el cifrado no implica la confianza del documento. Los certificados autofirmados
>*no* deben usarse con fines de autenticación. Debe usar un certificado de una entidad de certificación de confianza (CA) para fines de autenticación.

## Requisitos previos

Para cifrar correctamente las credenciales utilizadas para proteger una configuración DSC, asegúrese de que disponer de lo siguiente:

* **Algún medio de emisión y distribución de certificados**. En este tema y en sus ejemplos se supone que usa la entidad de certificación de Active Directory. Para obtener más información general acerca de 
los Servicios de certificados de Active Directory, consulte [Información general de Servicios de certificados de Active Directory](https://technet.microsoft.com/library/hh831740.aspx) y 
[Servicios de certificados de Active Directory en Windows Server 2008](https://technet.microsoft.com/windowsserver/dd448615.aspx).
* **Acceso administrativo a los nodos de destino**.
* **Cada uno de los nodos de destino tiene un certificado de cifrado guardado en su almacén personal**. En Windows PowerShell, la ruta de acceso al almacén es Cert:\LocalMachine\My. En los ejemplos de este tema se usa la 
plantilla de "autenticación de estación de trabajo", que puede encontrar (junto con otras plantillas de certificado) en las [Plantillas de certificado predeterminadas](https://technet.microsoft.com/library/cc740061(v=WS.10).aspx).
* Si va a ejecutar esta configuración en un equipo distinto al nodo de destino, **exporte la clave pública del certificado** y luego impórtela en el equipo desde el que se va a ejecutar 
la configuración. Asegúrese de que solo se exporte la clave **public**; mantenga la clave privada segura.

## Proceso general

 1. Configure los certificados, las claves y las huellas digitales, asegurándose de que cada nodo de destino tenga copias del certificado y de que el equipo de configuración tenga la huella digital y la clave pública.
 2. Cree un bloque de datos de configuración que contenga la ruta de acceso y la huella digital de la clave pública.
 3. Cree un script de configuración que defina la configuración deseada para el nodo de destino y configure el descifrado en los nodos de destino, estableciendo que el administrador de configuración local descifre los datos de configuración con el certificado y su huella digital.
 4. Ejecute la configuración, que establecerá la configuración del administrador de configuración local e inicie la configuración DSC.

![Diagrama1](images/CredentialEncryptionDiagram1.png)

## Requisitos de certificados

Para representar el cifrado de credenciales, se requiere que un certificado de clave pública esté disponible en el _nodo de destino_ en el que **confíe** el equipo usado para crear la configuración de DSC.
Este certificado de clave pública tiene requisitos específicos que debe usar para el cifrado de credenciales de DSC:
 1. **Uso de la clave**:
   - Debe contener: 'KeyEncipherment' y 'DataEncipherment'.
   - _No_ debe contener: 'Digital Signature'.
 2. **Uso mejorado de clave**:
   - Debe contener: cifrado del documento (1.3.6.1.4.1.311.80.1).
   - _No_ debe contener: autenticación de cliente (1.3.6.1.5.5.7.3.2) ni autenticación de servidor (1.3.6.1.5.5.7.3.1).
 3. La clave privada del certificado está disponible en el nodo de destino.
 4. El **proveedor** del certificado debe ser "Proveedor de servicios criptográficos de Microsoft RSA SChannel".
 
>**Procedimiento recomendado:** Aunque puede usar un certificado que contenga un uso de clave 'Digital Signature' o uno de los EKU de autenticación, lo que permitirá que la clave de cifrado 
>se use de forma inadecuada más fácilmente y sea vulnerable a ataques. Por lo tanto, le recomendamos que utilice un certificado creado específicamente con el fin de proteger las credenciales de DSC que omita estos EKU y 
>usos de clave.
  
Cualquier certificado existente en el _nodo de destino_ que cumpla estos criterios puede usarse para proteger las credenciales de DSC.
 
## Creación del certificado

La clave privada debe mantenerse secreta, ya que se usa para descifrar el MOF. La manera más fácil de hacerlo es crear el certificado de clave privada en el *nodo de destino* y copiar el certificado de clave 
pública en el equipo que se usa para compilar la configuración de DSC en un archivo MOF. En el ejemplo siguiente se crea un certificado, se exporta la clave pública y, luego, la clave pública se importa
en la raíz del almacén de certificados local.

```powershell
# create the cert
$cert = New-SelfSignedCertificate -Type DocumentEncryptionCertLegacyCsp -DnsName 'DscEncryptionCert' 
# export the cert’s public key
$cert | Export-Certificate -FilePath "$env:temp\DscPublicKey.cer"  -Force                                                              
# import the cert’s public key as a trusted root certificate authority so that it is trusted
Import-Certificate -FilePath "$env:temp\DscPublicKey.cer" -CertStoreLocation Cert:\LocalMachine\Root > $null
```

Como alternativa, se puede crear el certificado de clave privada en el equipo que se usa para compilar el archivo de configuración de DSC, exportarlo con la clave privada y luego importarlo en el _nodo de destino_. 
Este es el método actual para implementar el cifrado de credenciales de DSC en Nano Server. La clave privada debe mantenerse segura durante el tránsito.

## Datos de configuración

El bloque de datos de configuración define los nodos de destino en los que operará, si se cifrarán las credenciales o no, los medios de cifrado y otra información. Para más información sobre el bloque de datos de configuración, consulte [Separación de los datos de entorno y configuración](configData.md).

Los elementos que se pueden configurar para cada nodo que están relacionados con el cifrado de credenciales son:
* **NodeName**: nombre del nodo de destino para el que se configura el cifrado de credenciales.
* **PsDscAllowPlainTextPassword**: indica si las credenciales sin cifrar se pueden pasar a este nodo. Esto **no se recomienda**.
* **Huella digital**: huella digital del certificado que se usará para descifrar las credenciales en la configuración de DSC en el _nodo de destino_. **Este certificado debe existir en el almacén de certificados de la máquina local en el nodo de destino.**
* **CertificateFile**: archivo de certificado (que solo contiene la clave pública) que debe usarse para cifrar las credenciales para el _nodo de destino_. Debe ser un archivo de certificados con formato X.509 codificado base 64 o DER binario codificado X.509.

En este ejemplo se muestra un bloque de datos de configuración que especifica un nodo de destino en el que se actuará con el nombre targetNode, la ruta de acceso al archivo de certificado de clave pública (denominado targetNode.cer) y la huella digital de la clave pública.

```powershell
$ConfigData= @{ 
    AllNodes = @(     
            @{  
                # The name of the node we are describing 
                NodeName = "targetNode" 

                # The path to the .cer file containing the 
                # public key of the Encryption Certificate 
                # used to encrypt credentials for this node 
                CertificateFile = "C:\publicKeys\targetNode.cer" 

         
                # The thumbprint of the Encryption Certificate 
                # used to decrypt the credentials on target node 
                Thumbprint = "AC23EA3A9E291A75757A556D0B71CBBF8C4F6FD8" 
            }; 
        );    
    }
```

## Script de configuración

En el script de configuración, utilice el parámetro `PsCredential` para garantizar que las credenciales se almacenen durante el menor tiempo posible. Al ejecutar el ejemplo facilitado, DSC pedirá las credenciales y luego cifrará el archivo MOF con el elemento CertificateFile que esté asociado con el nodo de destino en el bloque de datos de configuración. Este ejemplo de código copia un archivo desde un recurso compartido que está protegido para un usuario.

```
configuration CredentialEncryptionExample 
{ 
    param( 
        [Parameter(Mandatory=$true)] 
        [ValidateNotNullorEmpty()] 
        [PsCredential] $credential 
        ) 
    

    Node $AllNodes.NodeName 
    { 
        File exampleFile 
        { 
            SourcePath = "\\Server\share\path\file.ext" 
            DestinationPath = "C:\destinationPath" 
            Credential = $credential 
        } 
    } 
}
```

## Configuración del descifrado

Antes de que [`Start-DscConfiguration`](https://technet.microsoft.com/en-us/library/dn521623.aspx) pueda funcionar, debe indicar al administrador de configuración local en cada nodo de destino qué certificado se usará para descifrar las credenciales, mediante el recurso CertificateID para comprobar la huella digital del certificado. Esta función de ejemplo encontrará el certificado local adecuado (es posible que deba personalizarlo para que encuentre el certificado exacto que quiere utilizar):

```powershell
# Get the certificate that works for encryption 
function Get-LocalEncryptionCertificateThumbprint 
{ 
    (dir Cert:\LocalMachine\my) | %{
        # Verify the certificate is for Encryption and valid 
        if ($_.PrivateKey.KeyExchangeAlgorithm -and $_.Verify()) 
        { 
            return $_.Thumbprint 
        } 
    } 
}
```

Con el certificado identificado mediante su huella digital, el script de configuración se puede actualizar para utilizar el valor:

```powershell
configuration CredentialEncryptionExample 
{ 
    param( 
        [Parameter(Mandatory=$true)] 
        [ValidateNotNullorEmpty()] 
        [PsCredential] $credential 
        ) 
    

    Node $AllNodes.NodeName 
    { 
        File exampleFile 
        { 
            SourcePath = "\\Server\share\path\file.ext" 
            DestinationPath = "C:\destinationPath" 
            Credential = $credential 
        } 
        
        LocalConfigurationManager 
        { 
             CertificateId = $node.Thumbprint 
        } 
    } 
}
```

## Ejecución de la configuración

En este punto, puede ejecutar la configuración, lo que dará como resultado dos archivos:

 * Un archivo *.meta.mof que configura el administrador de configuración local para que descifre las credenciales mediante el certificado que está almacenado en el almacén de la máquina local y se identifique mediante su huella digital. [`Set-DscLocalConfigurationManager`](https://technet.microsoft.com/en-us/library/dn521621.aspx) se aplica al archivo *.meta.mof.
 * Un archivo MOF que aplica realmente la configuración. Start-DscConfiguration aplica la configuración.

Estos comandos llevará a cabo esos pasos:

```powershell
Write-Host "Generate DSC Configuration..."
CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample

Write-Host "Setting up LCM to decrypt credentials..."
Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose 
 
Write-Host "Starting Configuration..."
Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose
```

En este ejemplo se insertaría la configuración de DSC en el nodo de destino.
La configuración de DSC también se puede aplicar mediante un servidor de incorporación de cambios de DSC, si está disponible.

Consulte [esta página](PullClient.md) para obtener más información sobre cómo aplicar configuraciones de DSC mediante un servidor de incorporación de cambios de DSC.

## Ejemplo de módulo de cifrado de credenciales

Este es un ejemplo completo que incorpora todos estos pasos, además de un cmdlet de aplicación auxiliar que exporta y copia las claves públicas:

```powershell
# A simple example of using credentials
configuration CredentialEncryptionExample
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PsCredential] $credential
        )
    

    Node $AllNodes.NodeName
    {
        File exampleFile
        {
            SourcePath = "\\server\share\file.txt"
            DestinationPath = "C:\Users\user"
            Credential = $credential
        }
        
        LocalConfigurationManager
        {
            CertificateId = $node.Thumbprint
        }
    }
}

# A Helper to invoke the configuration, with the correct public key 
# To encrypt the configuration credentials
function Start-CredentialEncryptionExample
{
    [CmdletBinding()]
    param ($computerName)


    [string] $thumbprint = Get-EncryptionCertificate -computerName $computerName -Verbose
    Write-Verbose "using cert: $thumbprint"

    $certificatePath = join-path -Path "$env:SystemDrive\$script:publicKeyFolder" -childPath "$computername.EncryptionCertificate.cer"         

    $ConfigData=    @{
        AllNodes = @(     
                        @{  
                            # The name of the node we are describing
                            NodeName = "$computerName"

                            # The path to the .cer file containing the
                            # public key of the Encryption Certificate
                            CertificateFile = "$certificatePath"

                            # The thumbprint of the Encryption Certificate
                            # used to decrypt the credentials
                            Thumbprint = $thumbprint
                        };
                    );    
    }

    Write-Verbose "Generate DSC Configuration..."
    CredentialEncryptionExample -ConfigurationData $ConfigData -OutputPath .\CredentialEncryptionExample `
        -credential (Get-Credential -UserName "$env:USERDOMAIN\$env:USERNAME" -Message "Enter credentials for configuration") 

    Write-Verbose "Setting up LCM to decrypt credentials..."
    Set-DscLocalConfigurationManager .\CredentialEncryptionExample -Verbose 

    Write-Verbose "Starting Configuration..."
    Start-DscConfiguration .\CredentialEncryptionExample -wait -Verbose

}


#region HelperFunctions

# The folder name for the exported public keys
$script:publicKeyFolder = "publicKeys"

# Get the certificate that works for encryptions
function Get-EncryptionCertificate
{
    [CmdletBinding()]
    param ($computerName)
    $returnValue= Invoke-Command -ComputerName $computerName -ScriptBlock {
            $certificates = dir Cert:\LocalMachine\my

            $certificates | %{
                    # Verify the certificate is for Encryption and valid
                    if ($_.PrivateKey.KeyExchangeAlgorithm -and $_.Verify())
                    {
                        # Create the folder to hold the exported public key
                        $folder= Join-Path -Path $env:SystemDrive\ -ChildPath $using:publicKeyFolder
                        if (! (Test-Path $folder))
                        {
                            md $folder | Out-Null
                        }

                        # Export the public key to a well known location
                        $certPath = Export-Certificate -Cert $_ -FilePath (Join-Path -path $folder -childPath "EncryptionCertificate.cer") 

                        # Return the thumbprint, and exported certificate path
                        return @($_.Thumbprint,$certPath);
                    }
                  }
        }
    Write-Verbose "Identified and exported cert..."
    # Copy the exported certificate locally
    $destinationPath = join-path -Path "$env:SystemDrive\$script:publicKeyFolder" -childPath "$computername.EncryptionCertificate.cer"
    Copy-Item -Path (join-path -path \\$computername -childPath $returnValue[1].FullName.Replace(":","$"))  $destinationPath | Out-Null

    # Return the thumbprint
    return $returnValue[0]
}

Start-CredentialEncryptionExample
```


<!--HONumber=Mar16_HO5-->


