# Llamar al constructor de clase base

Para llamar a un constructor de clase base desde una subclase, use la palabra clave **base**:

```PowerShell
class A 
{
    [int]$a

    A([int]$a)
    {
        $this.a = $a
    }
}

class B : A
{
    B() : base(103) {}
}

[B]::new().a # return 103
```

Si una clase base tiene un constructor predeterminado (sin parámetros), se puede omitir una llamada explícita al constructor:

```PowerShell
class C : B
{
    C([int]$c) {}
}
```

<!--HONumber=Jun16_HO4-->


