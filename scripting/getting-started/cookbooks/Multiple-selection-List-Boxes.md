---
title:  Cuadros de lista de selección múltiple
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  f74cd5d9-da57-4802-b614-0b194a7bc8f8
---

# Cuadros de lista de selección múltiple
Use Windows PowerShell 3.0 (y versiones posteriores) para crear un control de cuadro de lista de selección múltiple en un formulario de Windows Forms personalizado.

## Crear controles de cuadro de lista que permiten selecciones múltiples
Copie y pegue lo siguiente en Windows PowerShell ISE y, después, guárdelo como un script de Windows PowerShell (.ps1).

```
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form 
$form.Text = "Data Entry Form"
$form.Size = New-Object System.Drawing.Size(300,200) 
$form.StartPosition = "CenterScreen"

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20) 
$label.Size = New-Object System.Drawing.Size(280,20) 
$label.Text = "Please make a selection from the list below:"
$form.Controls.Add($label) 

$listBox = New-Object System.Windows.Forms.Listbox 
$listBox.Location = New-Object System.Drawing.Point(10,40) 
$listBox.Size = New-Object System.Drawing.Size(260,20) 

$listBox.SelectionMode = "MultiExtended"

[void] $listBox.Items.Add("Item 1")
[void] $listBox.Items.Add("Item 2")
[void] $listBox.Items.Add("Item 3")
[void] $listBox.Items.Add("Item 4")
[void] $listBox.Items.Add("Item 5")

$listBox.Height = 70
$form.Controls.Add($listBox) 
$form.Topmost = $True

$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItems
    $x
}
```

El script comienza con la carga de dos clases de .NET Framework: **System.Drawing** y **System.Windows.Forms**. A continuación, se inicia una nueva instancia de la clase de .NET Framework **System.Windows.Forms.Form**, que proporciona un formulario o ventana en blanco en la que puede empezar a agregar controles.

```
$form = New-Object System.Windows.Forms.Form
```

Tras crear una instancia de la clase Form, asigne valores a las tres propiedades de esta clase.

-   **Text.** Es el título de la ventana.

-   **Size.** Es el tamaño del formulario en píxeles. El script anterior crea un formulario de 300 píxeles de ancho y 200 píxeles de alto.

-   **StartingPosition.** Esta propiedad opcional está establecida en **CenterScreen** en el script anterior. Si no agrega esta propiedad, Windows selecciona una ubicación cuando el formulario se abre. Cuando **StartingPosition** se establece en **CenterScreen**, el formulario aparece automáticamente en el centro de la pantalla cada vez que se carga.

```
$form.Text = "Data Entry Form"
$form.Size = New-Object System.Drawing.Size(300,200) 
$form.StartPosition = "CenterScreen"
```

A continuación, cree un botón **Aceptar** para el formulario. Indique un tamaño y el comportamiento del botón **Aceptar**. En este ejemplo, la posición del botón es de 120 píxeles desde el borde superior del formulario y de 75 píxeles desde el borde izquierdo. La altura del botón es 23 píxeles y la longitud, 75 píxeles. El script usa tipos predefinidos de Windows Forms para definir el comportamiento del botón.

```
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Size(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

De manera similar, cree un botón **Cancelar**. El botón **Cancelar** está a 120 píxeles de la parte superior, pero a 150 píxeles del borde izquierdo de la ventana.

```
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

A continuación, escriba texto de etiqueta en la ventana para describir la información que quiera que los usuarios proporcionen.

```
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20) 
$label.Size = New-Object System.Drawing.Size(280,20) 
$label.Text = "Please make a selection from the list below:"
$form.Controls.Add($label)
```

Agregue el control (en este caso, un cuadro de lista) que permita a los usuarios proporcionar la información descrita en el texto de etiqueta. Aparte de los cuadros de texto, hay otros muchos controles que se pueden aplicar; para conocerlos, vea el tema sobre el [espacio de nombres System.Windows.Forms](http://msdn.microsoft.com/library/k50ex0x9(v=vs.110).aspx) en MSDN.

```
$listBox = New-Object System.Windows.Forms.Listbox 
$listBox.Location = New-Object System.Drawing.Point(10,40) 
$listBox.Size = New-Object System.Drawing.Size(260,20)
```

Aquí indicamos cómo especificar que se quiere permitir a los usuarios seleccionar varios valores en la lista.

```
$listBox.SelectionMode = "MultiExtended"
```

En la sección siguiente, hay que especificar los valores que quiere que el cuadro de lista muestre a los usuarios.

```
[void] $listBox.Items.Add("Item 1")
[void] $listBox.Items.Add("Item 2")
[void] $listBox.Items.Add("Item 3")
[void] $listBox.Items.Add("Item 4")
[void] $listBox.Items.Add("Item 5")
```

Especifique el alto máximo del control de cuadro de lista.

```
$listBox.Height = 70
```

Agregue el control de cuadro de lista al formulario e indique a Windows que, cuando el formulario se abra, lo haga encima del resto de ventanas y cuadros de diálogo abiertos.

```
$form.Controls.Add($listBox) 
$form.Topmost = $True
```

Agregue la siguiente línea de código para mostrar el formulario en Windows.

```
$result = $form.ShowDialog()
```

Por último, el código del bloque **If** indica a Windows qué hacer con el formulario después de que los usuarios seleccionen una o varias opciones en el cuadro de lista y hagan clic en **Aceptar** o presionen la tecla **Entrar**.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItems
    $x
}
```

## Consulte también
[Hey Scripting Guy: Why don’t these PowerShell GUI examples work? (Hey Scripting Guy: ¿Por qué no funcionan estos ejemplos de GUI de PowerShell?)](http://go.microsoft.com/fwlink/?LinkId=506644)
[GitHub: Dave Wyatt's WinFormsExampleUpdates (GitHub: WinFormsExampleUpdates de Dave Wyatt)](https://github.com/dlwyatt/WinFormsExampleUpdates)
[Windows PowerShell Tip of the Week: Multi-Select List Boxes – And More! (Sugerencia de Windows PowerShell de la semana: Cuadros de lista multiselección y mucho más)](http://technet.microsoft.com/library/ff730950.aspx)



<!--HONumber=May16_HO2-->


