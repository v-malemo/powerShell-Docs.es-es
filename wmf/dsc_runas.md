# Compatibilidad automática con RunAs para recursos de DSC
Compatibilidad con la credencial RunAs de DSC
--------------------------------

La versión de WMF 5.0 de abril de 2015 incluye compatibilidad para ejecutar **cualquier** recurso de DSC en un conjunto de credenciales especificado mediante la propiedad PsDscRunAsCredential. Permite escenarios como la instalación de paquetes MSI en un contexto de usuario específico, el acceso al subárbol del Registro de un usuario, el acceso al directorio local específico de un usuario, el acceso a un recurso compartido de red, etc.

A continuación se muestra un ejemplo del uso de la propiedad PsDscRunAsCredential de DSC para cambiar el color de fondo del símbolo del sistema de un usuario.

Configurar ChangeCmdBackGroundColor

{

    Node ("localhost")

    {

        Registry CmdPath

        {

            Key = "HKEY\_CURRENT\_USER\\\\Software\\Microsoft\\\\Command Processor"

            ValueName = "DefaultColor"

            ValueData = '1F'

            ValueType = "DWORD"

            Ensure = "Present"

            Force = $true

            Hex = $true

            PsDscRunAsCredential = get-credential

        }

    }

}

$configData = @{

AllNodes = @(

@{

NodeName="localhost";

CertificateFile = "C:\\publicKeys\\targetNode.cer"

}

)

}

ChangeCmdBackGroundColor -ConfigurationData $configData

## Problemas conocidos

En esta versión, los siguientes son problemas conocidos de la característica de credencial RunAs de DSC:

-   El recurso WindowsProcess no es compatible con la credencial RunAs.

<!--HONumber=Mar16_HO2-->
