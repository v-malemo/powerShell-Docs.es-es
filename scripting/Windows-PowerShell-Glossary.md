---
title: Glosario de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f88cbe-cb83-4912-a301-184534cb35c7
---
# Glosario de Windows PowerShell


|Término|Definición|
|--------|--------------|
|módulo binario|Módulo de Windows PowerShell cuyo módulo raíz es un archivo de módulo binario (.dll). Un módulo binario puede o no incluir un manifiesto de módulo.|
|parámetro común|Parámetro que el motor de Windows PowerShell agrega a todos los cmdlets y a las funciones avanzadas.|
|usar el operador punto|En Windows PowerShell, iniciar un comando escribiendo un punto y un espacio antes del comando. Los comandos que usan el operador punto se ejecutan en el ámbito actual en lugar de en un nuevo ámbito. Las variables, los alias, las funciones o las unidades que crea el comando se crean en el ámbito actual y están disponibles para los usuarios cuando se completa el comando.|
|módulo dinámico|Módulo que solo existe en memoria. El cmdlet Import-PSSession crea módulos dinámicos.|
|parámetro dinámico|Parámetro que se agrega a un cmdlet, una función o un script de Windows PowerShell en determinadas condiciones. Los cmdlets, las funciones, los proveedores y los scripts pueden agregar parámetros dinámicos.|
|archivo de formato|Archivo XML de Windows PowerShell con la extensión .format.ps1xml que define cómo Windows PowerShell muestra un objeto según su tipo de .NET Framework.|
|estado de sesión global|Estado de sesión que contiene los datos que son accesibles para el usuario de una sesión de Windows PowerShell.|
|host|Interfaz que el motor de Windows PowerShell usa para comunicarse con el usuario. Por ejemplo, el host especifica cómo se controlan los mensajes entre Windows PowerShell y el usuario.|
|aplicación host|Programa que carga el motor de Windows PowerShell en su proceso y lo usa para realizar operaciones.|
|método de procesamiento de entrada|Método que un cmdlet puede usar para procesar los registros que recibe como entrada. Los métodos de procesamiento de entrada incluyen el método BeginProcessing, el método ProcessRecord, el método EndProcessing y el método StopProcessing.|
|módulo de manifiesto|Módulo de Windows PowerShell que tiene un manifiesto y cuya clave ModulesToProcess está vacía.|
|manifiesto de módulo|Archivo de datos de Windows PowerShell (.psd1) que describe el contenido de un módulo y que controla cómo se procesa un módulo.|
|estado de sesión del módulo|Estado de sesión que contiene los datos públicos y privados de un módulo de Windows PowerShell. Los datos privados en este estado de sesión no están disponibles para el usuario de una sesión de Windows PowerShell.|
|error de no terminación|Error que no impide que Windows PowerShell continúe procesando el comando.|
|nombre|Palabra que sigue al guión en un nombre de cmdlet de Windows PowerShell. El nombre describe los recursos en los que actúa el cmdlet.|
|conjunto de parámetros|Grupo de parámetros que pueden usarse en el mismo comando para realizar una acción específica.|
|canalizar|En Windows PowerShell, enviar los resultados del comando anterior como entrada para el comando siguiente en la canalización.|
|canalización|Serie de comandos conectados mediante operadores de canalización (&#124;) (ASCII 124). Cada operador de canalización envía los resultados del comando anterior como entrada para el comando siguiente.|
|PSSession|Tipo de sesión de Windows PowerShell que crea, administra y cierra el usuario.|
|módulo raíz|Módulo especificado en la clave ModuleToProcess en un manifiesto de módulo.|
|espacio de ejecución|En Windows PowerShell, entorno operativo en el que se ejecuta cada comando de una canalización.|
|bloque de script|En el lenguaje de programación de Windows PowerShell, colección de instrucciones o expresiones que se pueden usar como una sola unidad. Un bloque de script puede aceptar argumentos y valores devueltos.|
|módulo de script|Módulo de Windows PowerShell cuyo módulo raíz es un archivo de módulo de script (.psm1). Un módulo binario puede o no incluir un manifiesto de módulo.|
|archivo de módulo de script|Archivo que contiene un script de Windows PowerShell. El script define los miembros que exporta el módulo de script. Los archivos de módulo de script tienen la extensión de nombre de archivo .psm1.|
|shell|Intérprete de comandos que se usa para pasar comandos al sistema operativo.|
|parámetro de modificador|Parámetro que no toma un argumento.|
|error de terminación|Error que impide que Windows PowerShell termine de procesar el comando.|
|transacción|Unidad atómica de trabajo. El trabajo de una transacción debe completarse como un todo; si se produce un error en cualquier parte de la transacción, no se puede realizar la transacción.|
|archivo de tipos|Archivo XML de Windows PowerShell que tiene la extensión .ps1xml y que extiende las propiedades de los tipos de Microsoft .NET Framework en Windows PowerShell.|
|verbo|Palabra que precede al guión en un nombre de cmdlet de Windows PowerShell. El verbo describe la acción que realiza el cmdlet.|
|Windows PowerShell|Tecnología de shell de línea de comandos y de scripting basado en tareas que permite a los administradores de TI controlar completamente y automatizar las tareas de administración del sistema.|
|Comando de Windows PowerShell|Elemento de una canalización que provocan que se lleve a cabo una acción. Los comandos de Windows PowerShell se escriben con el teclado o se invocan mediante programación.|
|Archivo de datos de Windows PowerShell|Archivo de texto que tiene la extensión de nombre de archivo .psd1. Windows PowerShell usa los archivos de datos para distintos propósitos, como almacenar datos de manifiesto de módulo y almacenar cadenas traducidas para la internacionalización del script.|
|Unidad de Windows PowerShell|Unidad virtual que proporciona acceso directo a un almacén de datos. La puede definir un proveedor de Windows PowerShell o se puede crear en la línea de comandos. Las unidades creadas en la línea de comandos son unidades específicas de la sesión y se pierden cuando se cierra la sesión.|
|Entorno de scripting integrado (ISE) de Windows PowerShell|Aplicación de host de Windows PowerShell que permite ejecutar comandos, así como escribir, probar y depurar scripts en un sencillo entorno con color de sintaxis compatible con Unicode.|
|Módulo de Windows PowerShell|Unidad reutilizable independiente que permite la partición, la organización y el resumen del código de Windows PowerShell. Un módulo puede contener cmdlets, proveedores, funciones, variables y otros tipos de recursos que pueden importarse como una sola unidad.|
|Proveedor de Windows PowerShell|Programa basado en Microsoft .NET Framework que hace que los datos de un almacén de datos especializado estén disponibles en Windows PowerShell, de modo que pueda verlos y administrarlos.|
|Script de Windows PowerShell|Script escrito en el lenguaje de Windows PowerShell.|
|Archivo de script de Windows PowerShell|Archivo que tiene la extensión .ps1 y que contiene un script escrito en el lenguaje de Windows PowerShell.|
|Complemento de Windows PowerShell|Recurso que define un conjunto de cmdlets, proveedores y tipos de Microsoft .NET Framework que se pueden agregar al entorno de Windows PowerShell.|
|Flujo de trabajo de Windows PowerShell|Un flujo de trabajo es una secuencia de pasos conectados y programados que realizan tareas de larga duración o requieren la coordinación de varios pasos a través en varios dispositivos o nodos administrados. El flujo de trabajo de Windows PowerShell permite a los profesionales de TI y desarrolladores crear secuencias de actividades de administración de varios dispositivos o tareas únicas dentro de un flujo de trabajo, como flujos de trabajo. El flujo de trabajo de Windows PowerShell permite adaptar y ejecutar scripts de Windows PowerShell y archivos XAML como flujos de trabajo.|



<!--HONumber=May16_HO2-->


