---
title:  Ejecutar comandos remotos
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  d6938b56-7dc8-44ba-b4d4-cd7b169fd74d
---

# Ejecutar comandos remotos
Puede ejecutar comandos en un equipo o en cientos de ellos usando un solo comando de Windows PowerShell. Windows PowerShell admite la informática remota a través de varias tecnologías, como WS\-Management, RPC y WMI.

## Comunicación remota sin configuración
Muchos de los cmdlets de Windows PowerShell tienen un parámetro ComputerName que les permite recopilar datos y cambiar la configuración de uno o más equipos remotos. Usan distintas tecnologías de comunicación y muchos de ellos funcionan con todos los sistemas operativos de Windows que Windows PowerShell admite sin necesidad de ninguna configuración especial.

Estos cmdlets son:

-   [Restart-Computer](https://technet.microsoft.com/en-us/library/dd315301.aspx)

-   [Test-Connection](https://technet.microsoft.com/en-us/library/dd315259.aspx)

-   [Clear-EventLog](https://technet.microsoft.com/en-us/library/dd347552.aspx)

-   [Get-EventLog](https://technet.microsoft.com/en-us/library/dd315250.aspx)

-   [Get-Hotfix](https://technet.microsoft.com/en-us/library/e1ef636f-5170-4675-b564-199d9ef6f101)

-   [Get-Process](https://technet.microsoft.com/en-us/library/dd347630.aspx)

-   [Get-Service](https://technet.microsoft.com/en-us/library/dd347591.aspx)

-   [Set-Service](https://technet.microsoft.com/en-us/library/dd315324.aspx)

-   [Get-WinEvent](https://technet.microsoft.com/en-us/library/dd315358.aspx)

-   [Get-WmiObject](https://technet.microsoft.com/en-us/library/dd315295.aspx)

Normalmente, los cmdlets que admiten la comunicación remota sin una configuración especial tienen un parámetro ComputerName y carecen de un parámetro Session. Para encontrar estos cmdlets en la sesión, escriba:

```
get-command | where { $_.parameters.keys -contains "ComputerName" -and $_.parameters.keys -notcontains "Session"}
```

## Comunicación remota de Windows PowerShell
La comunicación remota de Windows PowerShell, que usa el protocolo WS\-Management, permite ejecutar cualquier comando de Windows PowerShell en uno o varios equipos remotos. Así, permite establecer conexiones persistentes, iniciar sesiones interactivas de 1:1 y ejecutar scripts en varios equipos.

Para usar la comunicación remota de Windows PowerShell, el equipo remoto debe estar configurado para la administración remota. Para más información y ver instrucciones, consulte [About Remote Requirements](https://technet.microsoft.com/en-us/library/dd315349.aspx) (Acerca de los requisitos remotos).

Después de configurar la comunicación remota de Windows PowerShell, tendrá a su disposición un gran número de estrategias de comunicación remota. En lo que resta de este documento se mencionan solo algunas de ellas. Para más información, vea [About Remote](https://technet.microsoft.com/en-us/library/dd347744.aspx) (Acerca del acceso remoto) y [About Remote FAQ](https://technet.microsoft.com/en-us/library/dd347744.aspx) (Preguntas más frecuentes sobre el acceso remoto).

### Iniciar una sesión interactiva
Para iniciar una sesión interactiva con un único equipo remoto, use el cmdlet [Enter-PSSession](https://technet.microsoft.com/en-us/library/dd315384.aspx). Por ejemplo, para iniciar una sesión interactiva con el equipo remoto Server01, escriba:

```
enter-pssession Server01
```

El símbolo del sistema cambia para mostrar el nombre del equipo al que está conectado. Desde ese momento, cualquier comando que escriba en el símbolo del sistema se ejecutará en el equipo remoto y los resultados se mostrarán en el equipo local.

Para finalizar la sesión interactiva, escriba:

```
exit-pssession
```

Para obtener más información sobre los cmdlets \-PSSession y Exit\-PSSession, consulte [Enter-PSSession](https://technet.microsoft.com/en-us/library/dd315384.aspx) y [Exit-PSSession](https://technet.microsoft.com/en-us/library/dd315322.aspx).

### Ejecutar un comando remoto
Para ejecutar cualquier comando en uno o varios equipos remotos, use el cmdlet [Invoke-Command](https://technet.microsoft.com/en-us/library/dd347578.aspx).
Por ejemplo, para ejecutar un comando [Get-UICulture](https://technet.microsoft.com/en-us/library/dd347742.aspx) en los equipos remotos Server01 y Server02, escriba:

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

Para obtener más información sobre el cmdlet Invoke\-Command, consulte [Invoke-Command](https://technet.microsoft.com/en-us/library/22fd98ba-1874-492e-95a5-c069467b8462).

### Ejecutar un script
Para ejecutar un script en uno o varios equipos remotos, use el parámetro FilePath del cmdlet Invoke\-Command. El script debe estar en el equipo local o accesible desde este. Los resultados se devuelven en el equipo local.

Por ejemplo, el siguiente comando ejecuta el script DiskCollect.ps1 en los equipos remotos Server01 y Server02.

```
invoke-command -computername Server01, Server02 -filepath c:\Scripts\DiskCollect.ps1
```

Para obtener más información sobre el cmdlet Invoke\-Command, consulte [Invoke-Command](https://technet.microsoft.com/en-us/library/dd347578.aspx).

### Establecer una conexión persistente
Para ejecutar una serie de comandos relacionados que comparten datos, cree una sesión en el equipo remoto y luego use el cmdlet Invoke\-Command para ejecutar comandos en la sesión que cree. Para crear una sesión remota, use el cmdlet New\-PSSession.

Por ejemplo, el comando siguiente crea una sesión remota en el equipo Server01 y otra sesión remota en el equipo Server02. Guarda los objetos de la sesión en la variable $s.

```
$s = new-pssession -computername Server01, Server02
```

Ahora que las sesiones se han establecido, puede ejecutar cualquier comando en ellas. Y, como las sesiones son persistentes, puede recopilar datos en un solo comando y usarlos en un comando posterior.

Por ejemplo, el siguiente comando ejecuta un comando Get\-Hotfix en las sesiones de la variable $s y guarda los resultados en la variable $h. La variable $h se crea en cada una de las sesiones en $s, pero no existe en la sesión local.

```
invoke-command -session $s {$h = get-hotfix}
```

Ahora puede usar los datos de la variable $h en los siguientes comandos, como en el siguiente. Los resultados se muestran en el equipo local.

```
invoke-command -session $s {$h | where {$_.installedby -ne "NTAUTHORITY\SYSTEM"} }
```

### Comunicación remota avanzada
La administración remota de Windows PowerShell empieza aquí. Con los cmdlets que se instalan con Windows PowerShell puede, entre otras muchas cosas, establecer y configurar sesiones remotas desde extremos tanto locales como remotos, crear sesiones personalizadas y restringidas, dejar que los usuarios importen comandos desde una sesión remota que se ejecutan implícitamente en la sesión remota o configurar la seguridad de una sesión remota.

Para facilitar la configuración remota, Windows PowerShell incluye un proveedor WSMan. La unidad WSMAN: que este proveedor crea permite desplazarse por una jerarquía de valores de configuración en el equipo local y en los equipos remotos.
Para obtener más información sobre el proveedor de WSMan, consulte [Proveedor de WSMan](https://technet.microsoft.com/en-us/library/dd819476.aspx) y   [About WS-Management Cmdlets (Acerca de los cmdlets de WS-Management)](https://technet.microsoft.com/en-us/library/dd819481.aspx). También puede escribir "get\-help wsman" en la consola de Windows PowerShell.

Para obtener más información, consulte:
- [Acerca de las Preguntas más frecuentes sobre el acceso remoto](https://technet.microsoft.com/en-us/library/dd315359.aspx)
- [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/dd819496.aspx)
- [Import-PSSession](https://technet.microsoft.com/en-us/library/dd347575.aspx). 

Para obtener ayuda con los errores de comunicación remota, consulte [about_Remote_Troubleshooting](https://technet.microsoft.com/en-us/library/dd347642.aspx).

## Consulte también
- [about_Remote](https://technet.microsoft.com/en-us/library/9b4a5c87-9162-4adf-bdfe-fbc80b9b8970)
- [about_Remote_FAQ](https://technet.microsoft.com/en-us/library/e23702fd-9415-4a98-9975-390a4d3adc42)
- [about_Remote_Requirements](https://technet.microsoft.com/en-us/library/da213949-134c-4741-b307-81f4492ba1bd)
- [about_Remote_Troubleshooting](https://technet.microsoft.com/en-us/library/2f890148-8578-49ed-85ea-79a489dd6317)
- [about_PSSessions](https://technet.microsoft.com/en-us/library/7a9b4e0e-fa1b-47b0-92f6-6e2995d70acb)
- [about_WS-Management_Cmdlets](https://technet.microsoft.com/en-us/library/6ed3370a-ea10-45a5-9493-696aeace27ed)
- [Invoke-Command](https://technet.microsoft.com/en-us/library/22fd98ba-1874-492e-95a5-c069467b8462)
- [Import-PSSession](https://technet.microsoft.com/en-us/library/048c115e-a6fb-4e0d-8cea-c5ca24630c9d)
- [New-PSSession](https://technet.microsoft.com/en-us/library/59452f12-a11d-4558-99ea-e6ca6ad5ffd3)
- [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/af68867a-d201-4b19-a1de-594015ed8a25)
- [Proveedor de WSMan](https://technet.microsoft.com/en-us/library/66fe1241-e08f-49ca-832f-a84c33ca8735)



<!--HONumber=May16_HO4-->


