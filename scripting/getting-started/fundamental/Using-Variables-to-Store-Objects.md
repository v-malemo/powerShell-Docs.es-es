---
title: Usar variables para almacenar objetos
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1688d73-c173-491e-9ba6-6d0c1cc852de
---
# Usar variables para almacenar objetos
Windows PowerShell funciona con objetos. Windows PowerShell permite crear variables (básicamente objetos con nombre) para conservar la salida para usos posteriores. Si está acostumbrado a trabajar con variables en otros shells, recuerde que las variables de Windows PowerShell son objetos, no texto.

Las variables siempre se especifican con el carácter inicial $ y pueden incluir cualquier carácter alfanumérico o el carácter de subrayado en sus nombres.

### Crear una variable
Para crear una variable, puede escribir un nombre de variable válido:

```
PS> $loc
PS>
```

Esto no devuelve ningún resultado porque **$loc** no tiene un valor. Puede crear una variable y asignarle un valor en el mismo paso. Windows PowerShell solo crea la variable si no existe; de lo contrario, asigna el valor especificado a la variable existente. Para almacenar su ubicación actual en la variable **$loc**, escriba:

```
$loc = Get-Location
```

No se muestra ninguna salida cuando escribe este comando porque la salida se envía a $loc. En Windows PowerShell, la salida mostrada es un efecto secundario del hecho de que los datos siempre se envíen a la pantalla, si no se indica de otro modo. Al escribir $loc se mostrará su ubicación actual:

```
PS> $loc

Path
----
C:\temp
```

Puede usar **Get-Member** para mostrar información sobre el contenido de las variables. La canalización de $loc a Get-Member le mostrará que se trata de un objeto **PathInfo**, al igual que la salida de Get-Location:

```
PS> $loc | Get-Member -MemberType Property

   TypeName: System.Management.Automation.PathInfo

Name         MemberType Definition
----         ---------- ----------
Drive        Property   System.Management.Automation.PSDriveInfo Drive {get;}
Path         Property   System.String Path {get;}
Provider     Property   System.Management.Automation.ProviderInfo Provider {...
ProviderPath Property   System.String ProviderPath {get;}
```

### Manipular variables
Windows PowerShell proporciona varios comandos para manipular variables. Para ver una lista completa en un formato legible, escriba:

```
Get-Command -Noun Variable | Format-Table -Property Name,Definition -AutoSize -Wrap
```

Además de las variables que crea en la sesión actual de Windows PowerShell, existen varias variables definidas por el sistema. Puede usar el cmdlet **Remove-Variable** para borrar todas las variables que no se controlan mediante Windows PowerShell. Escriba el siguiente comando para borrar todas las variables:

```
Remove-Variable -Name * -Force -ErrorAction SilentlyContinue
```

Esto generará el mensaje de confirmación que aparece a continuación.

```
Confirm
Are you sure you want to perform this action?
Performing operation "Remove Variable" on Target "Name: Error".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):A
```

Si ejecuta el cmdlet **Get-Variable**, verá las demás variables de Windows PowerShell. Puesto que también existe una unidad de Windows PowerShell variable, también puede mostrar todas las variables de Windows PowerShell. Para ello, escriba:

```
Get-ChildItem variable:
```

### Usar variables de Cmd.exe
Aunque Windows PowerShell no es Cmd.exe, se ejecuta en un entorno de shell de comandos y puede usar las mismas variables disponibles en cualquier entorno de Windows. Estas variables se exponen a través de una unidad denominada **env**:. Para ver estas variables, escriba:

```
Get-ChildItem env:
```

Aunque los cmdlets de variables estándar no están diseñados para trabajar con variables **env:**, puede seguir usándolos si especifica el prefijo **env:**. Por ejemplo, para ver el directorio raíz del sistema operativo, puede usar la variable **%SystemRoot%** del shell de comandos en Windows PowerShell. Para ello, escriba:

```
PS> $env:SystemRoot
C:\WINDOWS
```

También puede crear y modificar variables de entorno desde Windows PowerShell. Las variables de entorno a las que se accede desde Windows PowerShell se ajustan a las reglas habituales para las variables de entorno en cualquier otra ubicación de Windows.



<!--HONumber=Apr16_HO1-->


