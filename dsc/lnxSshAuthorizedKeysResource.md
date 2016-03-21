# Recurso de DSC para Linux nxSshAuthorizedKeys

El recurso **nxAuthorizedKeys** de la configuración de estado de deseado (DSC) de PowerShell ofrece un mecanismo para administrar claves ssh autorizadas para un usuario especificado.

## Sintaxis

```
nxAuthorizedKeys <string> #ResourceName
{
    KeyComment = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ Username = <string> ]
    [ Key = <string> ]
    [ DependsOn = <string[]> ]

}
```

## Propiedades

|  Propiedad |  Descripción | 
|---|---|
| KeyComment| Un comentario único para la clave. Se utiliza para identificar las claves de forma única.| 
| Ensure| Especifica si la clave está definida. Establezca esta propiedad en "Absent" para asegurarse de que la clave no exista en el archivo de claves autorizadas del usuario. Establézcala en "Present" para asegurarse de que la clave esté definida en el archivo de claves autorizadas del usuario.| 
| Nombre de usuario| El nombre de usuario para el que se administrarán las claves ssh autorizadas. Si no se define, el usuario predeterminado es "root".| 
| Clave| El contenido de la clave. Esto es necesario si el valor de **Ensure** se establece en "Present".| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento **ID** del bloque del script de configuración del recurso que quiere ejecutar primero es **ResourceName** y su tipo es **ResourceType**, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 

## Ejemplo

En el ejemplo siguiente se define una clave ssh autorizada pública para el usuario "monuser".

```
Import-DSCResource -Module nx 

Node $node {

nxSshAuthorizedKeys myKey{
   KeyComment = "myKey"
   Ensure = "Present"
   Key = 'ssh-rsa AAAAB3NzaC1yc2EAAAABJQAAAQEA0b+0xSd07QXRifm3FXj7Pn/DblA6QI5VAkDm6OivFzj3U6qGD1VJ6AAxWPCyMl/qhtpRtxZJDu/TxD8AyZNgc8aN2CljN1hOMbBRvH2q5QPf/nCnnJRaGsrxIqZjyZdYo9ZEEzjZUuMDM5HI1LA9B99k/K6PK2Bc1NLivpu7nbtVG2tLOQs+GefsnHuetsRMwo/+c3LtwYm9M0XfkGjYVCLO4CoFuSQpvX6AB3TedUy6NZ0iuxC0kRGg1rIQTwSRcw+McLhslF0drs33fw6tYdzlLBnnzimShMuiDWiT37WqCRovRGYrGCaEFGTG2e0CN8Co8nryXkyWc6NSDNpMzw== rsa-key-20150401'
   UserName = "monuser"
} 
}
```

<!--HONumber=Feb16_HO4-->
