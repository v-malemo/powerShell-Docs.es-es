---
title: Obtener objetos de WMI (Get-WmiObject)
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f0ddfc7d-6b5e-4832-82de-2283597ea70d
---
# Obtener objetos de WMI (Get-WmiObject)

## Obtener objetos de WMI (Get-WmiObject)
Windows Management Instrumentation (WMI) es una tecnología principal para la administración del sistema de Windows porque expone una amplia gama de información de manera uniforme. Debido a la medida en que WMI lo hace posible, el cmdlet de Windows PowerShell para acceder a objetos WMI, **Get-WmiObject**, es uno de los más útiles para realizar el trabajo real. Vamos a explicar cómo usar Get-WmiObject para acceder a objetos WMI y, a continuación, cómo usar objetos WMI para realizar acciones específicas.

### Enumerar clases WMI
El primer problema que la mayoría de los usuarios de WMI experimentan es intentar averiguar qué se puede hacer con WMI. Las clases WMI describen los recursos que se pueden administrar. Existen cientos de clases WMI, algunas de los cuales contienen decenas de propiedades.

**Get-WmiObject** hace que WMI se pueda detectar para abordar este problema. Para obtener una lista de las clases WMI disponibles en el equipo local, escriba:

```
PS> Get-WmiObject -List

__SecurityRelatedClass                  __NTLMUser9X
__PARAMETERS                            __SystemSecurity
__NotifyStatus                          __ExtendedStatus
Win32_PrivilegesStatus                  Win32_TSNetworkAdapterSettingError
Win32_TSRemoteControlSettingError       Win32_TSEnvironmentSettingError
...
```

Puede recuperar la misma información de un equipo remoto mediante el parámetro ComputerName. Para ello, especifique un nombre de equipo o una dirección IP:

```
PS> Get-WmiObject -List -ComputerName 192.168.1.29

__SystemClass                           __NAMESPACE
__Provider                              __Win32Provider
__ProviderRegistration                  __ObjectProviderRegistration
...
```

La lista de clases que devuelven los equipos remotos puede variar según el sistema operativo específico que el equipo está ejecutando y las extensiones WMI determinadas agregadas por las aplicaciones instaladas.

> [!NOTE]
> Al usar Get-WmiObject para conectarse a un equipo remoto, el equipo remoto debe ejecutar WMI y, en la configuración predeterminada, la cuenta que está usando debe estar en el grupo de administradores local en el equipo remoto. El sistema remoto no necesita tener Windows PowerShell instalado. Esto permite administrar los sistemas operativos que no ejecutan Windows PowerShell, pero tienen WMI disponible.

También puede incluir el parámetro ComputerName al conectarse al sistema local. Puede usar el nombre del equipo local, su dirección IP (o la dirección de bucle invertido 127.0.0.1) o el estilo de WMI '.' como nombre del equipo. Si está ejecutando Windows PowerShell en un equipo denominado Admin01 con la dirección IP 192.168.1.90, los siguientes comandos devolverán la lista de clases WMI de ese equipo:

```
Get-WmiObject -List
Get-WmiObject -List -ComputerName .
Get-WmiObject -List -ComputerName Admin01
Get-WmiObject -List -ComputerName 192.168.1.90
Get-WmiObject -List -ComputerName 127.0.0.1
Get-WmiObject -List -ComputerName localhost
```

Get-WmiObject usa el espacio de nombres root/cimv2 de forma predeterminada. Si desea especificar otro espacio de nombres de WMI, use el parámetro **Namespace** y especifique la ruta de acceso del espacio de nombres correspondiente:

```
PS> Get-WmiObject -List -ComputerName 192.168.1.29 -Namespace root

__SystemClass                           __NAMESPACE
__Provider                              __Win32Provider
...
```

### Visualizar detalles de clases WMI
Si conoce el nombre de una clase WMI, puede usarlo para obtener información inmediatamente. Por ejemplo, una de las clases WMI que se usa habitualmente para recuperar información sobre un equipo es **Win32_OperatingSystem**.

```
PS> Get-WmiObject -Class Win32_OperatingSystem -Namespace root/cimv2 -ComputerName .

SystemDirectory : C:\WINDOWS\system32
Organization    : Global Network Solutions
BuildNumber     : 2600
RegisteredUser  : Oliver W. Jones
SerialNumber    : 12345-678-9012345-67890
Version         : 5.1.2600
```

Aunque vamos a presentar todos los parámetros, el comando se puede expresar de forma más concisa. El parámetro **ComputerName** no es necesario cuando se conecta al sistema local. Los presentamos para demostrar el caso más general y recordarle el parámetro. **Namespace** se establece de manera predeterminada en root/cimv2 y también se puede omitir. Por último, la mayoría de los cmdlets permite omitir el nombre de los parámetros comunes. Con Get-WmiObject, si no se especifica ningún nombre para el primer parámetro, Windows PowerShell lo trata como el parámetro **Class**. Esto significa que el último comando se podría haber emitido escribiendo:

```
Get-WmiObject Win32_OperatingSystem
```

La clase **Win32_OperatingSystem** tiene muchas más propiedades de las que se muestran aquí. Puede usar Get-Member para ver todas las propiedades. Las propiedades de una clase WMI están disponibles automáticamente como otras propiedades de objeto:

```
PS> Get-WmiObject -Class Win32_OperatingSystem -Namespace root/cimv2 -ComputerName . | Get-Member -MemberType Property

   TypeName: System.Management.ManagementObject#root\cimv2\Win32_OperatingSyste
m

Name                                      MemberType Definition
----                                      ---------- ----------
__CLASS                                   Property   System.String __CLASS {...
...
BootDevice                                Property   System.String BootDevic...
BuildNumber                               Property   System.String BuildNumb...
...
```

#### Visualizar propiedades no predeterminadas con cmdlets de formato
Si quiere ver la información incluida en la clase **Win32_OperatingSystem** que no aparece de forma predeterminada, puede mostrarla mediante los cmdlets **Format**. Por ejemplo, si desea mostrar los datos de memoria disponible, escriba:

```
PS> Get-WmiObject -Class Win32_OperatingSystem -Namespace root/cimv2 -ComputerName . | Format-Table -Property TotalVirtualMemorySize,TotalVisibleMemorySize,FreePhysicalMemory,FreeVirtualMemory,FreeSpaceInPagingFiles

TotalVirtualMemorySize TotalVisibleMem FreePhysicalMem FreeVirtualMemo FreeSpaceInPagi
                              ory              ry         ngFiles
--------------- --------------- --------------- --------------- ---------------
        2097024          785904          305808         2056724         1558232
```

> [!NOTE]
> Los caracteres comodín funcionan con los nombres de propiedad de **Format-Table**, por lo que el elemento final de la canalización se puede reducir a **Format-Table -Property TotalV&#42;,Free&#42;**

Los datos de la memoria podrían ser más legibles si se formatean como una lista escribiendo:

```
PS> Get-WmiObject -Class Win32_OperatingSystem -Namespace root/cimv2 -ComputerName . | Format-List TotalVirtualMemorySize,TotalVisibleMemorySize,FreePhysicalMemory,FreeVirtualMemory,FreeSpaceInPagingFiles

TotalVirtualMemorySize : 2097024
TotalVisibleMemorySize : 785904
FreePhysicalMemory     : 301876
FreeVirtualMemory      : 2056724
FreeSpaceInPagingFiles : 1556644
```



<!--HONumber=Apr16_HO1-->


