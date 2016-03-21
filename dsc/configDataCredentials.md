# Opciones de credenciales en los datos de configuración
>Se aplica a: Windows PowerShell 5.0

## Contraseñas de texto sin formato y usuarios del dominio

Las configuraciones DSC que contienen una credencial sin cifrado generarán mensajes de error sobre contraseñas de texto sin formato.
Además, DSC generará una advertencia cuando se usen credenciales de dominio.
Para suprimir estos mensajes de advertencia y de error, utilice las palabras clave de datos de configuración DSC:
* **PsDscAllowPlainTextPassword**
* **PsDscAllowDomainUser**

## Control de credenciales en DSC

Los recursos de configuración DSC se ejecutan como `Local System` de forma predeterminada.
Sin embargo, algunos recursos necesitan una credencial, por ejemplo cuando el recurso `Package` necesita instalar software en una cuenta de usuario concreta.

Los recursos anteriores utilizaban un nombre de propiedad `Credential` codificado de forma rígida para controlar esto.
WMF 5.0 agregó una propiedad `PsDscRunAsCredential` automática para todos los recursos.
Los recursos más recientes y los recursos personalizados pueden utilizar esta propiedad automática en lugar de crear su propia propiedad para las credenciales.

*Tenga en cuenta que el diseño de algunos recursos implica que se van a usar varias credenciales para un motivo concreto y tendrán sus propias propiedades de credencial.*

Para encontrar las propiedades de credenciales disponibles en un recurso, use `Get-DscResource -Name ResourceName -Syntax` o Intellisense en el ISE (`CTRL+SPACE`).

```PowerShell
PS C:\> Get-DscResource -Name Group -Syntax
Group [String] #ResourceName
{
    GroupName = [string]
    [Credential = [PSCredential]]
    [DependsOn = [string[]]]
    [Description = [string]]
    [Ensure = [string]{ Absent | Present }]
    [Members = [string[]]]
    [MembersToExclude = [string[]]]
    [MembersToInclude = [string[]]]
    [PsDscRunAsCredential = [PSCredential]]
}
```

En este ejemplo se utiliza un recurso [Group](https://msdn.microsoft.com/en-us/powershell/dsc/groupresource) del módulo de recursos integrado de DSC `PSDesiredStateConfiguration`.
Puede crear grupos locales y agregar o quitar miembros.
Acepta tanto la propiedad `Credential` como la propiedad automática `PsDscRunAsCredential`.
No obstante, el recurso solo utiliza la propiedad `Credential`.
Obtenga más información sobre `PsDscRunAsCredential` en las [notas de la versión de WMF](https://msdn.microsoft.com/en-us/powershell/wmf/dsc_runas).

## Ejemplo: la propiedad Credential del recurso Group

DSC se ejecuta en `Local System`, por lo que ya tiene permisos para cambiar grupos y usuarios locales.
Si el miembro agregado es una cuenta local, no se necesitan credenciales.
Si el recurso `Group` agrega una cuenta de dominio al grupo local, es necesaria una credencial.

No se permiten las consultas anónimas a Active Directory.
La propiedad `Credential` del recurso `Group` es la cuenta de dominio que se utiliza para consultar a Active Directory.
En la mayoría de casos esto podría deberse a una cuenta de usuario genérica ya que, de forma predeterminada, los usuarios pueden *leer* la mayoría de objetos de Active Directory.

## Configuración de ejemplo

En el ejemplo de código siguiente se utiliza DSC para rellenar un grupo local con un usuario de dominio:

```PowerShell
Configuration DomainCredentialExample
{
    param
    (
        [PSCredential] $DomainCredential
    )
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $DomainCredential
        }
    }
}

$cred = Get-Credential -UserName contoso\genericuser -Message "Password please"
DomainCredentialExample -DomainCredential $cred
```

Este código genera un error y un mensaje de advertencia:

```
ConvertTo-MOFInstance : System.InvalidOperationException error processing
property 'Credential' OF TYPE 'Group': Converting and storing encrypted
passwords as plain text is not recommended. For more information on securing
credentials in MOF file, please refer to MSDN blog:
http://go.microsoft.com/fwlink/?LinkId=393729

At line:11 char:9
+   Group
At line:297 char:16
+     $aliasId = ConvertTo-MOFInstance $keywordName $canonicalizedValue
+                ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Write-Error], InvalidOperationException
    + FullyQualifiedErrorId : FailToProcessProperty,ConvertTo-MOFInstance

WARNING: It is not recommended to use domain credential for node 'localhost'.
In order to suppress the warning, you can add a property named
'PSDscAllowDomainUser' with a value of $true to your DSC configuration data
for node 'localhost'.
```

Este ejemplo tiene dos problemas:
1.  Un error explica que no se recomiendan las contraseñas de texto sin formato.
2.  Una advertencia recomienda que no se use una credencial de dominio.

## PsDscAllowPlainTextPassword

El primer mensaje de error tiene una dirección URL con documentación.
En este vínculo se explica cómo cifrar contraseñas con una estructura [ConfigurationData](https://msdn.microsoft.com/en-us/powershell/dsc/configdata) y un certificado.
Para más información sobre certificados y DSC, [lea esta publicación](http://aka.ms/certs4dsc).

Para forzar una contraseña de texto sin formato, el recurso requiere la palabra clave `PsDscAllowPlainTextPassword` en la sección de datos de configuración, como se indica a continuación:

```PowerShell
Configuration DomainCredentialExample
{
    param
    (
        [PSCredential] $DomainCredential
    )
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    node localhost
    {
        Group DomainUserToLocalGroup
        {
            GroupName        = 'ApplicationAdmins'
            MembersToInclude = 'contoso\alice'
            Credential       = $DomainCredential
        }
    }
}

$cd = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            PSDscAllowPlainTextPassword = $true
        }
    )
}

$cred = Get-Credential -UserName contoso\genericuser -Message "Password please"
DomainCredentialExample -DomainCredential $cred -ConfigurationData $cd
```

*Tenga en cuenta que `NodeName` no puede ser igual a asterisco, es obligatorio un nombre de nodo específico.*

**Microsoft aconseja evitar las contraseñas de texto sin formato por sus riesgos de seguridad considerables.**

## Credenciales de dominio

Si se ejecuta de nuevo el script de configuración de ejemplo (con o sin cifrado), se sigue generando la advertencia que indica que no se recomienda el uso de una cuenta de dominio para una credencial.
Si se utiliza una cuenta local, se elimina la posible exposición de credenciales de dominio podría usarse en otros servidores.

**Cuando se usan credenciales con recursos de DSC, siempre que sea posible es preferible usar una cuenta local en lugar de una cuenta de dominio.**

Si hay un carácter '\' o '@' en la propiedad `Username` de la credencial, DSC lo tratará como una cuenta de dominio.
Existen excepciones para "localhost", "127.0.0.1" y "::1" en la parte del dominio del nombre de usuario.

## PSDscAllowDomainUser

En el ejemplo del recurso `Group` de DSC anterior, la consulta a un dominio de Active Directory *requiere* una cuenta de dominio.
En este caso, agregue la propiedad `PSDscAllowDomainUser` al bloque `ConfigurationData` como se indica a continuación:

```PowerShell
$cd = @{
    AllNodes = @(
        @{
            NodeName = 'localhost'
            PSDscAllowDomainUser = $true
            # PSDscAllowPlainTextPassword = $true
            CertificateFile = "C:\PublicKeys\server1.cer"
        }
    )
}
```

Ahora, el script de configuración generará el archivo MOF sin errores ni advertencias.
<!--HONumber=Feb16_HO4-->
