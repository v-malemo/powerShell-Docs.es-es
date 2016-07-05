---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: Acerca de la lista negra
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 8892e5e08a763fbc66d782bbc9252d1f3a7dcfcf

---

### Acerca de la lista negra
Ahora que se ha familiarizado con JEA, probablemente se pregunte si es posible incluir comandos en una lista negra.
Se trata de una solicitud comprensible, pero actualmente no está previsto para JEA por las razones siguientes:

1.  Hemos diseñado JEA para limitar a los operadores a las acciones que necesitan realizar.
Una lista negra es lo contrario a esto.

2.  Los autores de los comandos de PowerShell no diseñaron dichos comandos con JEA en mente.
En una instalación nueva de Windows Server 2016, existen aproximadamente 1520 comandos disponibles de inmediato.
Los modelos de riesgos para estos comandos no incluían la posibilidad de que un usuario pudiese ejecutar comandos como una cuenta con más privilegios.
Por ejemplo, determinados comandos permiten la inserción de código por diseño (por ejemplo, Invoke-Command y Add-Type en el módulo principal de PowerShell).
JEA puede avisarle cuando exponga los comandos específicos que conocemos, pero no hemos vuelto a evaluar todos los demás comandos de Windows en función del nuevo modelo de riesgos.
Debe comprender las funcionalidades de los comandos que exponga a través de JEA.  

3.  Además, incluso aunque JEA bloquease todos los comandos con vulnerabilidades de inserción de código, no hay ninguna garantía de que un usuario malintencionado no pueda llevar a cabo una acción incluida en la lista negra con otro comando relacionado.
A menos que comprenda todos los comandos que expone, es imposible garantizar que una acción determinada no es posible.
Usted tiene la responsabilidad de comprender qué comandos expone, tanto si se incluyen en una lista blanca como en una lista negra.
El número de comandos que expondría una lista negra sería imposible de administrar, por lo que JEA se implementa mediante listas blancas.




<!--HONumber=Jun16_HO4-->


