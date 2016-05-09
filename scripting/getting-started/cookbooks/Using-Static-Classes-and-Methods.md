---
title: Usar métodos y clases estáticas
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 418ad766-afa6-4b8c-9a44-471889af7fd9
---
# Usar métodos y clases estáticas
No todas las clases de .NET Framework se pueden crear mediante **New-Object**. Por ejemplo, si intenta crear un objeto **System.Environment** o **System.Math** con **New-Object**, obtendrá los siguientes mensajes de error:

```
PS> New-Object System.Environment
New-Object : Constructor not found. Cannot find an appropriate constructor for
type System.Environment.
At line:1 char:11
+ New-Object  <<<< System.Environment
PS> New-Object System.Math
New-Object : Constructor not found. Cannot find an appropriate constructor for
type System.Math.
At line:1 char:11
+ New-Object  <<<< System.Math
```

Estos errores se producen porque no hay ninguna manera de crear un nuevo objeto desde estas clases. Estas clases son bibliotecas de métodos y propiedades de referencia que no cambian el estado. No es necesario crearlas, simplemente úselas. Las clases y métodos de este tipo se denominan *clases estáticas* porque no se crean, destruyen ni modifican. Para aclarar esto, proporcionaremos algunos ejemplos que usan clases estáticas.

### Obtener datos del entorno con System.Environment
Normalmente, el primer paso para trabajar con un objeto en Windows PowerShell es usar Get-Member para averiguar qué miembros contiene. Con las clases estáticas, el proceso es un poco diferente, porque la clase real no es un objeto.

#### Hacer referencia a la clase System.Environment estática
Para hacer referencia a una clase estática incluya el nombre de la clase entre corchetes. Por ejemplo, para hacer referencia a **System.Environment** escriba el nombre entre corchetes. Al hacerlo, se mostrará alguna información de tipo genérico:

```
PS> [System.Environment]

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    Environment                              System.Object
```

> [!NOTE]
> Como mencionamos anteriormente, Windows PowerShell antepone '**System.**' automáticamente a los nombres de tipos cuando usa **New-Object**. Lo mismo ocurre cuando se usa un nombre de tipo entre corchetes, por lo que puede especificar **\[System.Environment]** como **\[Environment]**.

La clase **System.Environment** contiene información general sobre el entorno de trabajo para el proceso actual, que es powershell.exe al trabajar en Windows PowerShell.

Si intenta ver los detalles de esta clase escribiendo **\[System.Environment] | Get-Member**, el tipo de objeto se indica como **System.RuntimeType**, no como **System.Environment**:

```
PS> [System.Environment] | Get-Member

   TypeName: System.RuntimeType
```

Para ver los miembros estáticos con Get-Member, especifique el parámetro **Static**:

```
PS> [System.Environment] | Get-Member -Static

   TypeName: System.Environment

Name                       MemberType Definition
----                       ---------- ----------
Equals                     Method     static System.Boolean Equals(Object ob...
Exit                       Method     static System.Void Exit(Int32 exitCode)
...
CommandLine                Property   static System.String CommandLine {get;}
CurrentDirectory           Property   static System.String CurrentDirectory ...
ExitCode                   Property   static System.Int32 ExitCode {get;set;}
HasShutdownStarted         Property   static System.Boolean HasShutdownStart...
MachineName                Property   static System.String MachineName {get;}
NewLine                    Property   static System.String NewLine {get;}
OSVersion                  Property   static System.OperatingSystem OSVersio...
ProcessorCount             Property   static System.Int32 ProcessorCount {get;}
StackTrace                 Property   static System.String StackTrace {get;}
SystemDirectory            Property   static System.String SystemDirectory {...
TickCount                  Property   static System.Int32 TickCount {get;}
UserDomainName             Property   static System.String UserDomainName {g...
UserInteractive            Property   static System.Boolean UserInteractive ...
UserName                   Property   static System.String UserName {get;}
Version                    Property   static System.Version Version {get;}
WorkingSet                 Property   static System.Int64 WorkingSet {get;}
TickCount                               ExitCode
```

Ahora podemos seleccionar las propiedades que queremos ver desde System.Environment.

#### Visualizar propiedades estáticas de System.Environment
Las propiedades de System.Environment también son estáticas y deben especificarse de manera diferente que las propiedades normales. Usamos **::** para indicar a Windows PowerShell que queremos trabajar con una propiedad o un método estático. Para ver el comando que se usó para iniciar Windows PowerShell, comprobamos la propiedad **CommandLine**, para lo cual escribimos:

```
PS> [System.Environment]::Commandline
"C:\Program Files\Windows PowerShell\v1.0\powershell.exe"
```

Para comprobar la versión del sistema operativo, debemos mostrar la propiedad OSVersion. Para ello, escribimos:

```
PS> [System.Environment]::OSVersion

           Platform ServicePack         Version             VersionString
           -------- -----------         -------             -------------
            Win32NT Service Pack 2      5.1.2600.131072     Microsoft Windows...
```

Para comprobar si el equipo se está apagando, podemos mostrar la propiedad **HasShutdownStarted**:

```
PS> [System.Environment]::HasShutdownStarted
False
```

### Operaciones matemáticas con System.Math
La clase estática System.Math es útil para realizar algunas operaciones matemáticas. Los miembros importantes de **System.Math** son principalmente métodos, que podemos mostrar mediante **Get-Member**.

> [!NOTE]
> System.Math tiene varios métodos con el mismo nombre, pero se distinguen por el tipo de sus parámetros.

Escriba el siguiente comando para enumerar los métodos de la clase **System.Math**.

```
PS> [System.Math] | Get-Member -Static -MemberType Methods

   TypeName: System.Math

Name            MemberType Definition
----            ---------- ----------
Abs             Method     static System.Single Abs(Single value), static Sy...
Acos            Method     static System.Double Acos(Double d)
Asin            Method     static System.Double Asin(Double d)
Atan            Method     static System.Double Atan(Double d)
Atan2           Method     static System.Double Atan2(Double y, Double x)
BigMul          Method     static System.Int64 BigMul(Int32 a, Int32 b)
Ceiling         Method     static System.Double Ceiling(Double a), static Sy...
Cos             Method     static System.Double Cos(Double d)
Cosh            Method     static System.Double Cosh(Double value)
DivRem          Method     static System.Int32 DivRem(Int32 a, Int32 b, Int3...
Equals          Method     static System.Boolean Equals(Object objA, Object ...
Exp             Method     static System.Double Exp(Double d)
Floor           Method     static System.Double Floor(Double d), static Syst...
IEEERemainder   Method     static System.Double IEEERemainder(Double x, Doub...
Log             Method     static System.Double Log(Double d), static System...
Log10           Method     static System.Double Log10(Double d)
Max             Method     static System.SByte Max(SByte val1, SByte val2), ...
Min             Method     static System.SByte Min(SByte val1, SByte val2), ...
Pow             Method     static System.Double Pow(Double x, Double y)
ReferenceEquals Method     static System.Boolean ReferenceEquals(Object objA...
Round           Method     static System.Double Round(Double a), static Syst...
Sign            Method     static System.Int32 Sign(SByte value), static Sys...
Sin             Method     static System.Double Sin(Double a)
Sinh            Method     static System.Double Sinh(Double value)
Sqrt            Method     static System.Double Sqrt(Double d)
Tan             Method     static System.Double Tan(Double a)
Tanh            Method     static System.Double Tanh(Double value)
Truncate        Method     static System.Decimal Truncate(Decimal d), static...
```

Muestra varios métodos matemáticos. A continuación, presentamos una lista de comandos que muestran el funcionamiento de algunos de los métodos comunes:

```
PS> [System.Math]::Sqrt(9)
3
PS> [System.Math]::Pow(2,3)
8
PS> [System.Math]::Floor(3.3)
3
PS> [System.Math]::Floor(-3.3)
-4
PS> [System.Math]::Ceiling(3.3)
4
PS> [System.Math]::Ceiling(-3.3)
-3
PS> [System.Math]::Max(2,7)
7
PS> [System.Math]::Min(2,7)
2
PS> [System.Math]::Truncate(9.3)
9
PS> [System.Math]::Truncate(-9.3)
-9
```



<!--HONumber=Apr16_HO1-->


