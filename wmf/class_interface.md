# Declarar la interfaz implementada

Puede declarar interfaces implementadas después de los tipos base o inmediatamente después de dos puntos (:) si no existe ningún tipo base especificado. Separe todos los nombres de tipo con comas. Es muy similar a la sintaxis de C#.

```PowerShell
class MyComparable : system.IComparable
{
    [int] CompareTo([object] $obj)
    {
        return 0;
    }
}

class MyComparableBar : bar, system.IComparable
{
    [int] CompareTo([object] $obj)
    {
        return 0;
    }
}
```

<!--HONumber=Jun16_HO4-->


