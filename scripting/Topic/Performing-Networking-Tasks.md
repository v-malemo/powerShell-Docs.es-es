---
title: Realizar tareas de redes
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a43cc55f-70c1-45c8-9467-eaad0d57e3b5
---
# Realizar tareas de redes
Dado que TCP/IP es el protocolo de red más usado, la mayoría de las tareas de administración de protocolo de red de bajo nivel implican TCP/IP. En esta sección, se usan Windows PowerShell y WMI para realizar estas tareas.

### Enumerar las direcciones IP de un equipo
Para obtener todas las direcciones IP en uso en el equipo local, use el siguiente comando:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName . | Format-Table -Property IPAddress
```

La salida de este comando difiere de la mayoría de las listas de propiedades porque los valores están entre llaves:

<pre>IPAddress
---------
{192.168.1.80}
{192.168.148.1}
{192.168.171.1}
{0.0.0.0}</pre>

Para entender por qué aparecen las llaves, use el cmdlet Get-Member para examinar la propiedad **IPAddress**:

<pre>PS> Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName . | Get-Member -Name IPAddress
TypeName: System.Management.ManagementObject#root\cimv2\Win32_NetworkAdapter
Configuración
Name      MemberType Definition
----      ---------- ----------
IPAddress Property   System.String[] IPAddress {get;}</pre>

La propiedad IPAddress de cada adaptador de red es en realidad una matriz. Las llaves de la definición indican que **IPAddress** no es un valor **System.String**, sino una matriz de valores **System.String**.

### Enumerar datos de configuración de IP
Para mostrar datos detallados de configuración de IP de cada adaptador de red, use el siguiente comando:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName .
```

La visualización predeterminada del objeto de configuración del adaptador de red es un conjunto muy reducido de datos disponibles. Para la inspección en profundidad y la solución de problemas, use Select-Object o un cmdlet de formato, como Format-List, para especificar las propiedades que se mostrarán.

Si no está interesado en las propiedades IPX o WINS, probablemente el caso de una red TCP/IP moderna, puede usar el parámetro ExcludeProperty de Select-Object para ocultar las propiedades con nombres que comiencen por "WINS" o "IPX":

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=TRUE -ComputerName . | Select-Object -Property [a-z]* -ExcludeProperty IPX*,WINS*
```

Este comando devuelve información detallada acerca de DHCP, DNS, el enrutamiento y otras propiedades de configuración de IP secundarias.

### Hacer ping de equipos
Puede hacer ping simplemente en un equipo mediante **Win32_PingStatus**. El comando siguiente hace ping, pero devuelve una salida larga:

```
Get-WmiObject -Class Win32_PingStatus -Filter "Address='127.0.0.1'" -ComputerName .
```

Un formato más útil de información resumida de la presentación de las propiedades Address, ResponseTime y StatusCode, como el que genera el siguiente comando. El parámetro Autosize de Format-Table cambia el tamaño de las columnas de la tabla para que se muestren correctamente en Windows PowerShell.

```
PS> Get-WmiObject -Class Win32_PingStatus -Filter "Address='127.0.0.1'" -ComputerName . | Format-Table -Property Address,ResponseTime,StatusCode -Autosize

Address   ResponseTime StatusCode
-------   ------------ ----------
127.0.0.1            0          0
A status code of 0 indicates a successful ping.
```

Puede usar una matriz para hacer ping a varios equipos con un solo comando. Dado que hay más de una dirección, use **ForEach_OBject** para hacer ping a cada dirección por separado:

```
"127.0.0.1","localhost","research.microsoft.com" | ForEach-Object -Process {Get-WmiObject -Class Win32_PingStatus -Filter ("Address='" + $_ + "'") -ComputerName .} | Select-Object -Property Address,ResponseTime,StatusCode
```

Puede usar el mismo formato de comando para hacer ping a todos los equipos de una subred, como una red privada que use el número de red 192.168.1.0 y una máscara de subred de clase C estándar (255.255.255.0). Solo las direcciones del intervalo de 192.168.1.1 a 192.168.1.254 son direcciones locales legítimas (0 se reserva siempre al número de red y 255 es una dirección de difusión de subred).

Para representar una matriz de los números del 1 al 254 en Windows PowerShell, use la instrucción **1..254.** Para hacer ping a una subred completa se puede generar la matriz y, a continuación, agregar los valores en una dirección parcial en la instrucción ping:

```
1..254| ForEach-Object -Process {Get-WmiObject -Class Win32_PingStatus -Filter ("Address='192.168.1." + $_ + "'") -ComputerName .} | Select-Object -Property Address,ResponseTime,StatusCode
```

Tenga en cuenta que esta técnica para generar un intervalo de direcciones puede usarse también en otras ubicaciones. Puede generar un conjunto completo de direcciones de esta manera:

`$ips = 1..254 | ForEach-Object -Process {"192.168.1." + $_}`

### Recuperar las propiedades del adaptador de red
Anteriormente en esta guía de usuario, mencionamos que podía recuperar las propiedades de configuración general mediante **Win32_NetworkAdapterConfiguration**. Aunque no sea estrictamente información de TCP/IP, la información del adaptador de red, como las direcciones MAC y los tipos de adaptador, puede ser útil para comprender lo que ocurre con un equipo. Para obtener un resumen de esta información, use el siguiente comando:

```
Get-WmiObject -Class Win32_NetworkAdapter -ComputerName .
```

### Asignación del dominio DNS de un adaptador de red
Para asignar el dominio DNS para la resolución de nombres automática, use el método **Win32_NetworkAdapterConfiguration SetDNSDomain**. Dado que el dominio DNS se asigna para cada configuración de adaptador de red de manera independiente, debe usar una instrucción **ForEach-Object** para asignar el dominio a cada adaptador:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=true -ComputerName . | ForEach-Object -Process { $_. SetDNSDomain("fabrikam.com") }
```

La instrucción de filtrado "IPEnabled\=true" es necesaria, ya que, incluso en una red que use solo TCP/IP, varias de las configuraciones de adaptador de red de un equipo no son adaptadores TCP/IP reales; son elementos de software generales que admiten RAS, PPTP, QoS y otros servicios para todos los adaptadores y, por tanto, no tienen una dirección propia.

Puede filtrar el comando mediante el cmdlet **Where-Object**, en lugar de usar el filtro **Get-WmiObject**.

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -ComputerName . | Where-Object -FilterScript {$_.IPEnabled} | ForEach-Object -Process {$_.SetDNSDomain("fabrikam.com")}
```

### Realizar tareas de configuración de DHCP
La modificación de detalles de DHCP implica trabajar con un conjunto de adaptadores de red, igual que en la configuración de DNS. Existen varias acciones distintas que se pueden realizar mediante WMI. Veremos las más comunes.

#### Determinar los adaptadores con DHCP habilitado
Para buscar los adaptadores con DHCP habilitado en un equipo, use el siguiente comando:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "DHCPEnabled=true" -ComputerName .
```

Para excluir los adaptadores con problemas de configuración de IP, puede recuperar solo los adaptadores con IP habilitada:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=true and DHCPEnabled=true" -ComputerName .
```

#### Recuperar propiedades de DHCP
Dado que las propiedades relacionadas con DHCP de un adaptador suelen empezar por "DHCP", puede usar el parámetro Property de Format-Table para mostrar solo esas propiedades:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "DHCPEnabled=true" -ComputerName . | Format-Table -Property DHCP*
```

#### Habilitar DHCP en cada adaptador
Para habilitar DHCP en todos los adaptadores, use el siguiente comando:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter IPEnabled=true -ComputerName . | ForEach-Object -Process {$_.EnableDHCP()}
```

Puede usar la instrucción **Filter** "IPEnabled\=true y DHCPEnabled\=false" para evitar tener que habilitar DHCP si ya está habilitado, aunque la omisión de este paso no provocará errores.

#### Liberar y renovar las concesiones DHCP en adaptadores específicos
La clase **Win32_NetworkAdapterConfiguration** tiene los métodos **ReleaseDHCPLease** y **RenewDHCPLease**. Ambos se usan de la misma manera. En general, use estos métodos si solo necesita liberar o renovar direcciones de un adaptador en una subred específica. La manera más fácil de filtrar adaptadores en una subred es elegir solo las configuraciones de adaptador que usen la puerta de enlace para esa subred. Por ejemplo, el comando siguiente libera todas las concesiones DHCP de los adaptadores en el equipo local que obtienen concesiones DHCP de 192.168.1.254:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=true and DHCPEnabled=true" -ComputerName . | Where-Object -FilterScript {$_.DHCPServer -contains "192.168.1.254"} | ForEach-Object -Process {$_.ReleaseDHCPLease()}
```

El único cambio en la renovación de una concesión DHCP es que se usa el método **RenewDHCPLease** en lugar del método **ReleaseDHCPLease**:

```
Get-WmiObject -Class Win32_NetworkAdapterConfiguration -Filter "IPEnabled=true and DHCPEnabled=true" -ComputerName . | Where-Object -FilterScript {$_.DHCPServer -contains "192.168.1.254"} | ForEach-Object -Process {$_.ReleaseDHCPLease()}
```

> [!NOTE]
> Al usar estos métodos en un equipo remoto, tenga en cuenta que puede perder el acceso al sistema remoto si está conectados a este a través del adaptador con la concesión liberada o renovada.

#### Liberar y renovar las concesiones DHCP en todos los adaptadores
Puede realizar liberaciones o renovaciones de direcciones DHCP en todos los adaptadores mediante los métodos **ReleaseDHCPLeaseAll** y **RenewDHCPLease** de la clase **Win32_NetworkAdapterConfiguration**. Sin embargo, el comando se debe aplicar a la clase WMI, en lugar de a un adaptador determinado, porque liberar y renovar concesiones globalmente se realiza en la clase, no en un adaptador específico.

Puede obtener una referencia a una clase WMI, en lugar de instancias de clase. Para ello, enumere todas las clases WMI y, a continuación, seleccione solo la clase deseada por nombre. Por ejemplo, el siguiente comando devuelve la clase Win32_NetworkAdapterConfiguration:

```
Get-WmiObject -List | Where-Object -FilterScript {$_.Name -eq "Win32_NetworkAdapterConfiguration"}
```

Puede tratar todo el comando como la clase y, después, invocar en este el método **ReleaseDHCPAdapterLease**. En el siguiente comando, los paréntesis que rodean los elementos de la canalización **Get-WmiObject** y **Where-Object** indican a Windows PowerShell que los evalúe primero:

```
( Get-WmiObject -List | Where-Object -FilterScript {$_.Name -eq "Win32_NetworkAdapterConfiguration"} ).ReleaseDHCPLeaseAll()
```

Puede usar el mismo formato de comando para invocar el método **RenewDHCPLeaseAll**:

```
( Get-WmiObject -List | Where-Object -FilterScript {$_.Name -eq "Win32_NetworkAdapterConfiguration"} ).RenewDHCPLeaseAll()
```

### Crear un recurso compartido de red
Para crear un recurso compartido de red, use el método **Win32_Share Create**:

```
(Get-WmiObject -List -ComputerName . | Where-Object -FilterScript {$_.Name -eq "Win32_Share"}).Create("C:\temp","TempShare",0,25,"test share of the temp folder")
```

También puede crear el recurso compartido mediante **net share** en Windows PowerShell:

```
net share tempshare=c:\temp /users:25 /remark:"test share of the temp folder"
```

### Quitar un recurso compartido de red
Puede quitar un recurso compartido de red con **Win32_Share**, pero el proceso es ligeramente diferente del de creación de un recurso compartido, porque debe recuperar el recurso compartido específico que quiere quitar, en lugar de la clase **Win32_Share**. La instrucción siguiente elimina el recurso compartido "TempShare":

```
(Get-WmiObject -Class Win32_Share -ComputerName . -Filter "Name='TempShare'").Delete()
```

**Net share** también funciona:

```
PS> net share tempshare /delete
tempshare was deleted successfully.
```

### Conectar una unidad de red accesible de Windows
El cmdlet **New-PSDrive** crea una unidad de Windows PowerShell, pero las unidades creadas de esta manera solo están disponibles para Windows PowerShell. Para crear una nueva unidad en red, puede usar el objeto COM **WScript.Network**. El siguiente comando asigna el recurso compartido \\FPS01\users a la unidad local B:

```
(New-Object -ComObject WScript.Network).MapNetworkDrive("B:", "\\FPS01\users")
```

El comando **net use** también funciona:

```
net use B: \\FPS01\users
```

Las unidades asignadas con **WScript.Network** o net use están disponibles inmediatamente para Windows PowerShell.



<!--HONumber=Apr16_HO1-->


