# Información general sobre la configuración de estado deseado para responsables de toma de decisiones #

En este documento se describen los beneficios de negocio del uso de la configuración de estado deseado (DSC) de PowerShell. No se trata de una guía técnica.

## ¿Qué es la configuración de estado deseado? ##

La configuración de estado deseado (DSC) de Windows PowerShell ofrece una plataforma de configuración integrada en Windows que se basa en estándares abiertos. DSC es lo suficientemente flexible como para funcionar de forma confiable y coherente en cada una de las etapas del ciclo de vida de implementación (desarrollo, prueba, preproducción, producción), así como durante el escalado horizontal. 

DSC se centra en la idea de "[configuraciones](https://msdn.microsoft.com/en-us/powershell/dsc/configurations)", que son documentos fáciles de leer que describen un entorno compuesto por equipos ("nodos") con características específicas. Estas características pueden ser tan sencillas como garantizar que una característica concreta de Windows está habilitada, o tan complejas como la implementación de SharePoint. 

DSC también tiene supervisión y generación de informes integradas. Si un sistema ya no es conforme, DSC puede generar una alerta y actuar para corregir el sistema. 

## Ventajas de usar la configuración de estado deseado ##

Las configuraciones están diseñadas para que se puedan leer, almacenar y actualizar fácilmente. Las configuraciones simplemente declaran el estado en que deberían estar los dispositivos de destino, en lugar de escribir instrucciones sobre cómo hacer que lleguen a ese estado. Esto permite que la configuración mediante DSC sea mucho menos costosa de aprender, adoptar, implementar y mantener. 

La creación de configuraciones implica que se capturen los pasos de implementación complejos como un "origen único de verdad" en una ubicación única. Esto hace que las implementaciones repetidas de un conjunto concreto de máquinas sean mucho menos propensas a errores. A su vez, esto hace que las implementaciones sean más rápidas y confiables. Esto permite un tiempo de entrega rápido en las implementaciones complejas.

Las configuraciones también pueden compartirse a través de la galería de PowerShell. Esto significa que puede que ya existan escenarios comunes y procedimientos recomendados para el trabajo que necesita.


## Configuración de estado deseado y DevOps ##

[DevOps](http://blogs.technet.com/b/ashleymcglone/archive/2015/11/20/devops-for-n00bs-ie-windows-people.aspx) es una combinación de personas, tecnologías y referencias culturales que permiten una implementación e iteración rápidas. DSC se diseñó con DevOps en mente. Que una sola configuración defina un entorno significa que los desarrolladores pueden codificar sus requisitos en una configuración, incorporar esa configuración en el control de código fuente, y los equipos de operaciones pueden implementar fácilmente el código sin tener que realizar procesos manuales propensos a errores. 

Las configuraciones también están [controladas por datos](https://msdn.microsoft.com/en-us/powershell/dsc/configdata), lo que facilita que los equipos de operaciones identifiquen y cambien los entornos sin intervención del programador. 

## Configuración de estado deseado local y remota ##

DSC se puede utilizar para administrar implementaciones locales y remotas. Para las soluciones locales, la configuración de estado deseado tiene un [servidor de extracción](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) que puede utilizarse para centralizar la administración de máquinas e informar sobre su estado. Para las soluciones de nube, la configuración de estado deseado se puede utilizar siempre que se pueda usar Windows. También hay ofertas específicas de Azure basadas en la configuración de estado deseado, como [Automatización de Azure](https://azure.microsoft.com/en-us/documentation/services/automation/), que centraliza la creación de informes de la configuración de estado deseado. 

## DSC y la compatibilidad ##

Aunque DSC se introdujo en Windows Server 2012 R2, está disponible para los sistemas operativos inferiores mediante el paquete Windows Management Framework (WMF). Puede encontrar más información sobre WMF en la [página de aterrizaje de PowerShell](https://msdn.microsoft.com/en-us/powershell/). 

DSC también se puede usar para administrar Linux. Para más información, consulte [Introducción a DSC para Linux](https://msdn.microsoft.com/en-us/powershell/dsc/lnxgettingstarted).<!--HONumber=Feb16_HO4-->
