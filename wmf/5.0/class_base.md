# Declarar una clase base
Puede declarar una clase de Windows PowerShell como un tipo base para otra clase de Windows PowerShell.

```PowerShell
class bar
{
   [int]foo() 
       {
           return 100500
       }
}

class baz : bar {}

[baz]::new().foo() # return 100500
```

También puede usar los tipos de .NET Framework existentes como clases base:

```PowerShell
class MyIntList : system.collections.generic.list[int]
{
    
}

$list = [MyIntList]::new()

$list.Add(100)

$list[0] # return 100
```

<!--HONumber=Jun16_HO4-->


