---
title:   Recurso de DSC Group
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Recurso de DSC Group

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

El recurso Group de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para administrar grupos locales en el nodo de destino.

##Sintaxis##
```
Group [string] #ResourceName
{
    GroupName = [string]
    [ Credential = [PSCredential] ]
    [ Description = [string[]] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ Members = [string[]] ]
    [ MembersToExclude = [string[]] ]
    [ MembersToInclude = [string[]] ]
    [ DependsOn = [string[]] ]
}
```

## Propiedades

|  Propiedad  |  Descripción   | 
|---|---| 
| NombreDeGrupo| El nombre del grupo para el que quiere garantizar un estado específico.| 
| Credential| Las credenciales necesarias para acceder a los recursos remotos. **Nota**: Esta cuenta debe tener los permisos adecuados de Active Directory para agregar todas las cuentas no locales al grupo; en caso contrario, se producirá un error.
| Descripción| La descripción del grupo.| 
| Ensure| Indica si existe el grupo. Establezca esta propiedad en "Absent" para asegurarse de que el grupo no exista. Si se establece en "Present" (el valor predeterminado), se garantiza que el grupo exista.| 
| Miembros| Use esta propiedad para reemplazar la pertenencia al grupo actual con los miembros especificados. El valor de esta propiedad es una matriz de cadenas del formulario *Domain*\\*UserName*. Si establece esta propiedad en una configuración, no use las propiedades **MembersToExclude** ni **MembersToInclude**. Si lo hace, se generará un error. Establezca el valor de esta propiedad en una cadena vacía para quitar a todos los miembros del grupo.| 
| MembersToExclude| Use esta propiedad para quitar a los miembros de la pertenencia existente del grupo. El valor de esta propiedad es una matriz de cadenas del formulario *Domain*\\*UserName*. Si establece esta propiedad en una configuración, no use la propiedad **Members**. Si lo hace, se generará un error.| 
| MembersToInclude| Use esta propiedad para agregar miembros a la pertenencia existente al grupo. El valor de esta propiedad es una matriz de cadenas del formulario *Domain*\\*UserName*. Si establece esta propiedad en una configuración, no use la propiedad **Members**. Si lo hace, se generará un error.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"``.| 

## Ejemplo 1

En el ejemplo siguiente se muestra cómo asegurarse de que no existe un grupo denominado "TestGroup". 

```powershell
Group GroupExample
{
    # This will remove TestGroup, if present
    # To create a new group, set Ensure to "Present“
    Ensure = "Absent"
    GroupName = "TestGroup"
}
```
## Ejemplo 2
En el ejemplo siguiente se muestra cómo agregar un usuario de Active Directory al grupo de administradores local como parte de una compilación de prácticas en varias máquinas que ya usan un PSCredential para la cuenta de administrador local. Puesto que también se usa para la cuenta de administrador del dominio (después de la promoción del dominio), necesitamos convertir este PSCredential existente en una credencial apta para dominio para poder agregar un usuario de dominio al grupo de administradores local en el servidor miembro.

```powershell
@{
    AllNodes = @(
        @{
            NodeName = '*';
            DomainName = 'SubTest.contoso.com';
         }
     @{
            NodeName = 'Box2';
            AdminAccount = 'Admin-Dave_Alexanderson'   
      }    
    )
}
                  
$domain = $node.DomainName.split('.')[0]
$DCredential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList ("$domain\$($credential.Username)", $Credential.Password)

Group AddADUserToLocalAdminGroup
        {
            GroupName='Administrators'   
            Ensure= 'Present'             
            MembersToInclude= "$domain\$($Node.AdminAccount)"
            Credential = $dCredential    
            PsDscRunAsCredential = $DCredential
        }
```



<!--HONumber=May16_HO3-->


