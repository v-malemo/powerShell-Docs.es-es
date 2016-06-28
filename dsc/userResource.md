---
title: Recurso de DSC User
ms.date: 2016-05-16
keywords: powershell,DSC
description: 
ms.topic: article
author: eslesar
manager: dongill
ms.prod: powershell
translationtype: Human Translation
ms.sourcegitcommit: 6477ae8575c83fc24150f9502515ff5b82bc8198
ms.openlocfilehash: 5c7878bdfc8a3f118b569a9e43be6c7e4333ad2c

---

#Recurso de DSC User#

 
>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0


El recurso __User__ de la configuración de estado deseado (DSC) de Windows PowerShell ofrece un mecanismo para administrar cuentas de usuarios locales en el nodo de destino.


##Sintaxis##

```
User [string] #ResourceName
{
    UserName = [string]
    [ Description = [string] ]
    [ Disabled = [bool] ]
    [ Ensure = [string] { Absent | Present }  ]
    [ FullName = [string] ]
    [ Password = [PSCredential] ]
    [ PasswordChangeNotAllowed = [bool] ]
    [ PasswordChangeRequired = [bool] ]
    [ PasswordNeverExpires = [bool] ]
    [ DependsOn = [string[]] ]
}
```

## Propiedades
|  Propiedad  |  Descripción   | 
|---|---| 
| UserName| Indica el nombre de la cuenta para la que quiere garantizar un estado específico.| 
| Descripción| Indica la descripción que se quiere utilizar para la cuenta de usuario.| 
| Deshabilitada| Indica si la cuenta se encuentra habilitada. Establezca esta propiedad en __$true__ para asegurarse de que esta cuenta esté deshabilitada y establézcala como __$false__ para asegurarse de que esté habilitada.| 
| Ensure| Indica si existe la cuenta. Establezca esta propiedad en "Present" para asegurarse de que la cuenta exista y establézcala como "Absent" para asegurarse de que la cuenta no exista.| 
| FullName| Representa una cadena con el nombre completo que quiere utilizar para la cuenta de usuario.| 
| Contraseña| Indica la contraseña que quiere usar para esta cuenta. | 
| PasswordChangeNotAllowed| Indica si el usuario puede cambiar la contraseña. Establezca esta propiedad en __$true__ para asegurarse de que el usuario no pueda cambiar la contraseña y establézcala como __$false__ para permitir al usuario cambiar la contraseña. El valor predeterminado es __$false__.| 
| PasswordChangeRequired| Indica si el usuario debe cambiar la contraseña en el próximo inicio de sesión. Establezca esta propiedad en __$true__ si el usuario debe cambiar la contraseña. El valor predeterminado es __$true__.| 
| PasswordNeverExpires| Indica si la contraseña expirará. Para asegurarse de que la contraseña para esta cuenta no expire nunca, establezca esta propiedad en __$true__; establézcala en __$false__ si la contraseña expirará. El valor predeterminado es __$false__.| 
| DependsOn | Indica que la configuración de otro recurso debe ejecutarse antes de que se configure este recurso. Por ejemplo, si el elemento ID del bloque del script de configuración del recurso que quiere ejecutar primero es __ResourceName__ y su tipo es __ResourceType__, la sintaxis para usar esta propiedad es `DependsOn = "[ResourceType]ResourceName"`.| 

## Ejemplo

```powershell
User UserExample
{
    Ensure = "Present"  # To ensure the user account does not exist, set Ensure to "Absent"
    UserName = "SomeName"
    Password = $passwordCred # This needs to be a credential object
    DependsOn = “[Group]GroupExample" # Configures GroupExample first
}
```




<!--HONumber=Jun16_HO4-->


