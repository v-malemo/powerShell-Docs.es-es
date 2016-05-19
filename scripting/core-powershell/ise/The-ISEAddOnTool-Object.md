---
title: El objeto ISEAddOnTool
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce84d8bc-07ba-41f6-bdde-d6f3fddcd1e3
---
# El objeto ISEAddOnTool
  Un objeto **ISEAddonTool** representa una herramienta de complemento instalada que proporciona funcionalidad adicional a Windows PowerShell ISE. Un ejemplo es la herramienta **Comandos** que se puede mostrar haciendo clic en **Ver** y, después, en **Show Command Add-on** (Mostrar complemento Comandos). Esta herramienta es accesible mediante la manipulación de los distintos objetos **ISEAddOnTool** disponibles.

 Cada herramienta de complemento se puede asociar al panel vertical o al panel horizontal. El panel vertical está acoplado al borde derecho de Windows PowerShell ISE. El panel horizontal está acoplado al borde inferior.

 Cada pestaña de PowerShell en Windows PowerShell ISE puede tener instalado su propio conjunto de herramientas de complemento. Consulte [$psISE.CurrentPowerShellTab.HorizontalAddOnTools](The-ISEAddOnToolCollection-Object.md) y [$psISE.CurrentPowerShellTab.VerticalAddOnTools](The-ISEAddOnToolCollection-Object.md) para tener acceso a la colección de herramientas disponibles en la pestaña seleccionada actualmente o las mismas propiedades en cualquiera de los objetos **PowerShellTab** del objeto de la colección [$psISE.PowerShellTabs](The-PowerShellTabCollection-Object.md).

## Métodos
 No hay disponible ningún método específico de Windows PowerShell ISE para los objetos de esta clase.

## Propiedades

###  <a name="Control"></a> Control
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores.

 La propiedad **Control** proporciona acceso de lectura a muchos de los detalles de la herramienta de complemento Comandos.

```
# View the properties of the Commands add-on tool.
# (assumes that it is visible in the vertical pane)
$psISE.CurrentVisibleVerticalTool.Control
HostObject                  : Microsoft.PowerShell.Host.ISE.ObjectModelRoot
Content                     :
HasContent                  :
ContentTemplate             :
ContentTemplateSelector     :
ContentStringFormat         :
BorderBrush                 :
BorderThickness             :
Background                  :
Foreground                  :
FontFamily                  :
FontSize                    :
FontStretch                 :
FontStyle                   :
FontWeight                  :
HorizontalContentAlignment  :
VerticalContentAlignment    :
TabIndex                    :
IsTabStop                   :
Padding                     :
Template                    : System.Windows.Controls.ControlTemplate
Style                       :
OverridesDefaultStyle       :
UseLayoutRounding           :
Triggers                    : {}
TemplatedParent             :
Resources                   : {System.Windows.Controls.TabItem}
DataContext                 :
BindingGroup                :
Language                    :
Name                        :
Tag                         :
InputScope                  :
ActualWidth                 : 370.75
ActualHeight                : 676.559097412109
LayoutTransform             :
Width                       :
MinWidth                    :
MaxWidth                    :
Height                      :
MinHeight                   :
MaxHeight                   :
FlowDirection               : LeftToRight
Margin                      :
HorizontalAlignment         :
VerticalAlignment           :
FocusVisualStyle            :
Cursor                      :
ForceCursor                 :
IsInitialized               : True
IsLoaded                    :
ToolTip                     :
ContextMenu                 :
Parent                      :
HasAnimatedProperties       :
InputBindings               :
CommandBindings             :
AllowDrop                   :
DesiredSize                 : 227.66,676.559097412109
IsMeasureValid              : True
IsArrangeValid              : True
RenderSize                  : 370.75,676.559097412109
RenderTransform             :
RenderTransformOrigin       :
IsMouseDirectlyOver         : False
IsMouseOver                 : False
IsStylusOver                : False
IsKeyboardFocusWithin       : False
IsMouseCaptured             :
IsMouseCaptureWithin        : False
IsStylusDirectlyOver        : False
IsStylusCaptured            :
IsStylusCaptureWithin       : False
IsKeyboardFocused           : False
IsInputMethodEnabled        :
Opacity                     :
OpacityMask                 :
BitmapEffect                :
Effect                      :
BitmapEffectInput           :
CacheMode                   :
Uid                         :
Visibility                  : Visible
ClipToBounds                : False
Clip                        :
SnapsToDevicePixels         : False
IsFocused                   :
IsEnabled                   :
IsHitTestVisible            :
IsVisible                   : True
Focusable                   :
PersistId                   : 1
IsManipulationEnabled       :
AreAnyTouchesOver           : False
AreAnyTouchesDirectlyOver   :
AreAnyTouchesCapturedWithin : False
AreAnyTouchesCaptured       :
TouchesCaptured             : {}
TouchesCapturedWithin       : {}
TouchesOver                 : {}
TouchesDirectlyOver         : {}
DependencyObjectType        : System.Windows.DependencyObjectType
IsSealed                    : False
Dispatcher                  : System.Windows.Threading.Dispatcher

```

###  <a name="IsVisible"></a> IsVisible
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores.

 Propiedad Boolean que indica si la herramienta de complemento está visible actualmente en el panel asignado. Si está visible, puede establecer la propiedad **IsVisible** en **$false** para ocultar la herramienta o establecer la propiedad **IsVisible** en **$true** para hacer que una herramienta de complemento esté visible en su pestaña de PowerShell. Tenga en cuenta que al ocultar una herramienta de complemento, esta deja de ser accesible a través de los objetos **CurrentVisibleHorizontalTool** o **CurrentVisibleVerticalTool** y, por lo tanto, no puede hacerse visible usando esta propiedad en ese objeto.

```
# Hide the current tool in the vertical tool pane
$psISE.CurrentVisibleVerticalTool.IsVisible = $false
# Show the first tool on the currently selected PowerShell tab
$psISE.CurrentPowerShellTab.VerticalAddOnTools[0].IsVisible=$true

```

###  <a name="name"></a> Nombre
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores.

 Propiedad de solo lectura que obtiene el nombre de la herramienta de complemento.

```
# Gets the name of the visible vertical pane add-on tool.
$psISE.CurrentVisibleVerticalTool.name
Commands

```

## Consulte también
 [El objeto ISEAddOnToolCollection](The-ISEAddOnToolCollection-Object.md)
 [El modelo de objetos de scripting de ISE de Windows PowerShell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md)
 [Referencia del modelo de objetos de ISE de Windows PowerShell](Windows-PowerShell-ISE-Object-Model-Reference.md)
 [La jerarquía del modelo de objetos de ISE](The-ISE-Object-Model-Hierarchy.md)


<!--HONumber=May16_HO2-->


