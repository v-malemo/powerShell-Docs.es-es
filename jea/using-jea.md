---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: uso de JEA
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 3bac5932c3ed57713bdb08e3a9ed435b228518bc

---

# Uso de JEA
Esta sección se centra en la comprensión de la experiencia del *uso de JEA* por parte del usuario final.
En la sección Requisitos previos, creó un punto de conexión de JEA de demostración.
Usaremos esta demostración para mostrar JEA en acción.
En secciones posteriores, la guía funcionará con versiones anteriores, es decir, introducirá acciones y archivos que hicieron posible la experiencia del usuario final.

## Uso de JEA como no administrador
Para mostrar JEA en acción, debe usar Comunicación remota de PowerShell como si fuera un usuario sin privilegios de administrador.
Ejecute el comando siguiente en una nueva ventana de PowerShell:   

```PowerShell
$NonAdminCred = Get-Credential
```

Escriba las credenciales de la cuenta que no es de administrador cuando se le solicite.
Si ha seguido la sección [Configurar usuarios y grupos](creating-a-domain-controller.md#set-up-users-and-groups), serán:
-   Nombre de usuario = "OperatorUser"
-   Contraseña = "pa$$w0rd"

Después, ejecute el comando siguiente para conectarse al punto de conexión de demostración mediante las credenciales proporcionadas:

```PowerShell
Enter-PSSession -ComputerName . -ConfigurationName JEA_Demo -Credential $NonAdminCred
```

Ahora se encuentra en una sesión remota interactiva de PowerShell en la máquina local.
Mediante el parámetro "Credential", se ha conectado *como si fuera* OperatorUser (o la cuenta que usase).
El cambio en el símbolo del sistema a `[localhost]: PS>` indica que está trabajando en una sesión remota.  

Ejecute lo siguiente en el símbolo del sistema remoto para mostrar los comandos disponibles:

```PowerShell
Get-Command
```

Como puede ver, se trata de un subconjunto muy limitado de los comandos disponibles en una ventana normal de PowerShell (que a menudo puede incluir varios miles comandos).
En concreto, solo muestra los 7 cmdlets de JEA predeterminados (Clear-Host, Exit-PSSession, Get-Command, Get-FormatData, Get-Help, Measure-Object, Out-Default, Select-Object) y los dos comandos incluidos explícitamente en el archivo de funcionalidad de rol de mantenimiento.

Ahora echaremos un vistazo al contexto de usuario en el que opera esta sesión. Para ello, invocaremos la función personalizada que se incluye en el archivo de funcionalidad de rol de mantenimiento:

```PowerShell
Get-UserInfo
```

La salida de esta función muestra "ConnectedUser" (usuario conectado) y "RunAsUser" (usuario de ejecución).
El usuario conectado es la cuenta que se conectó a la sesión remota (por ejemplo, su cuenta).
No es necesario que el usuario conectado tenga privilegios de administrador.
La cuenta de ejecución es la cuenta que realmente realiza las acciones con privilegios.
El proceso de conectarse como un usuario y realizar la ejecución como un usuario con privilegios permite que los usuarios sin privilegios realicen tareas administrativas específicas sin concederles derechos administrativos.

Para demostrarlo en la práctica, ejecute el siguiente comando:

```PowerShell
Restart-Service -Name Spooler -Verbose
```

Normalmente, Restart-Service requiere que se ejecuten privilegios de administrador.
En cambio, con la cuenta virtual de JEA, podemos ejecutarlo con credenciales sin privilegios.

De este modo, JEA le permite realizar su trabajo con los comandos que ya usa.
Pero ¿qué sucede con los comandos que *no debería* poder usar?
Intente ejecutar un comando diferente en la sesión de JEA, como `Restart-Computer`. Observe que JEA impide que se ejecute este tipo de comandos.

```PowerShell
[localhost]: PS> Restart-Computer
The term 'Restart-Computer' is not recognized as the name of a cmdlet, function, script file, or
operable program. Check the spelling of the name, or if a path was included, verify that the path
is correct and try again.
    + CategoryInfo          : ObjectNotFound: (Restart-Computer:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
```

Por último, para salir del punto de conexión restringido de JEA, ejecute el siguiente comando:

```PowerShell
Exit-PSSession
```

Esto lo desconecta de la sesión remota de PowerShell.




<!--HONumber=Jun16_HO4-->


