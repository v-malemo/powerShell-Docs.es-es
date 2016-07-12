---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: consideraciones al limitar comandos
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 9f3f79a29e0fb7ec5a5111284bb7985548e17749

---

### Consideraciones al limitar comandos
En este paso, hay que hacer hincapié en un aspecto importante.
Es fundamental que todas las funcionalidades que se exponen a través de JEA se encuentren en áreas restringidas por el administrador.
Los usuarios sin privilegios de administrador no deben tener la capacidad de modificar los scripts que se usan a través de puntos de conexión de JEA.

Además, es fundamental que no ofrezca a los usuarios de JEA la posibilidad de sobrescribir configuraciones de JEA y scripts incluidos en una lista blanca dentro de sus sesiones de JEA.
Es muy importante que tenga mucho cuidado al exponer comandos como `Copy-Item`.




<!--HONumber=Jul16_HO1-->


