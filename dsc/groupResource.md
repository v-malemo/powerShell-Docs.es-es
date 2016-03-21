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
| NombreDeGrupo| Indica el nombre del grupo para el que quiere garantizar un estado específico.| 
| Credential| Indica las credenciales necesarias para acceder a los recursos remotos. **Nota**: Esta cuenta debe tener los permisos adecuados de Active Directory para agregar todas las cuentas no locales al grupo; en caso contrario, se producirá un error.
| Descripción| Indica la descripción del grupo.| 
| Ensure| Indica si existe el grupo. Establezca esta propiedad en "Absent" para asegurarse de que el grupo no exista. Si se establece en "Present" (el valor predeterminado), se garantiza que el grupo exista.| 
| Miembros| Indica que quiere asegurarse de que estos miembros forman el grupo.| 
| MembersToExclude| Indica los usuarios que quiera garantizar que no sean miembros de este grupo.| 
| MembersToInclude| Indica los usuarios que quiera garantizar que sean miembros del grupo.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"``.| 

## Ejemplo

En el ejemplo siguiente se muestra cómo asegurarse de que no existe un grupo denominado TestGroup. 

```powershell
Group GroupExample
{
    # This will remove TestGroup, if present
    # To create a new group, set Ensure to "Present“
    Ensure = "Absent"
    GroupName = "TestGroup"
}
```
<!--HONumber=Feb16_HO4-->
