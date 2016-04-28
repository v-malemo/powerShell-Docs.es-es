---
title: Ejecutar comandos remotos
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6938b56-7dc8-44ba-b4d4-cd7b169fd74d
---
# Ejecutar comandos remotos
Puede ejecutar comandos en un equipo o en cientos de ellos usando un solo comando de Windows PowerShell. Windows PowerShell admite la informática remota a través de varias tecnologías, como WS-Management, RPC y WMI.

## Comunicación remota sin configuración
Muchos de los cmdlets de Windows PowerShell tienen un parámetro ComputerName que les permite recopilar datos y cambiar la configuración de uno o más equipos remotos. Usan distintas tecnologías de comunicación y muchos de ellos funcionan con todos los sistemas operativos de Windows que Windows PowerShell admite sin necesidad de ninguna configuración especial.

Estos cmdlets son:

-   [Restart-Computer](assetId:///bd52bcf6-80ee-4866-9320-04ee1d1dca4a)

-   [Test-Connection](assetId:///87d293e5-10e2-489b-b0a9-922d77c05f3f)

-   [Clear-EventLog](assetId:///05d0de31-3c9d-4cd6-8e1a-dac19835464c)

-   [Get-EventLog [m2]](assetId:///a4372a60-b7d9-4b1c-a268-aa5240300141)

-   [Get-Hotfix](assetId:///e1ef636f-5170-4675-b564-199d9ef6f101)

-   [Get-Process [m2]](assetId:///27a05dbd-4b69-48a3-8d55-b295f6225f15)

-   [Get-Service [m2]](assetId:///0a09cb22-0a1c-4a79-9851-4e53075f9cf6)

-   [Set-Service [m2]](assetId:///b71e29ed-372b-4e32-a4b7-5eb6216e56c3)

-   [Get-WinEvent](assetId:///e1ef636f-5170-4675-b564-199d9ef6f101)

-   [Get-WmiObject [m2]](assetId:///a4c499fa-deec-4c4b-b3fb-6e195d48a396)

Normalmente, los cmdlets que admiten la comunicación remota sin una configuración especial tienen un parámetro ComputerName y carecen de un parámetro Session. Para encontrar estos cmdlets en la sesión, escriba:

```
get-command | where { $_.parameters.keys -contains "ComputerName" -and $_.parameters.keys -notcontains "Session"}
```

## Comunicación remota de Windows PowerShell
La comunicación remota de Windows PowerShell, que usa el protocolo WS-Management, permite ejecutar cualquier comando de Windows PowerShell en uno o varios equipos remotos. Así, permite establecer conexiones persistentes, iniciar sesiones interactivas de 1:1 y ejecutar scripts en varios equipos.

Para usar la comunicación remota de Windows PowerShell, el equipo remoto debe estar configurado para la administración remota. Para más información y ver instrucciones, consulte [about_Remote_Requirements](assetId:///da213949-134c-4741-b307-81f4492ba1bd).

Después de configurar la comunicación remota de Windows PowerShell, tendrá a su disposición un gran número de estrategias de comunicación remota. En lo que resta de este documento se mencionan solo algunas de ellas. Para más información, consulte [about_Remote](assetId:///9b4a5c87-9162-4adf-bdfe-fbc80b9b8970) y [about_Remote_FAQ](assetId:///e23702fd-9415-4a98-9975-390a4d3adc42).

### Iniciar una sesión interactiva
Para iniciar una sesión interactiva con un único equipo remoto, use el cmdlet [Enter-PSSession](assetId:///f4fd89b4-80e9-434e-bd46-952aa8d40d4c). Por ejemplo, para iniciar una sesión interactiva con el equipo remoto Server01, escriba:

```
enter-pssession Server01
```

El símbolo del sistema cambia para mostrar el nombre del equipo al que está conectado. Desde ese momento, cualquier comando que escriba en el símbolo del sistema se ejecutará en el equipo remoto y los resultados se mostrarán en el equipo local.

Para finalizar la sesión interactiva, escriba:

```
exit-pssession
```

Para más información sobre los cmdlets Enter-PSSession y Exit-PSSession, consulte [Enter-PSSession](assetId:///f4fd89b4-80e9-434e-bd46-952aa8d40d4c) y [Exit-PSSession](assetId:///b6daa1ce-48a5-41a3-ac4b-b64dbe03465d).

### Ejecutar un comando remoto
Para ejecutar cualquier comando en uno o varios equipos remotos, use el cmdlet [Invoke-Command](assetId:///22fd98ba-1874-492e-95a5-c069467b8462). Por ejemplo, para ejecutar un comando [Get-UICulture [m2]](assetId:///99175c2e-e856-4208-970e-3dd2f6bac5b8) en los equipos remotos Server01 y Server02, escriba:

```
invoke-command -computername Server01, Server02 {get-UICulture}
```

El resultado se muestra en su equipo.

```
LCID    Name     DisplayName               PSComputerName
----    ----     -----------               --------------
1033    en-US    English (United States)   server01.corp.fabrikam.com
1033    en-US    English (United States)   server02.corp.fabrikam.com
```

Para más información sobre el cmdlet Invoke-Command, consulte [Invoke-Command](assetId:///22fd98ba-1874-492e-95a5-c069467b8462).

### Ejecutar un script
Para ejecutar un script en uno o varios equipos remotos, use el parámetro FilePath del cmdlet Invoke-Command. El script debe estar en el equipo local o accesible desde este. Los resultados se devuelven en el equipo local.

Por ejemplo, el siguiente comando ejecuta el script DiskCollect.ps1 en los equipos remotos Server01 y Server02.

```
invoke-command -computername Server01, Server02 -filepath c:\Scripts\DiskCollect.ps1
```

Para más información sobre el cmdlet Invoke-Command, consulte [Invoke-Command](assetId:///22fd98ba-1874-492e-95a5-c069467b8462).

### Establecer una conexión persistente
Para ejecutar una serie de comandos relacionados que comparten datos, cree una sesión en el equipo remoto y, a continuación, use el cmdlet Invoke-Command para ejecutar comandos en la sesión que cree. Para crear una sesión remota, use el cmdlet New-PSSession.

Por ejemplo, el comando siguiente crea una sesión remota en el equipo Server01 y otra sesión remota en el equipo Server02. Guarda los objetos de la sesión en la variable $s.

```
$s = new-pssession -computername Server01, Server02
```

Ahora que las sesiones se han establecido, puede ejecutar cualquier comando en ellas. Y, como las sesiones son persistentes, puede recopilar datos en un solo comando y usarlos en un comando posterior.

Por ejemplo, el siguiente comando ejecuta un comando Get-Hotfix en las sesiones de la variable $s y guarda los resultados en la variable $h. La variable $h se crea en cada una de las sesiones en $s, pero no existe en la sesión local.

```
invoke-command -session $s {$h = get-hotfix}
```

Ahora puede usar los datos de la variable $h en los siguientes comandos, como en el siguiente. Los resultados se muestran en el equipo local.

```
invoke-command -session $s {$h | where {$_.installedby -ne "NTAUTHORITY\SYSTEM"
```

### Comunicación remota avanzada
La administración remota de Windows PowerShell empieza aquí. Con los cmdlets que se instalan con Windows PowerShell puede, entre otras muchas cosas, establecer y configurar sesiones remotas desde extremos tanto locales como remotos, crear sesiones personalizadas y restringidas, dejar que los usuarios importen comandos desde una sesión remota que se ejecutan implícitamente en la sesión remota o configurar la seguridad de una sesión remota.

Para facilitar la configuración remota, Windows PowerShell incluye un proveedor WSMan. La unidad WSMAN: que este proveedor crea permite desplazarse por una jerarquía de valores de configuración en el equipo local y en los equipos remotos. Para más información sobre el proveedor WSMan, consulte el tema sobre el [proveedor WSMan](assetId:///66fe1241-e08f-49ca-832f-a84c33ca8735) y el tema [about_WS-Management_Cmdlets](assetId:///6ed3370a-ea10-45a5-9493-696aeace27ed). También puede escribir "get-help wsman" en la consola de Windows PowerShell.

Para más información, consulte [about_Remote_FAQ](assetId:///e23702fd-9415-4a98-9975-390a4d3adc42), [Register-PSSessionConfiguration](assetId:///af68867a-d201-4b19-a1de-594015ed8a25) e [Import-PSSession](assetId:///048c115e-a6fb-4e0d-8cea-c5ca24630c9d). Para obtener ayuda con los errores de comunicación remota, consulte [about_Remote_Troubleshooting](assetId:///2f890148-8578-49ed-85ea-79a489dd6317).

## Consulte también
[about_Remote](assetId:///9b4a5c87-9162-4adf-bdfe-fbc80b9b8970)
[about_Remote_FAQ](assetId:///e23702fd-9415-4a98-9975-390a4d3adc42)
[about_Remote_Requirements](assetId:///da213949-134c-4741-b307-81f4492ba1bd)
[about_Remote_Troubleshooting](assetId:///2f890148-8578-49ed-85ea-79a489dd6317)
[about_PSSessions](assetId:///7a9b4e0e-fa1b-47b0-92f6-6e2995d70acb)
[about_WS-Management_Cmdlets](assetId:///6ed3370a-ea10-45a5-9493-696aeace27ed)
[Invoke-Command](assetId:///22fd98ba-1874-492e-95a5-c069467b8462)
[Import-PSSession](assetId:///048c115e-a6fb-4e0d-8cea-c5ca24630c9d)
[New-PSSession](assetId:///59452f12-a11d-4558-99ea-e6ca6ad5ffd3)
[Register-PSSessionConfiguration](assetId:///af68867a-d201-4b19-a1de-594015ed8a25)
[Proveedor de WSMan](assetId:///66fe1241-e08f-49ca-832f-a84c33ca8735)



<!--HONumber=Apr16_HO1-->


