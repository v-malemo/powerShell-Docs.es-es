---
title: Entorno de scripting integrado (ISE) de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f156b92d-0203-46d2-89c7-b4989d32e3d2
---
# Entorno de scripting integrado (ISE) de Windows PowerShell
El Entorno de scripting integrado (ISE ) de Windows PowerShell es uno de los dos hosts para el motor y el lenguaje de Windows PowerShell. Puede escribir, ejecutar y probar scripts de maneras que no están disponibles en la consola de Windows PowerShell. El ISE agrega color de sintaxis, finalización con tabulación, IntelliSense, depuración visual y ayuda contextual.

El ISE permite ejecutar comandos en un panel de consola, pero también admite paneles que se pueden usar para ver simultáneamente el código fuente del script y otras herramientas que pueden conectarse al ISE. También puede abrir varias ventanas de script al mismo tiempo, lo cual es especialmente útil cuando se depura un script que usa las funciones definidas en otros scripts o módulos.

## <a name="BKMK_NEW"></a>Novedades
Estas son algunas de las características que se agregaron al ISE en las versiones más recientes de PowerShell.

### Agregado en PowerShell 3.0 (Windows Server 2012, Windows 8)
**Intellisense** completa automáticamente sus comandos mostrando menús de cmdlets, parámetros, valores de parámetros, archivos o carpetas coincidentes a medida que escribe.

**Fragmentos de código** son secciones de código breves que puede insertar fácilmente en los scripts que escribe. Una colección de fragmentos de código útiles se incluye en el cuadro y puede agregar más mediante el cmdlet **New-Snippet**.

Se pueden crear **herramientas de complemento** que agreguen características al ISE mediante la escritura de código que interactúe con el [modelo de objetos de scripting de Windows PowerShell ISE](https://technet.microsoft.com/en-us/library/dd819478.aspx). Estas herramientas pueden mostrar controles en un panel con fichas o funcionar de manera invisible en segundo plano. El complemento **Comandos** es un buen ejemplo. Se incluye con la versión 3.0 y posteriores, que muestra una lista de los comandos disponibles y su ayuda.

**Administrador de reinicio y Autoguardar** permite guardar automáticamente los scripts cada dos minutos para ayudar a evitar la pérdida del trabajo en caso de un bloqueo o reinicio inesperado.

Ahora, la **lista Usados más recientemente** forma parte del menú Abrir archivo para que sea más fácil obtener acceso a los archivos que se usan con mayor frecuencia.

**Panel de consola combinado**. En versiones anteriores del ISE, los paneles de comandos y salida eran independientes. Ahora se combinan en un solo panel que representa más directamente lo que se ve en la consola de Windows Powershell.

**Modificadores de línea de comandos** Varios modificadores de línea de comandos nuevos le ofrecen un mayor control sobre el funcionamiento del ISE. –NoProfile inicia el ISE sin ejecutar un script de perfil. –Help abre una ventana de ayuda con el ISE. –mta inicia el ISE en "modo de contenedor multiproceso". El modo predeterminado es el de uniproceso.

Las **nuevas características del editor** facilitan la creación y lectura del código:

-   **Color de la sintaxis XML**. El editor de ISE colorea ahora la sintaxis XML de la misma manera que la sintaxis de código de Windows PowerShell.

-   **Coincidencia de llaves**. El ISE[!INCLUDE[ise_2](../Token/ise_2_md.md)] destaca las llaves coincidentes para que pueda asegurarse de tener el número correcto de llaves de cierre según el número de llaves de apertura. Use CTRL-[ para ubicar la llave de cierre que coincida con la llave de apertura en la que se encuentra en cursor.

-   **Vista Esquema**. Puede contraer o expandir las secciones del código haciendo clic en los signos más y menos del margen izquierdo. De este modo, podrá encontrar fácilmente el código que busca en un script largo.

-   **Editar texto con arrastrar y colocar**. Puede seleccionar un bloque de texto y arrastrarlo a otra ubicación para moverlo. Si mantiene presionada la tecla Ctrl mientras arrastra el texto seleccionado, lo copiará en lugar de moverlo.

-   **Presentación de errores de análisis**. Windows PowerShell examina el script a medida que escribe. Si detecta un error, muestra un zigzag rojo bajo el código incorrecto. Al mantener el puntero sobre el error indicado, la información sobre herramientas muestra el problema encontrado.

-   **Zoom**. Puede acercar el texto para que resulte más fácil de leer o alejarlo para ver la imagen más grande mediante el control deslizante de la esquina inferior derecha de la ventana del ISE.

-   **Copia y pegado de texto enriquecido**. Al copia texto del ISE en el Portapapeles, se incluye la información de fuente, tamaño y de color del texto seleccionado.

-   **Selección de bloques**. Para seleccionar un fragmento de texto en forma de bloque, puede mantener presionada la tecla ALT y seleccionar texto en el panel de scripts con el mouse, o bien presionar **Alt+Mayús+Flecha**.

### Agregado en PowerShell 2.0 (Windows Server 2008 R2, Windows 7)
El ISE se introdujo con PowerShell 2.0.

## Requisitos para ejecutar Windows PowerShell ISE
El ISE está disponible en cualquier equipo que puede ejecutar Windows PowerShell 2.0 o una versión posterior. Cada versión de Windows y Windows Server incluye una versión de Windows PowerShell y el ISE, pero se puede actualizar a la versión más reciente disponible mediante la instalación de Windows Management Framework. Realice esta búsqueda para encontrar la versión más reciente disponible: [Descargas](http://www.microsoft.com/en-us/search/DownloadResults.aspx?q=%22windows%20management%20framework%22%20PowerShell&sortby=Relevancy~Descending). Tenga en cuenta que todas las entradas etiquetadas como "Vista previa" son código de versión preliminar y no una característica completa.

> [!NOTE]
> Dado que [!INCLUDE[ise_2](../Token/ise_2_md.md)] requiere una interfaz gráfica de usuario, no puede ejecutar la opción Server Core de Windows Server.

## <a name="BKMK_LINKS"></a>Vea también
[Uso del Entorno de scripting integrado de Windows PowerShell](http://technet.microsoft.com/library/cc732148.aspx)



<!--HONumber=Apr16_HO2-->


