---
title: Explorar Windows PowerShell ISE
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0d2c6e8-5126-40e7-a1e1-d1cff29fe94a
---
# Explorar Windows PowerShell ISE
Puede usar el [!INCLUDE[ise_1](../Token/ise_1_md.md)] para crear, ejecutar y depurar comandos y scripts. [!INCLUDE[ise_2](../Token/ise_2_md.md)] consta de la barra de menús, las pestañas de Windows PowerShell, la barra de herramientas, las pestañas de script, un panel de scripts, un panel de consola, una barra de estado, un control deslizante de tamaño de texto y la ayuda contextual.

> [!NOTE]
> A partir de [!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0, los paneles de comandos y de consola están combinados en un único panel de consola.

## Barra de menús
La barra de menús contiene los menús **Archivo**, **Edición**, **Ver**, **Herramientas**, **Depurar**, **Complementos** y **Ayuda**. Los botones de los menús permiten realizar tareas relacionadas con la escritura y ejecución de scripts y la ejecución de comandos en [!INCLUDE[ise_2](../Token/ise_2_md.md)]. Además, se puede colocar un [menú Complementos](https://technet.microsoft.com/en-us/library/412dd662-417a-4661-ada2-558802d0f6d2#submenus) en la barra de menús mediante la ejecución de scripts que usan el [modelo de objetos de scripting de Windows PowerShell ISE](https://technet.microsoft.com/en-us/library/1737ddb7-c20d-4e6b-a0d3-68cc2650f2a1).

> [!NOTE]
> En [!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0, los menús **Herramientas** y **Complementos** no estaban presentes.

## Pestañas de Windows PowerShell
Una pestaña de Windows PowerShell es el entorno en el que se ejecuta un script de Windows PowerShell. Puede abrir nuevas pestañas de Windows PowerShell en [!INCLUDE[ise_2](../Token/ise_2_md.md)] para crear entornos independientes en el equipo local o en equipos remotos. Puede tener un máximo de ocho pestañas de PowerShell abiertas de forma simultánea.

## Barra de herramientas
Los botones siguientes están ubicados en la barra de herramientas.

|Botón|Función|
|----------|------------|
|**New**|Abre un nuevo script.|
|**Abrir**|Abre un script o un archivo existente.|
|**Guardar**|Guarda un script o un archivo.|
|**Cortar**|Corta el texto seleccionado y lo copia en el Portapapeles.|
|**Copiar**|Copia el texto seleccionado en el portapapeles.|
|**Pegar**|Pega el contenido del Portapapeles en la ubicación del cursor.|
|**Borrar panel de salida**|Borra todo el contenido del panel de salida.|
|**Deshacer**|Invierte la acción que se acaba de realizar.|
|**Rehacer**|Realiza la acción que se acaba de deshacer.|
|**Ejecutar secuencia de comandos**|Ejecuta un script.|
|**Ejecutar selección**|Ejecuta una parte seleccionada de un script.|
|**Detener ejecución**|Detiene un script que se está ejecutando.|
|**Nueva pestaña de PowerShell en remoto**|Crea una nueva pestaña de PowerShell que establece una sesión en un equipo remoto. Aparece un cuadro de diálogo que le pide que escriba los detalles necesarios para establecer la conexión remota.|
|**Iniciar PowerShell.exe**|Abre una consola de PowerShell.|
|**Mostrar panel de scripts arriba**|Mueve el panel de scripts a la parte superior de la pantalla.|
|**Mover panel de scripts a la derecha**|Mueve el panel de scripts a la derecha de la pantalla.|
|**Mostrar panel de scripts maximizado**|Maximiza el panel de scripts.|

## Pestaña Script
Muestra el nombre del script que se está editando. Puede hacer clic en una pestaña de script para seleccionar el script que desea editar.

Cuando apunte a la pestaña de script, la ruta de acceso completa al archivo de script se mostrará en una información sobre herramientas.

## Panel de scripts
Permite crear y ejecutar scripts. Puede abrir, editar y ejecutar scripts existentes en el panel de scripts.

## Panel de salida
Muestra los resultados de los comandos y scripts que ha ejecutado. También puede copiar y borrar el contenido en el panel de salida.

## Panel de comandos
Permite escribir comandos. Puede ejecutar un comando de una línea o un comando de varias líneas en el panel de comandos. Presione MAYÚS+ENTRAR para introducir cada línea de un comando de varias líneas y presione ENTRAR después de la última línea para ejecutarlo. El mensaje que aparece en la parte superior del panel de comandos muestra la ruta de acceso al directorio de trabajo actual.

## Barra de estado
Permite ver si los comandos y scripts que ejecuta se han completado. La barra de estado se encuentra en la parte inferior de la pantalla. Las partes seleccionadas de los mensajes de error se muestran en la barra de estado.

## Control deslizante Tamaño del texto
Aumenta o disminuye el tamaño del texto en la pantalla.

## Ayuda
La ayuda de [!INCLUDE[ise_2](../Token/ise_2_md.md)] está disponible en la biblioteca de TechNet en Internet. Para abrir la Ayuda, haga clic en **Ayuda de Windows PowerShell ISE** en el menú **Ayuda** o presione la tecla F1 en cualquier lugar excepto cuando el cursor esté en el nombre de un cmdlet en el panel de scripts o en panel de consola. Desde el menú **Ayuda** también puede ejecutar el cmdlet Update-Help y mostrar la ventana Comandos, que le muestra todos los parámetros de un cmdlet para ayudarle a construir comandos, lo que permite rellenar los parámetros en un formulario fácil de usar.

## Consulte también
[Usar Windows PowerShell ISE](../Topic/Using-the-Windows-PowerShell-ISE.md)



<!--HONumber=Apr16_HO2-->


