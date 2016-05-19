---
title: El objeto ISEMenuItemCollection
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c0f5484-3320-408e-8534-5bd1c8e48512
---
# El objeto ISEMenuItemCollection
  Un objeto **ISEMenuItemCollection** es una colección de objetos **ISEMenuItem**. Es una instancia de la clase Microsoft.PowerShell.Host.ISE.ISEMenuItemCollection. Un ejemplo es el objeto **$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus** que se utiliza para personalizar el menú **Complemento** en el Entorno de scripting integrado (ISE) de Windows PowerShellÂ®.

## Método

### Add\(string DisplayName, System.Management.Automation.ScriptBlock Action, System.Windows.Input.KeyGesture Shortcut \)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Agrega un elemento de menú a la colección.

 **DisplayName**
 El nombre para mostrar del menú que se va a agregar.

 **Acción**
 El objeto **System.Management.Automation.ScriptBlock** que especifica la acción que está asociada con este elemento de menú.

 **Directa**
 El método abreviado de teclado para la acción.

 **Devuelve**
 El objeto ISEMenuItem que acaba de agregar.

```
# Create an Add-ons menu with an fast access key and a shortcut.
# Note the use of "_"  as opposed to the "&" for mapping to the fast access key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("_Process",{get-process},"Alt+P")
```

### Clear\(\)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Quita todos los submenús del elemento de menú.

```
# Remove all custom submenu items from the AddOns menu
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()

```

## Consulte también
 [El objeto ISEMenuItem](The-ISEMenuItem-Object.md) 
 [El modelo de objetos de scripting de ISE de Windows PowerShell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referencia del modelo de objetos de ISE de Windows PowerShell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [La jerarquía del modelo de objetos de ISE](The-ISE-Object-Model-Hierarchy.md)

  


<!--HONumber=May16_HO2-->


