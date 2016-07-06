---
title: "Descripción de los conceptos importantes de Windows PowerShell"
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 3e601e38-4520-4578-a48d-b6779f1d35ee
translationtype: Human Translation
ms.sourcegitcommit: 03ac4b90d299b316194f1fa932e7dbf62d4b1c8e
ms.openlocfilehash: db5f410c8f84949c969f21ed59ac48a4e31e91fd

---

# Descripción de los conceptos importantes de Windows PowerShell
El diseño de Windows PowerShell integra los conceptos de numerosos entornos diferentes. Varios de ellos son familiares para las personas con experiencia en shells o entornos de programación específicos, pero muy pocas los conocen todos. Observar algunos de estos conceptos nos proporcionará una práctica introducción del shell.

### Los comandos no se basan en texto
A diferencia de los comandos de la interfaz de línea de comandos tradicional, los cmdlets de Windows PowerShell están diseñados para tratar con objetos (información estructurada que es algo más que una mera cadena de caracteres que aparecen en la pantalla). La salida del comando siempre incluye información adicional que puede usarse si es necesaria. Trataremos este tema en profundidad en este documento.

Si en el pasado ha usado herramientas de procesamiento de texto para procesar los datos de la línea de comandos, se dará cuenta de que se comportan de forma diferente si intenta usarlas en Windows PowerShell. En la mayoría de los casos, no se necesitan herramientas de procesamiento de texto para extraer información concreta. Puede acceder a partes de los datos directamente mediante comandos estándar de manipulación de objetos de Windows PowerShell.

### La familia de comandos es extensible
Las interfaces, como Cmd.exe, no le permiten extender directamente el conjunto de comandos integrado. Puede crear herramientas de línea de comandos externas que se ejecuten en Cmd.exe, pero dichas herramientas no tienen servicios, como la integración de la Ayuda, y Cmd.exe no sabe automáticamente que son comandos válidos.

Los comandos binarios nativos de Windows PowerShell, que se conocen como *cmdlets*, se pueden ampliar con los cmdlets que cree y agregue a Windows PowerShell mediante complementos. Los *complementos\- de Windows PowerShell se compilan como las herramientas binarias de cualquier otra interfaz. Puede usarlos para agregar proveedores de Windows PowerShell al shell, así como nuevos cmdlets.

Dada la naturaleza especial de los comandos internos de Windows PowerShell, nos referiremos a ellos como *cmdlets*.

> [!NOTE]
> Windows PowerShell puede ejecutar comandos que no sean cmdlets. No los analizaremos en detalle en la Guía de usuario de Windows PowerShell, pero son útiles para conocer las categorías de tipos de comandos. Windows PowerShell admite scripts que son análogos a los scripts de shell de UNIX y los archivos por lotes de Cmd.exe, pero tienen la extensión de nombre de archivo .ps1. Windows PowerShell también permite crear funciones internas que se pueden usar directamente en la interfaz o en los scripts.

### Windows PowerShell controla la entrada y la presentación de la consola
Cuando se escribe un comando, Windows PowerShell siempre procesa directamente la entrada de la línea de comandos. Windows PowerShell también formatea la salida que se ve en la pantalla. Esto es importante, ya que reduce el trabajo necesario de cada cmdlet y garantiza que siempre se hagan las cosas igual independientemente del cmdlet que se use. Un ejemplo de cómo simplifica esto el trabajo tanto de los usuarios como de los desarrolladores de herramientas es la Ayuda de la línea de comandos.

Las herramientas de línea de comandos tradicionales tienen sus propios esquemas para solicitar y mostrar ayuda. Algunas herramientas de línea de comandos usan **\/?** para desencadenar la presentación de la Ayuda; otras usan **\-?**, **\/H**, o incluso **\/\/**. Algunas mostrarán la Ayuda en una ventana de interfaz gráfica de usuario, en lugar de hacerlo en la pantalla de la consola. Algunas herramientas complejas, como los instaladores de actualizaciones de la aplicación, desempaquetan los archivos internos antes de mostrar su Ayuda. Si usa un parámetro equivocado, la herramienta podría omitir la entrada y empezar a realizar una tarea automáticamente.

Al especificar un comando en Windows PowerShell, Windows PowerShell analiza y preprocesa automáticamente todo lo que se escribe. Si usa el parámetro **\-?** con un cmdlet de Windows PowerShell, siempre significa "mostrarme la Ayuda de este comando". Los desarrolladores de cmdlets no tienen que analizar el comando; solo necesitan proporcionar el texto de la Ayuda.

Es importante saber que las características de la Ayuda de Windows PowerShell están disponibles incluso cuando se ejecutan herramientas de línea de comandos tradicionales en Windows PowerShell. Windows PowerShell procesa los parámetros y envía los resultados a las herramientas externas.

> [!NOTE]
> Si se ejecuta una aplicación de gráficos en Windows PowerShell, se abre la ventana de la aplicación. Windows PowerShell solo interviene solo cuando se procesa la entrada de línea de comandos que se proporciona o la salida de la aplicación que se devuelve a la ventana de la consola; no afecta al funcionamiento interno de la aplicación.

### Windows PowerShell usa la sintaxis de C\#
Windows PowerShell tiene palabras clave y características de su sintaxis que son muy similares a las que se usan en el lenguaje de programación C\#, ya que Windows PowerShell se basa en .NET Framework. El aprendizaje de Windows PowerShell facilitará mucho el aprendizaje de C\#, en caso de que le interese este lenguaje.

Para los que no sean programadores de C\#, esta similitud no es importante. Sin embargo, para aquellos que ya estén familiarizados con C\#, las similitudes pueden facilitar considerablemente el aprendizaje de Windows PowerShell.




<!--HONumber=Jun16_HO4-->


