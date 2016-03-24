# Cmdlets de sintaxis de mensajes de cifrado (CMS)

Los cmdlets de sintaxis de mensajes de cifrado admiten el cifrado y descifrado de contenido mediante el formato estándar IETF para proteger los mensajes de forma criptográfica según se documenta en [RFC5652](http://tools.ietf.org/html/rfc5652).

```powershell
Get-CmsMessage [-Content] <string>
Get-CmsMessage [-Path] <string>
Get-CmsMessage [-LiteralPath] <string>
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-Content] <string> [[-OutFile] <string>]
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-Path] <string> [[-OutFile] <string>]
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-LiteralPath] <string> [[-OutFile] <string>]
Unprotect-CmsMessage [-EventLogRecord] <EventLogRecord> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
Unprotect-CmsMessage [-Content] <string> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
Unprotect-CmsMessage [-Path] <string> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
Unprotect-CmsMessage [-LiteralPath] <string> [[-To] <CmsMessageRecipient[]>] [-IncludeContext]
```

El estándar de cifrado de CMS implementa la criptografía de clave pública, donde las claves usadas para cifrar contenido (la *clave pública*) y las claves usadas para descifrar contenido (la *clave privada*) son independientes.

La clave pública se puede compartir y no se considera información confidencial. Si cualquier contenido está cifrado con esta clave pública, solo su clave privada puede descifrarlo. Para obtener más información acerca de la criptografía de clave pública, consulte: <http://en.wikipedia.org/wiki/Public-key_cryptography>.

Para que puedan reconocerse en PowerShell, los certificados de cifrado requieren un identificador de uso de claves único (EKU) para identificarlos como certificados de cifrado de datos (por ejemplo, los identificadores de "Firma de código" y "Correo cifrado").

Este es un ejemplo de cómo crear un certificado adecuado para el cifrado del documento:

```powershell
(Change the text in **Subject** to your name, email, or other identifier), and put in a file (i.e.: DocumentEncryption.inf):
[Version]
Signature = "$Windows NT$"
[Strings]
szOID\_ENHANCED\_KEY\_USAGE = "2.5.29.37"
szOID\_DOCUMENT\_ENCRYPTION = "1.3.6.1.4.1.311.80.1"
[NewRequest]
Subject = "<cn=me@somewhere.com>"
MachineKeySet = false
KeyLength = 2048
KeySpec = AT\_KEYEXCHANGE
HashAlgorithm = Sha1
Exportable = true
RequestType = Cert
KeyUsage = "CERT\_KEY\_ENCIPHERMENT\_KEY\_USAGE | CERT\_DATA\_ENCIPHERMENT\_KEY\_USAGE"
ValidityPeriod = "Years"
ValidityPeriodUnits = "1000"
[Extensions]
%szOID\_ENHANCED\_KEY\_USAGE% = "{text}%szOID\_DOCUMENT\_ENCRYPTION%"
```

A continuación, ejecute:
```powershell
certreq -new DocumentEncryption.inf DocumentEncryption.cer
```

Ahora puede cifrar y descifrar el contenido:

```powershell
$protected = "Hello World" | Protect-CmsMessage -To "\*me@somewhere.com\*[](mailto:*leeholm@microsoft.com*)"
$protected
-----BEGIN CMS-----
MIIBqAYJKoZIhvcNAQcDoIIBmTCCAZUCAQAxggFQMIIBTAIBADA0MCAxHjAcBgNVBAMMFWxlZWhv
bG1AbWljcm9zb2Z0LmNvbQIQQYHsbcXnjIJCtH+OhGmc1DANBgkqhkiG9w0BAQcwAASCAQAnkFHM
proJnFy4geFGfyNmxH3yeoPvwEYzdnsoVqqDPAd8D3wao77z7OhJEXwz9GeFLnxD6djKV/tF4PxR
E27aduKSLbnxfpf/sepZ4fUkuGibnwWFrxGE3B1G26MCenHWjYQiqv+Nq32Gc97qEAERrhLv6S4R
G+2dJEnesW8A+z9QPo+DwYU5FzD0Td0ExrkswVckpLNR6j17Yaags3ltNVmbdEXekhi6Psf2MLMP
TSO79lv2L0KeXFGuPOrdzPAwCkV0vNEqTEBeDnZGrjv/5766bM3GW34FXApod9u+VSFpBnqVOCBA
DVDraA6k+xwBt66cV84OHLkh0kT02SIHMDwGCSqGSIb3DQEHATAdBglghkgBZQMEASoEEJbJaiRl
KMnBoD1dkb/FzSWAEBaL8xkFwCu0e1ZtDj7nSJc=
-----END CMS-----

$protected | Unprotect-CmsMessage
Hello World
```

Cualquier parámetro de tipo **CMSMessageRecipient** admite identificadores en los siguientes formatos:
- Un certificado real (tal y como se recuperó del proveedor de certificados).
- Ruta de acceso al archivo que contiene el certificado.
- Ruta de acceso a un directorio que contiene el certificado.
- Huella digital del certificado (usada para buscar en el almacén de certificados).
- Nombre de asunto del certificado (usado para buscar en el almacén de certificados).

Para ver los certificados de cifrado del documento en el proveedor de certificados, puede usar el parámetro dinámico **-DocumentEncryptionCert**:

```powershell
dir -DocumentEncryptionCert
```<!--HONumber=Mar16_HO2-->
