---
title: Novedades de Windows PowerShell ISE
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38648d47-7c27-4b37-a40e-ad29948519c2
---
# Novedades de Windows PowerShell ISE
En este tema se explican las características nuevas y actualizadas que se han introducido en las versiones de [!INCLUDE[ise_1](../Token/ise_1_md.md)].

## <a name="overview"></a>Descripción de la característica
[!INCLUDE[ise_2](../Token/ise_2_md.md)] es una aplicación host que permite escribir, ejecutar y probar scripts y módulos en un entorno gráfico e intuitivo. Las características principales, como el color de la sintaxis, la finalización con tabulación, la depuración visual, la compatibilidad con Unicode y la Ayuda contextual, proporcionan una rica experiencia de scripting.

Para obtener una introducción de [!INCLUDE[ise_2](../Token/ise_2_md.md)], consulte [Introducción a Windows PowerShell Integrated Scripting Environment](assetId:///3c1892c2-bf84-4cb6-af26-1f453be9e671).

## <a name="versions"></a>Funciones nuevas y modificadas en Windows PowerShell ISE
En la tabla siguiente se enumeran las características nuevas y modificadas de esta versión de [!INCLUDE[ise_2](../Token/ise_2_md.md)] en [!INCLUDE[wps_2](../Token/wps_2_md.md)].

|Característica o funcionalidad|[!INCLUDE[ise_2](../Token/ise_2_md.md)] 4.0|[!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0|[!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0|
|--------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------------------------|
|**[Intellisense](#BKMK_Intellisense)**|X|X||
|**[Fragmentos de código](#bkmk_snippets)**|X|X||
|**[Herramientas de complemento](#BKMK_AddOnTools)**|X|X||
|**[Administrador de reinicio y Autoguardar](#BKMK_RestartMgr)**|X|X||
|**[Panel de consola](#BKMK_ConsolePane)**|X|X||
|**[Lista Usados más recientemente](#BKMK_MRU)**|X|X||
|**[Modificadores de línea de comandos](#BKMK_CommandLine)**|X|X||
|**[Nuevas características del editor](#BKMK_NewEditorFeatures)**|X|X||
|**[Ventana del nuevo visor de Ayuda](#BKMK_NewHelpViewer)**|X|X||
|**[Cmdlet Show-Command](#BKMK_ShowCommand)**|X|X||

### <a name="BKMK_Intellisense"></a>Intellisense
**Agregado en ISE 3.0**

IntelliSense es una característica de asistencia de finalización automática que forma parte de [!INCLUDE[ise_2](../Token/ise_2_md.md)]. Intellisense muestra menús en los que se pueden hacer clic de los cmdlets, parámetros, valores de parámetros, archivos o carpetas potencialmente coincidentes a medida que escribe.

**¿Qué valor aporta este cambio?**

Con la adición de Intellisense, es más fácil detectar cmdlets y la sintaxis cuando se usa [!INCLUDE[ise_2](../Token/ise_2_md.md)] para crear scripts. También puede usar [!INCLUDE[ise_2](../Token/ise_2_md.md)] para obtener información sobre [!INCLUDE[wps_2](../Token/wps_2_md.md)] al crear nuevos scripts.

**¿Qué funciona de manera diferente?**

Si escribe cmdlets en [!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0 o posterior, se muestra un menú desplazable en el que puede realizar sus selecciones, que permite examinar y seleccionar los comandos adecuados.

### <a name="BKMK_Snippets"></a>Fragmentos de código
**Agregado en ISE 3.0**

Los *fragmentos de código* son secciones de código de [!INCLUDE[wps_2](../Token/wps_2_md.md)] breves que puede insertar fácilmente en los scripts que crea en [!INCLUDE[ise_2](../Token/ise_2_md.md)]. [!INCLUDE[ise_2](../Token/ise_2_md.md)] incluye un conjunto predeterminado de fragmentos de código. Puede agregar fragmentos de código mediante el cmdlet **New-Snippet** mientras trabaja en [!INCLUDE[ise_2](../Token/ise_2_md.md)].

**¿Qué valor aporta este cambio?**

El uso de fragmentos de código permite ensamblar y crear rápidamente scripts para automatizar el entorno.

**¿Qué funciona de manera diferente?**

Para utilizar fragmentos de código en [!INCLUDE[wps_2](../Token/wps_2_md.md)] 3.0 o posterior, en el menú **Edición**, haga clic en **Iniciar fragmentos de código** o presione **Ctrl-J**.

### <a name="BKMK_AddOnTools"></a>Herramientas de complemento
**Agregado en PowerShell 3.0**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] es ahora compatible las herramientas de complemento, que son controles de Windows Presentation Foundation (WPF) que se agregan mediante el modelo de objetos. Las herramientas de complemento se pueden visualizar en la consola en un panel vertical u horizontal. Si hay varias herramientas de complemento en un panel, se mostrarán como un control de pestaña. También puede agregar o quitar herramientas de complemento no producidas por Microsoft. Para obtener más información sobre cómo importar o quitar herramientas de complemento, consulte el tema sobre las [operaciones de Windows PowerShell ISE](http://technet.microsoft.com/library/cc732148.aspx).

**¿Qué valor aporta este cambio?**

Los complementos permiten ampliar y personalizar [!INCLUDE[ise_2](../Token/ise_2_md.md)] con herramientas que pueden mejorar su experiencia de scripting o agregar funcionalidad a [!INCLUDE[ise_2](../Token/ise_2_md.md)].

**¿Qué funciona de manera diferente?**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0 y versiones posteriores incluyen el complemento **Commands**. El complemento **Commands** permite examinar cmdlets y acceder a la ayuda sobre los cmdlets en paralelo con los paneles de **scripts** y de **consola**.

Puede encontrar complementos adicionales mediante el comando **Abrir sitio web de herramientas de complemento** del menú **Complementos**.

### <a name="BKMK_RestartMgr"></a>Administrador de reinicio y Autoguardar
**Agregado en PowerShell 3.0**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] guarda ahora automáticamente los scripts abiertos cada dos minutos en una ubicación aparte.  Si [!INCLUDE[ise_2](../Token/ise_2_md.md)] deja de funcionar o si el sistema operativo se reinicia, al reiniciar [!INCLUDE[ise_2](../Token/ise_2_md.md)] se recuperarán los scripts que estaban abiertos en la última sesión, aunque no se hayan guardado.

Para cambiar el intervalo de guardado automático, ejecute el siguiente comando en el panel de consola: **$psise.Options.AutoSaveMinuteInterval**.

**¿Qué valor aporta este cambio?**

Ahora puede trabajar en [!INCLUDE[ise_2](../Token/ise_2_md.md)] con la tranquilidad de saber que los scripts abiertos se guardan automáticamente en el caso de un reinicio inesperado.

**¿Qué funciona de manera diferente?**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0 no guarda los scripts automáticamente en caso de un reinicio.

### <a name="BKMK_MRU"></a>Lista Usados más recientemente
**Agregado en PowerShell 3.0**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] tiene ahora una lista Usados más recientemente para los archivos. Al abrir un archivo en [!INCLUDE[ise_2](../Token/ise_2_md.md)], este se agrega a la lista Usados más recientemente en el menú **Archivo**.

Para cambiar el número predeterminado de archivos en la lista Usados más recientemente, ejecute el siguiente comando en el panel de consola: **$psise.Options.MruCount**.

**¿Qué valor aporta este cambio?**

Ahora puede usar la lista Usados más recientemente para acceder fácilmente a los archivos que usa con frecuencia.

**¿Qué funciona de manera diferente?**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0 no tiene una lista Usados más recientemente.

### <a name="BKMK_ConsolePane"></a>Panel de consola
**Agregado en PowerShell 3.0**

Los paneles de comandos y de salida separados que estaban disponibles en la primera versión de [!INCLUDE[ise_2](../Token/ise_2_md.md)] se han combinado en un solo panel de consola. El panel de consola es similar en funciones y apariencia a una consola típica de [!INCLUDE[wps_2](../Token/wps_2_md.md)], pero incluye las siguientes mejoras (la mayoría se describen en este tema).

-   El color de la sintaxis del texto de entrada (no texto de salida), incluida la sintaxis XML

-   Intellisense

-   Coincidencia de llaves

-   Indicación de errores

-   Compatibilidad total con Unicode

-   **F1**: ayuda contextual

-   **Ctrl+F1**: Show-Command contextual

-   Compatibilidad con scripts complejos y de derecha a izquierda

-   Compatibilidad con fuentes

-   Zoom

-   Modos de selección de líneas y bloques

-   Conservación del contenido escrito en la línea de comandos cuando presiona la flecha **Arriba** para ver el historial en la consola

**¿Qué valor aporta este cambio?**

La adición de estos cambios del panel de consola proporciona una experiencia de scripting más coherente con la interfaz de la consola.

**¿Qué funciona de manera diferente?**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0 presenta paneles de comandos y de salida independientes.

### <a name="BKMK_CommandLine"></a>Modificadores de línea de comandos
**Agregado en PowerShell 3.0**

Si inicia [!INCLUDE[ise_2](../Token/ise_2_md.md)] desde la línea de comandos (escribiendo **Powershell\_ise.exe**), puede agregar los siguientes modificadores de línea de comandos nuevos:

-   *-NoProfile*: inicia [!INCLUDE[ise_2](../Token/ise_2_md.md)] sin ejecutar **$profile**.

-   *-Help*: muestra una ventana de Ayuda.

-   *-mta*: inicia [!INCLUDE[ise_2](../Token/ise_2_md.md)] en modo de contenedor multiproceso. El modo de operación predeterminado de [!INCLUDE[ise_2](../Token/ise_2_md.md)] es el modo de contenedor uniproceso o *-sta*.

**¿Qué valor aporta este cambio?**

La adición de estos modificadores de línea de comandos permite controlar el entorno en el que se ejecuta [!INCLUDE[ise_2](../Token/ise_2_md.md)].

**¿Qué funciona de manera diferente?**

[!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0 no reconoce estos modificadores de línea de comandos.

### <a name="BKMK_NewEditorFeatures"></a>Nuevas características del editor
**Agregado en PowerShell 3.0**

Otras características de edición de [!INCLUDE[ise_2](../Token/ise_2_md.md)] son:

-   **Color de la sintaxis XML**: [!INCLUDE[ise_2](../Token/ise_2_md.md)] colorea ahora la sintaxis XML de la misma manera que colorea la sintaxis de [!INCLUDE[wps_2](../Token/wps_2_md.md)].

-   **Coincidencia de llaves**: [!INCLUDE[ise_2](../Token/ise_2_md.md)] incluye coincidencia de llaves y resaltado, y puede usarse de las siguientes formas: (por ejemplo, con el comando **Ir a Igualar** o **Ctrl + ]** localiza la llave de cierre, si tiene seleccionada una llave de apertura).

-   **Vista Esquema**: el panel de scripts admite la esquematización, que permite expandir y contraer secciones de código al hacer clic en los signos más o menos en el margen izquierdo. Puede usar llaves o las etiquetas **#region** y **#endregion** para marcar el inicio o el final de una sección que se puede contraer. Para expandir o contraer todas las regiones, presione **Ctrl + M**.

-   **Edición de texto con arrastrar y colocar**: [!INCLUDE[ise_2](../Token/ise_2_md.md)] permite ahora la edición de texto con arrastrar y colocar. Puede seleccionar cualquier bloque de texto y arrastrar el texto a otra ubicación en el editor o la consola para mover el texto. Si mantiene presionada la tecla Ctrl mientras arrastra el texto seleccionado, al soltar el botón del mouse, el texto se copiará en la nueva ubicación. En esta versión de [!INCLUDE[ise_2](../Token/ise_2_md.md)], así como la versión anterior de [!INCLUDE[ise_2](../Token/ise_2_md.md)], al arrastrar y colocar archivos en [!INCLUDE[ise_2](../Token/ise_2_md.md)], [!INCLUDE[ise_2](../Token/ise_2_md.md)] abre el archivo.

-   **Presentación de errores de análisis**: los errores de análisis se indican con un subrayado rojo. Cuando desplaza el cursor sobre un error indicado, el texto de información sobre herramientas muestra el problema que se encontró en el código.

-   **Zoom**: el porcentaje de zoom del contenido de la consola puede establecerse mediante el control deslizante del zoom (en la esquina inferior derecha de la ventana de [!INCLUDE[ise_2](../Token/ise_2_md.md)]) o escribiendo el comando **$psise.options.Zoom** en el panel de consola.

-   **Copiar y pegar texto enriquecido**: la copia en el Portapapeles de [!INCLUDE[ise_2](../Token/ise_2_md.md)] conserva la información de fuente, tamaño y color de la selección original.

-   **Selección de bloques**: para seleccionar un bloque de texto puede mantener presionada la tecla ALT y seleccionar texto en el panel de scripts con el mouse, o bien presionar **Alt+Mayús+Flecha**.

**¿Qué valor aporta este cambio?**

Las características de edición adicionales proporcionan un entorno de edición más coherente y eficaz.

**¿Qué funciona de manera diferente?**

Estas mejoras de edición no estaban presentes en [!INCLUDE[ise_2](../Token/ise_2_md.md)] 2.0.

### <a name="BKMK_NewHelpViewer"></a>Ventana del nuevo visor de Ayuda
**Agregado en PowerShell 3.0**

Si presiona **F1** cuando el cursor está en un cmdlet, o si tiene parte de un cmdlet resaltado, el nuevo visor de Ayuda abre la ayuda contextual sobre el cmdlet resaltado. Para ver la Ayuda acerca de [!INCLUDE[wps_2](../Token/wps_2_md.md)], escriba **operators** en el panel de consola y, a continuación, presione **F1**.

Antes de usar esta característica, descargue la versión más actual de los temas de Ayuda de [!INCLUDE[wps_2](../Token/wps_2_md.md)] desde el sitio web de Microsoft. El método más sencillo para la descarga de los temas de Ayuda es ejecutar el cmdlet **Update-Help** en el panel de consola al ejecutar [!INCLUDE[ise_2](../Token/ise_2_md.md)] como administrador.

Puede modificar donde busca ayuda la tecla **F1**. En el menú **Herramientas**/**Opciones**, en la pestaña **Configuración general**, debajo de **Otra configuración**, puede activar o desactivar la casilla **Usar Ayuda local en lugar de contenido en línea**. Si la activa, el cliente busca la Ayuda del cmdlet en la Ayuda descargada en la carpeta de módulos.  Si la casilla se desactiva, el cliente busca en la biblioteca de TechNet para obtener la Ayuda del cmdlet.

**¿Qué valor aporta este cambio?**

La ayuda contextual sin salir del cmdlet o script actual proporciona una experiencia de aprendizaje sin problemas.

**¿Qué funciona de manera diferente?**

Al presionar F1 en versiones anteriores de [!INCLUDE[ise_2](../Token/ise_2_md.md)] se abría el archivo de Ayuda en el equipo local. En [!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0 y versiones posteriores, se abre una ventana que contiene la Ayuda del cmdlet, que es configurable y permite realizar búsquedas. Esta experiencia de la Ayuda es una novedad de [!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0, y la ayuda actualizable es una novedad de [!INCLUDE[wps_2](../Token/wps_2_md.md)] 3.0.

### <a name="BKMK_ShowCommand"></a>Cmdlet Show-Command
**Agregado en PowerShell 3.0**

El cmdlet **Show-Command** permite crear o ejecutar un cmdlet o una función rellenando formulario gráfico. El formulario permite a los usuarios trabajar con [!INCLUDE[wps_2](../Token/wps_2_md.md)] en un entorno gráfico. **Show-Command** también permite a los generadores de scripts avanzados crear una GUI rápida basada en [!INCLUDE[wps_2](../Token/wps_2_md.md)].

**¿Qué valor aporta este cambio?**

Si usa **Show-Command** en sus scripts de [!INCLUDE[wps_2](../Token/wps_2_md.md)], puede proporcionar a los usuarios con el entorno gráfico con el que está familiarizados. **Show-Command** también puede ayudar a los nuevos usuarios a aprender a usar [!INCLUDE[wps_2](../Token/wps_2_md.md)].

**¿Qué funciona de manera diferente?**

Show-Command es nuevo en [!INCLUDE[ise_2](../Token/ise_2_md.md)] 3.0.

## <a name="BKMK_LINKS"></a>Vea también
Para obtener más información sobre el uso de [!INCLUDE[ise_2](../Token/ise_2_md.md)] en [!INCLUDE[wps_2](../Token/wps_2_md.md)], consulte los vínculos siguientes.

-   [Uso del Entorno de scripting integrado de Windows PowerShell](assetId:///777ea82b-dd73-4269-b61a-69a17e6ff16f)

-   [ISE en el Wiki de TechNet](http://social.technet.microsoft.com/wiki/search/searchresults.aspx?q=ISE)

-   [Centro de scripts](http://technet.microsoft.com/scriptcenter/default)



<!--HONumber=Apr16_HO1-->


