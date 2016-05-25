---
title:  Crear un selector de fecha gráfico
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  jpjofre
manager:  dongill
ms.prod:  powershell
ms.assetid:  c1cb722c-41e9-4baa-be83-59b4653222e9
---

# Crear un selector de fecha gráfico
Use Windows PowerShell 3.0 (y versiones posteriores) para crear un formulario con un control gráfico de estilo de calendario que permita a los usuarios seleccionar un día del mes.

## Crear un control gráfico de selector de fecha
Copie y pegue lo siguiente en Windows PowerShell ISE y, después, guárdelo como un script de Windows PowerShell (.ps1).

```
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object Windows.Forms.Form 

$form.Text = "Select a Date" 
$form.Size = New-Object Drawing.Size @(243,230) 
$form.StartPosition = "CenterScreen"

$calendar = New-Object System.Windows.Forms.MonthCalendar 
$calendar.ShowTodayCircle = $False
$calendar.MaxSelectionCount = 1
$form.Controls.Add($calendar) 

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(38,165)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(113,165)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$form.Topmost = $True

$result = $form.ShowDialog() 

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $date = $calendar.SelectionStart
    Write-Host "Date selected: $($date.ToShortDateString())"
}
```

El script comienza con la carga de dos clases de .NET Framework: **System.Drawing** y **System.Windows.Forms**. A continuación, inicie una nueva instancia de la clase de .NET Framework **Windows.Forms.Form**, que proporciona un formulario o una ventana en blanco en la que puede empezar a agregar controles.

```
$form = New-Object Windows.Forms.Form
```

Tras crear una instancia de la clase Form, asigne valores a las tres propiedades de esta clase.

-   **Text.** Es el título de la ventana.

-   **Size.** Es el tamaño del formulario en píxeles. El script anterior crea un formulario de 243 píxeles de ancho y 230 píxeles de alto.

-   **StartingPosition.** Esta propiedad opcional está establecida en **CenterScreen** en el script anterior. Si no agrega esta propiedad, Windows selecciona una ubicación cuando el formulario se abre. Cuando **StartingPosition** se establece en **CenterScreen**, el formulario aparece automáticamente en el centro de la pantalla cada vez que se carga.

```
$form.Text = "Select a Date" 
$form.Size = New-Object Drawing.Size @(243,230) 
$form.StartPosition = "CenterScreen"
```

A continuación, cree un control de calendario y agréguelo al formulario. En este ejemplo, el día actual no está resaltado o dentro de un círculo. Los usuarios pueden seleccionar un solo día en el calendario cada vez.

```
$calendar = New-Object System.Windows.Forms.MonthCalendar 
$calendar.ShowTodayCircle = $False
$calendar.MaxSelectionCount = 1
$form.Controls.Add($calendar)
```

A continuación, cree un botón **Aceptar** para el formulario. Indique un tamaño y el comportamiento del botón **Aceptar**. En este ejemplo, la posición del botón es de 165 píxeles desde el borde superior del formulario y de 38 píxeles desde el borde izquierdo. La altura del botón es 23 píxeles y la longitud, 75 píxeles. El script usa tipos predefinidos de Windows Forms para definir el comportamiento del botón.

```
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(38,165)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = "OK"
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

De manera similar, cree un botón **Cancelar**. El botón **Cancelar** está a 165 píxeles de la parte superior, pero a 113 píxeles del borde izquierdo de la ventana.

```
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(113,165)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = "Cancel"
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

Establezca la propiedad **Topmost** en **$True** para forzar que la ventana se abra encima del resto de ventanas y cuadros de diálogo abiertos.

```
$form.Topmost = $True
```

Agregue la siguiente línea de código para mostrar el formulario en Windows.

```
$result = $form.ShowDialog()
```

Por último, el código del bloque **If** indica a Windows qué hacer con el formulario después de que los usuarios seleccionen un día en el calendario y hagan clic en **Aceptar** o presionen la tecla **Entrar**. Windows PowerShell muestra la fecha seleccionada a los usuarios.

```
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $date = $calendar.SelectionStart
    Write-Host "Date selected: $($date.ToShortDateString())"
}
```

## Consulte también
[Hey Scripting Guy: Why don’t these PowerShell GUI examples work? (Hey Scripting Guy: ¿Por qué no funcionan estos ejemplos de GUI de PowerShell?)](http://go.microsoft.com/fwlink/?LinkId=506644)
[GitHub: Dave Wyatt's WinFormsExampleUpdates (GitHub: WinFormsExampleUpdates de Dave Wyatt)](https://github.com/dlwyatt/WinFormsExampleUpdates)
[Windows PowerShell Tip of the Week: Creating a Graphical Date Picker (Sugerencia de la semana de Windows PowerShell: Crear un selector de fecha gráfico)](http://technet.microsoft.com/library/ff730942.aspx)



<!--HONumber=May16_HO2-->


