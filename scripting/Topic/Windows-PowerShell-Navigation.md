---
title: Navegación en Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 846f5233-43be-496a-9ca1-e3e34207cb49
---
# Navegación en Windows PowerShell
Las carpetas son una característica organizativa muy conocida de la interfaz del Explorador de archivos, de Cmd.exe y de herramientas de UNIX como BASH. Las carpetas (o directorios, como se les conoce más comúnmente) son un concepto útil para organizar los archivos y otros directorios. Los sistemas operativos de la familia UNIX amplían este concepto considerando todos los elementos posibles como archivos; así, las conexiones de red y de hardware específicas aparecen como archivos dentro carpetas concretas. Este método no garantiza que el contenido vaya a ser legible o utilizable por parte de determinadas aplicaciones, pero hace que sea más sencillo buscar elementos específicos. Las herramientas que enumeran archivos y carpetas o que realizan búsquedas en ellos también funcionan con estos dispositivos. También puede tratar un elemento específico mediante la ruta de acceso al archivo que lo representa.

De forma análoga, la infraestructura de Windows PowerShell admite exponer prácticamente cualquier cosa que permita la navegación, como una unidad de disco estándar de Microsoft Windows o un sistema de archivos UNIX como una unidad de Windows PowerShell. Una unidad de Windows PowerShell no representa necesariamente una unidad real, ya sea local o en la red. En este capítulo se aborda principalmente la navegación en sistemas de archivos, pero estos conceptos son igualmente válidos para las unidades de Windows PowerShell que no estén asociadas a sistemas de archivos.



<!--HONumber=Apr16_HO1-->


