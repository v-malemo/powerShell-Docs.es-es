# Acceso directo a los métodos de recursos de DSC


El cmdlet `Invoke-DscResource` se agregó para permitir el acceso directo a los recursos de DSC y a sus métodos existentes (Get, Set o Test). Los pueden usar terceros que quieran sacar partido de los recursos de DSC. Este cmdlet suele usarse en combinación con `refreshMode = ‘Disabled’`, pero se puede usar independientemente de la opción establecida en refreshMode. A continuación se muestran algunos ejemplos de cómo usar el nuevo cmdlet:

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


<!--HONumber=Apr16_HO4-->


