---
title: El objeto ISEFileCollection
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f86a427-ea38-4bce-85f8-06c98d30d508
---
# El objeto ISEFileCollection
  El objeto **ISEFileCollection** es una colección de objetos **ISEFile**. Un ejemplo es la colección $psISE.CurrentPowerShellTab.Files.

## Métodos

### Add\( \[rutaAccesoCompleta\] \)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Crea y devuelve un archivo sin título nuevo y lo agrega a la colección. La propiedad **IsUntitled** del archivo recién creado es **$true**..

 **\[rutaAccesoCompleta\]**: cadena opcional
 Ruta de acceso completamente especificada del archivo. Se genera una excepción si incluye el parámetro **rutaAccesoCompleta** y una ruta de acceso relativa, o si utiliza un nombre de archivo en lugar de la ruta de acceso completa.

```
# Adds a new untitled file to the collection of files in the current PowerShell tab.
$newFile = $psISE.CurrentPowerShellTab.Files.Add()

# Adds a file specified by its full path to the collection of files in the current PowerShell tab.
$psISE.CurrentPowerShellTab.Files.Add("$pshome\Examples\profile.ps1")

```

### Remove\(Archivo, \[Force\]\)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Quita un archivo especificado en la pestaña actual de PowerShell.

 **Archivo**: Cadena
 El archivo ISEFile que desea quitar de la colección. Si el archivo no se ha guardado, este método inicia una excepción. Utilice el parámetro modificador **Force** parámetro para forzar la eliminación de un archivo no guardado.

 **\[Force\]**: Booleano opcional
 Si establece en **$true**, concede permiso para quitar el archivo incluso si no se ha guardado después del último uso. El valor predeterminado es **$false**..

```
# Removes the first opened file from the file collection associated with the current PowerShell tab.
# If the file has not yet been saved, then an exception is generated.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.Remove($firstfile)

# Removes the first opened file from the file collection associated with the current PowerShell tab, even if it has not been saved.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.Remove($firstfile, $true)
```

### SetSelectedFile\ (archivoSeleccionado\)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Selecciona el archivo que especifica el parámetro **archivoSeleccionado**.

 **archivoSeleccionado**: Microsoft.PowerShell.Host.ISE.ISEFile.
 El archivo ISEFile que desea seleccionar.

```

# Selects the specified file.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.SetSelectedFile($firstfile)

```

## Consulte también
 [El objeto ISEFile](The-ISEFile-Object.md) 
 [El modelo de objetos de scripting de ISE de Windows PowerShell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referencia del modelo de objetos de ISE de Windows PowerShell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [La jerarquía del modelo de objetos de ISE](The-ISE-Object-Model-Hierarchy.md)

  


<!--HONumber=May16_HO2-->


