# Nuevas características de lenguaje de PowerShell 5.0 

PowerShell 5.0 presenta los siguientes nuevos elementos de lenguaje en Windows PowerShell:

## Palabra clave class

La palabra clave **class** define una nueva clase. Se trata de un tipo verdadero de .NET Framework. 
Los miembros de clase son públicos, pero solo en el ámbito del módulo.
No puede hacer referencia al nombre de tipo como una cadena (por ejemplo, `New-Object` no funciona) y, en esta versión, no puede usar un literal de tipo (por ejemplo, `[MyClass]`) fuera del archivo de script o módulo en el que se define la clase.

```powershell
class MyClass
{
    ...
}
```

## Palabra clave Enum y enumeraciones

Se agregó compatibilidad con la palabra clave **enum**, que usa el delimitador de nueva línea.
Limitaciones actuales: no se puede definir un enumerador en términos propios, pero se puede inicializar una enumeración en términos de otra enumeración, tal como se muestra en el ejemplo siguiente.
Además, el tipo base no se puede especificar actualmente; siempre es [int].

```powershell
enum Color2
{
    Yellow = [Color]::Blue
}
```

Un valor de enumerador debe ser una constante de tiempo de análisis; no puede establecerlo en el resultado de un comando invocado.

```powershell
enum MyEnum
{
    Enum1
    Enum2
    Enum3 = 42
    Enum4 = [int]::MaxValue
}
```

Las enumeraciones admiten operaciones aritméticas, como se muestra en el ejemplo siguiente.

```powershell
enum SomeEnum { Max = 42 }
enum OtherEnum { Max = [SomeEnum]::Max + 1 }
```

## Import-DscResource

**Import-DscResource** es ahora una palabra clave dinámica verdadera.
PowerShell analiza el módulo raíz del módulo especificado y busca las clases que contienen el atributo **DscResource**.

## ImplementingAssembly

Un nuevo campo, **ImplementingAssembly**, se agregó a ModuleInfo. Se establece en el ensamblado dinámico creado para un módulo de script si el script define clases, o en el ensamblado cargado en el caso de los módulos binarios. No se establece si ModuleType = Manifest. 

La reflexión en el campo **ImplementingAssembly** descubre los recursos de un módulo. Esto significa que puede descubrir recursos escritos en PowerShell o en otros lenguajes administrados.

Campos con inicializadores:      

```powershell
[int] $i = 5
```

Se admite static; funciona como un atributo, como ocurre en las restricciones de tipo, por lo que puede especificarse en cualquier orden.

```powershell
static [int] $count = 0
```

Un tipo es opcional.

```powershell
$s = "hello"
```

Todos los miembros son públicos. 

## Creación de instancias y constructores

Las clases de Windows PowerShell pueden tener constructores; tienen el mismo nombre que su clase. Los constructores se pueden sobrecargar. Se admiten constructores estáticos. Las propiedades con expresiones de inicialización se inicializan antes de ejecutar cualquier código en un constructor. Las propiedades estáticas se inicializan antes del cuerpo de un constructor estático y las propiedades de instancia se inicializan antes del cuerpo del constructor no estático. Actualmente no hay ninguna sintaxis para llamar a un constructor desde otro constructor (como la sintaxis de C\# ": this()"). La solución consiste en definir un método Init común. 

Las siguientes son formas de crear instancias de clases en esta versión.

Crear una instancia mediante el constructor predeterminado. Tenga en cuenta que New-Object no se admite en esta versión.

```powershell
$a = [MyClass]::new()
```

Llamar a un constructor con un parámetro

```powershell
$b = [MyClass]::new(42)
```

Pasar una matriz a un constructor con varios parámetros
```powershell
$c = [MyClass]::new(@(42,43,44), "Hello")
```

En esta versión, New-Object no funciona con las clases definidas en Windows PowerShell. En esta versión, el nombre del tipo solo es visible léxicamente, lo que significa que no está visible fuera del módulo o el script que define la clase. Las funciones pueden devolver instancias de una clase definida en Windows PowerShell, y las instancias funcionan bien fuera del módulo o el script.

`Get-Member -Static` enumera los constructores, para que pueda ver las sobrecargas como cualquier otro método. El rendimiento de esta sintaxis también es considerablemente más rápido que el de New-Object.

El método seudoestático denominado **new** funciona con los tipos .NET, como se muestra en el ejemplo siguiente.

```powershell
[hashtable]::new()
```

Ahora puede ver las sobrecargas de constructores con Get-Member, o bien como se muestra en este ejemplo:

```powershell
PS> [hashtable]::new
OverloadDefinitions
-------------------
hashtable new()
hashtable new(int capacity)
hashtable new(int capacity, float loadFactor)
```

## Métodos

Un método de clase de Windows PowerShell se implementa como un bloque de script que tiene solo un bloque final. Todos los métodos son públicos. A continuación, se ofrece un ejemplo de definición de un método denominado **DoSomething**.

```powershell
class MyClass
{
    DoSomething($x)
    {
        $this._doSomething($x) # method syntax
    }
    private _doSomething($a) {}
}
```

Invocación del método:

```powershell
$b = [MyClass]::new()
$b.DoSomething(42) 
```

También se admiten los métodos sobrecargados, es decir, aquellos con el mismo nombre que un método existente, pero que se diferencian por los valores especificados.

## Propiedades 

Todas las propiedades son públicas. Las propiedades requieren una nueva línea o un punto y coma. Si no se especifica ningún tipo de objeto, el tipo de propiedad es object.

Las propiedades que usan atributos de validación o atributos de transformación de argumentos (por ejemplo, `[ValidateSet("aaa")]`) funcionan según lo esperado.

## Hidden

Se agregó la nueva palabra clave **Hidden**. **Hidden** se puede aplicar a propiedades y métodos (incluidos constructores).

Los miembros ocultos son públicos, pero no aparecen en la salida de Get-Member salvo que se agregue el parámetro -Force.

Los miembros ocultos no se incluyen al finalizar con tabulación o usar Intellisense, salvo que la finalización se produzca en la clase que define el miembro oculto.

Un atributo nuevo, **System.Management.Automation.HiddenAttribute** se agregó para que el código de C# pueda tener la misma semántica en Windows PowerShell.

## Tipos de valor devueltos

El tipo de valor devuelto es un contrato; el valor devuelto se convierte en el tipo esperado. Si no se especifica ningún tipo de valor devuelto, el tipo de valor devuelto es void. No hay ningún streaming de objetos; los objetos no se pueden escribir en la canalización, ni intencionadamente ni por accidente.

## Atributos

Se agregaron cuatro nuevos atributos, **DscResource**, **DscResourceKey**, **DscResourceMandatory** y **DscResourceOut**.

## Ámbito léxico de las variables

A continuación, se muestra un ejemplo del funcionamiento del ámbito léxico en esta versión.

```powershell
$d = 42 # Script scope

function bar
{
    $d = 0 # Function scope
    [MyClass]::DoSomething()
}

class MyClass
{
    static [object] DoSomething()
    {
        return $d # error, not found dynamically
        return $script:d # no error
        $d = $script:d
        return $d # no error, found lexically
    }
}

$v = bar
$v -eq $d # true
```

## Acceso de un extremo a otro

En el ejemplo siguiente se crean varias clases nuevas y personalizadas para implementar un lenguaje de hojas de estilo dinámico (DSL) HTML. 
A continuación, el ejemplo agrega funciones auxiliares para crear tipos de elementos específicos como parte de la clase de elemento, tales como tablas y estilos de encabezado, porque los tipos no pueden usarse fuera del ámbito de un módulo.

```powershell
# Classes that define the structure of the document
#
class Html
{
    [string] $docType
    [HtmlHead] $Head
    [Element[]] $Body
    
    [string] Render()
    {
        $text = "<html>`n<head>`n"
        $text += $this.Head
        $text += "`n</head>`n<body>`n"
        $text += $this.Body -join "`n" # Render all of the body elements
        $text += "</body>`n</html>"
        return $text
    }
    [string] ToString() { return $this.Render() }
}

class HtmlHead
{
    $Title
    $Base
    $Link
    $Style
    $Meta
    $Script
    [string] Render() { return "<title>$($this.Title)</title>" }
    [string] ToString() { return $this.Render() }
}

class Element
{
    [string] $Tag
    [string] $Text
    [hashtable] $Attributes
    [string] Render() {
        $attributesText= ""
        if ($this.Attributes)
        {
            foreach ($attr in $this.Attributes.Keys)
            {
                #BUGBUG - need to validate keys against attribute
                $attributesText += " $attr=`"$($this.Attributes[$attr])\`""
            }
        }
        return "<$($this.tag)${attributesText}>$($this.text)</$($this.tag)>`n"
    }
[string] ToString() { return $this.Render() }
}

#
# Helper functions for creating specific element types on top of the classes.
# These are required because types aren’t visible outside of the module.
#

function H1 { [Element] @{ Tag = "H1" ; Text = $args.foreach{$_} -join " " }}
function H2 { [Element] @{ Tag = "H2" ; Text = $args.foreach{$_} -join " " }}
function H3 { [Element] @{ Tag = "H3" ; Text = $args.foreach{$_} -join " " }}
function P { [Element] @{ Tag = "P" ; Text = $args.foreach{$_} -join " " }}
function B { [Element] @{ Tag = "B" ; Text = $args.foreach{$_} -join " " }}
function I { [Element] @{ Tag = "I" ; Text = $args.foreach{$_} -join " " }}
function HREF
{
    param (
        $Name,
        $Link
    )
    return [Element] @{
        Tag = "A"
        Attributes = @{ HREF = $link }
        Text = $name
    }
}
function Table
{
    param (
    [Parameter(Mandatory)]
    [object[]]
        $Data,
    [Parameter()]
    [string[]]
        $Properties = "*",
    [Parameter()]
    [hashtable]
        $Attributes = @{ border=2; cellpadding=2; cellspacing=2 }
    )
$bodyText = ""
# Add the header tags
$bodyText += $Properties.foreach{TH $_}
# Add the rows
$bodyText += foreach ($row in $Data)
    {
        TR (-join $Properties.Foreach{ TD ($row.$\_) } )
    }

    $table = [Element] @{
        Tag = "Table"
        Attributes = $Attributes
        Text = $bodyText
    }
$table
}
function TH { ([Element] @{ Tag = "TH" ; Text = $args.foreach{$_} -join " " }) }
function TR { ([Element] @{ Tag = "TR" ; Text = $args.foreach{$_} -join " " }) }
function TD { ([Element] @{ Tag = "TD" ; Text = $args.foreach{$_} -join " " }) }
function Style

{
    return [Element] @{
        Tag = "style"
        Text = "$args"
    }
}

# Takes a hash table, casts it to and HTML document
# and then returns the resulting type.
#
function Html ([HTML] $doc) { return $doc }
```<!--HONumber=Mar16_HO2-->
