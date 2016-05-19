---
title: El objeto ISEAddOnToolCollection
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 634eab89-0845-4016-974b-361b09bb8f7b
---
# El objeto ISEAddOnToolCollection
  El objeto **ISEAddOnToolCollection** es una colección de objetos **ISEAddOnTool**. Un ejemplo es el objeto **$psISE.CurrentPowerShellTab.VerticalAddOnTools**.

## Métodos

### Add\( Name, ControlType, \[IsVisible\] \)
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 Agrega una nueva herramienta de complemento a la colección. Devuelve la herramienta de complemento recién agregada. Antes de ejecutar este comando, debe instalar la herramienta de complemento en el equipo local y cargar el ensamblado.

 **Name**: cadena
 Especifica el nombre para mostrar de la herramienta de complemento que se agrega a Windows PowerShell ISE.

 **ControlType**: tipo
 Especifica el control que se agrega.

 **\[IsVisible\]**: booleano opcional
 Si se establece en **$true**, la herramienta de complemento está visible inmediatamente en el panel de herramientas asociado.

```
# Load a DLL with an add-on and then add it to the ISE
[reflection.assembly]::LoadFile("c:\test\ISESimpleSolution\ISESimpleSolution.dll")
$psISE.CurrentPowerShellTab.VerticalAddOnTools.Add("Solutions", [ISESimpleSolution.Solution], $true)

```

### Remove\( Item \)
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 Quita de la colección la herramienta de complemento especificada.

 **Item**: Microsoft.PowerShell.Host.ISE.ISEAddOnTool
 Especifica el objeto que se quitará de Windows PowerShell ISE.

```
# Load a DLL with an add-on and then add it to the ISE
[reflection.assembly]::LoadFile("c:\test\ISESimpleSolution\ISESimpleSolution.dll")
$psISE.CurrentPowerShellTab.VerticalAddOnTools.Add("Solutions", [ISESimpleSolution.Solution], $true)

```

### SetSelectedPowerShellTab\( psTab \)
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 Selecciona la pestaña de PowerShell que el parámetro **psTab** especifica.

 **psTab**: Microsoft.PowerShell.Host.ISE.PowerShellTab
 La pestaña de PowerShell que se seleccionará.

```

      $newTab = $psISE.PowerShellTabs.Add()
# Change the DisplayName of the new PowerShell tab. 
$newTab.DisplayName="Brand New Tab"

```

### Remove\( psTab \)
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 Quita la pestaña de PowerShell que el parámetro **psTab** especifica.

 **psTab**: Microsoft.PowerShell.Host.ISE.PowerShellTab
 La pestaña de PowerShell que se quitará.

```

$newTab = $psISE.PowerShellTabs.Add()
Change the DisplayName of the new PowerShell tab. 
$newTab.DisplayName="This tab will go away in 5 seconds" 
sleep 5 
$psISE.PowerShellTabs.Remove($newTab)
```

## Consulte también
 [El objeto PowerShellTab](The-PowerShellTab-Object.md) 
 [El modelo de objetos de scripting de ISE de Windows PowerShell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referencia del modelo de objetos de ISE de Windows PowerShell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [La jerarquía del modelo de objetos de ISE](The-ISE-Object-Model-Hierarchy.md)

  


<!--HONumber=May16_HO2-->


