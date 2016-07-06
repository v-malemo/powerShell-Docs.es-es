---
title: Crear un cuadro de entrada personalizado
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0b12e56c-299f-40ee-afbf-d30d23ed2565
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: 628062cfadcadcec3311dcb8bfdd6bd3b9a9e567

---

# Crear un cuadro de entrada personalizado
Cree un script de un cuadro de entrada gráfico personalizado con las características de creación de formularios de Microsoft .NET Framework de Windows PowerShell 3.0, y las versiones posteriores.

## Crear un cuadro de entrada gráfico personalizado
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
$label.Text = "Please enter the information in the space below:"
$form.Controls.Add($label) 

$textBox = New-Object System.Windows.Forms.TextBox 
$textBox.Location = New-Object System.Drawing.Point(10,40) 
$textBox.Size = New-Object System.Drawing.Size(260,20) 
$form.Controls.Add($textBox) 

$form.Topmost = $True

$form.Add_Shown({$textBox.Select()})
$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
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
$OKButton.Location = New-Object System.Drawing.Point(75,120)
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
$label.Text = "Please enter the information in the space below:"
$form.Controls.Add($label)
```

Agregue el control (en este caso, un cuadro de texto) que permita a los usuarios proporcionar la información descrita en el texto de etiqueta. Aparte de los cuadros de texto, hay otros muchos controles que se pueden aplicar; para conocerlos, vea el tema sobre el [espacio de nombres System.Windows.Forms](http://msdn.microsoft.com/library/k50ex0x9(v=vs.110).aspx) en MSDN.

```
$textBox = New-Object System.Windows.Forms.TextBox 
$textBox.Location = New-Object System.Drawing.Point(10,40) 
$textBox.Size = New-Object System.Drawing.Size(260,20) 
$form.Controls.Add($textBox)
```

Establezca la propiedad **Topmost** en **$True** para forzar que la ventana se abra encima del resto de ventanas y cuadros de diálogo abiertos.

```
$form.Topmost = $True
```

A continuación, agregue esta línea de código para activar el formulario y establezca el foco en el cuadro de texto que ha creado.

```
$form.Add_Shown({$textBox.Select()})
```

Agregue la siguiente línea de código para mostrar el formulario en Windows.

```
$result = $form.ShowDialog()
```

Por último, el código del bloque **If** indica a Windows qué hacer con el formulario después de que los usuarios proporcionen el texto en el cuadro de texto y hagan clic en **Aceptar** o presionen la tecla **Entrar**.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

## Consulte también
[Hey Scripting Guy: Why don’t these PowerShell GUI examples work? (Hey Scripting Guy: ¿Por qué no funcionan estos ejemplos de GUI de PowerShell?)](http://go.microsoft.com/fwlink/?LinkId=506644)
[GitHub: Dave Wyatt's WinFormsExampleUpdates (GitHub: WinFormsExampleUpdates de Dave Wyatt)](https://github.com/dlwyatt/WinFormsExampleUpdates)
[Windows PowerShell Tip of the Week: Creating a Custom Input Box (Sugerencia de la semana de Windows PowerShell: Crear un cuadro de entrada personalizado)](http://technet.microsoft.com/library/ff730941.aspx)




<!--HONumber=Jun16_HO4-->


