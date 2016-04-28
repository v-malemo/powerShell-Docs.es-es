---
title: Usar expansión de pestañas
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8730471-bf6a-43b8-ab1d-f9ef5a74f04e
---
# Usar expansión de pestañas
Los shells de línea de comandos suelen ofrecer un método para completar los nombres de archivos largos o comandos automáticamente, lo que acelera la entrada de comando y proporciona sugerencias. Windows PowerShell permite rellenar los nombres de archivo y los nombres de cmdlet presionando la tecla **Tab**.

> [!NOTE]
> La función interna TabExpansion controla la expansión de pestañas. Puesto que esta función se puede modificar o reemplazar, este artículo es una guía para el comportamiento de la configuración predeterminada de Windows PowerShell.

Para rellenar automáticamente un nombre de archivo o una ruta de acceso desde las opciones disponibles, escriba parte del nombre y presione la tecla **Tab**. Windows PowerShell expandirá automáticamente el nombre a la primera coincidencia que encuentre. Al presionar la tecla **Tab** repetidamente recorrerá todas las opciones disponibles.

La expansión de pestañas de nombres de cmdlet es ligeramente diferente. Para usar la expansión de pestañas en un nombre de cmdlet, escriba la primera parte completa del nombre (verbo) y el guion que aparece detrás. Puede seguir rellenando el nombre para obtener una coincidencia parcial. Por ejemplo, si escribe **get-co** y, después, presiona la tecla **Tab**, Windows PowerShell lo expandirá automáticamente al cmdlet **Get-Command** (observe que también cambia las letras mayúsculas y minúsculas a su formato estándar). Si vuelve a presionar la tecla **Tab**, Windows PowerShell lo reemplazará por el otro único nombre de cmdlet coincidente, **Get-Content**.

Puede usar la expansión de pestañas repetidamente en la misma línea. Por ejemplo, puede escribir lo siguiente para usar la expansión de pestañas en el nombre del cmdlet **Get-Content**:

```
PS> Get-Con<Tab>
```

Cuando se presiona la tecla **Tab**, el comando se expande a:

```
PS> Get-Content
```

Luego, puede especificar parcialmente la ruta de acceso al archivo de registro de instalación activa y volver a usar la expansión de pestañas:

```
PS> Get-Content c:\windows\acts<Tab>
```

Cuando se presiona la tecla **Tab**, el comando se expande a:

```
PS> Get-Content C:\windows\actsetup.log
```

> [!NOTE]
> Una limitación del proceso de expansión de pestañas es que las pestañas se interpretan siempre como intentos de completar una palabra. Si copia y pega ejemplos de comandos en una consola de Windows PowerShell, asegúrese de que el ejemplo no contiene pestañas; si es así, los resultados serán impredecibles y seguramente no serán los esperados.



<!--HONumber=Apr16_HO1-->


