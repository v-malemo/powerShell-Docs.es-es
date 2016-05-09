# Llamada directa a los métodos de recursos de DSC

>Se aplica a: Windows PowerShell 5.0

Puede usar el cmdlet [Invoke-DscResource](https://technet.microsoft.com/en-us/library/mt517869.aspx) para llamar directamente a las funciones o los métodos de un recurso de DSC (las funciones **Get TargetResource**,
**Set-TargetResource** y **Test-TargetResource** de un recurso basado en MOF o los métodos **Get**, **Set** y **Test** de un recurso basado en clases). 
Esto puede servir para terceros que quieran usar recursos de DSC, o como herramienta útil durante el desarrollo de recursos. 

Este cmdlet suele usarse en combinación con una propiedad de metaconfiguración `refreshMode = 'Disabled'`, pero se puede usar independientemente de la opción establecida en **refreshMode**.

Al llamar al cmdlet **Invoke-DscResource**, debe especificar qué método o función llamar mediante el parámetro **Method**. Puede especificar las propiedades del recurso pasando una 
tabla hash como valor del parámetro **Property**.

A continuación, se muestran ejemplos de llamada directa a los métodos de recursos:

## Asegurarse de que un archivo está presente

```powershell
$result = Invoke-DscResource -Name File -Method Set -Property @{
                            DestinationPath = "$env:SystemDrive\\DirectAccess.txt";
                            Contents = 'This file is create by Invoke-DscResource'} -Verbose
$result | fl
```

## Comprobar que un archivo está presente

```powershell
$result = Invoke-DscResource -Name File -Method Test -Property @{
                            DestinationPath="$env:SystemDrive\\DirectAccess.txt";
                            Contents='This file is create by Invoke-DscResource'} -Verbose
$result | fl
```

## Obtener el contenido del archivo

```powershell
$result = Invoke-DscResource -Name File -Method Get -Property @{
                            DestinationPath="$env:SystemDrive\\DirectAccess.txt";
                            Contents='This file is create by Invoke-DscResource'} -Verbose
$result.ItemValue | fl
```

## Consulte también
- [Escribir un recurso de DSC personalizado con MOF](authoringResourceMOF.md) 
- [Escribir un recurso de DSC personalizado con clases de PowerShell](authoringResourceClass.md)
- [Depuración de recursos de DSC](debugResource.md)

<!--HONumber=Apr16_HO4-->


