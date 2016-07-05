---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Introducción"
ms.technology: powershell
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 46f4e5b231d09952f11387cec7c80fbfce9f9d4b

---

# Introducción

##  **Motivaciones**  
Cuando le concede a alguien el acceso con privilegios a los sistemas, está ampliando el límite de confianza a esa persona.
Esto es un riesgo, ya que los administradores son una superficie expuesta a ataques.
Los ataques internos y las credenciales robadas son una realidad habitual.

##  **No es un problema nuevo**  
Probablemente esté familiarizado con el principio del privilegio mínimo y use algún tipo de control de acceso basado en rol (RBAC) con aplicaciones que lo proporcionan.
Pero la eficacia y la facilidad de uso de estas soluciones suelen verse limitadas por su amplio alcance y su imprecisión.
Además, existen lagunas en la cobertura de RBAC.
Por ejemplo, en Windows, el acceso privilegiado es en gran medida un conmutador binario, que le obliga a conceder permisos innecesarios al agregar usuarios al grupo de administradores.

##  **Just Enough Administration (JEA)** 
Proporciona una plataforma de control de acceso basado en roles, RBAC, mediante la comunicación remota de PowerShell.
*Permite a usuarios específicos realizar tareas administrativas específicas en servidores sin concederles derechos de administrador.*
Esto permite llenar el vacío de las soluciones de RBAC existentes y simplifica la administración de las configuraciones.




<!--HONumber=Jun16_HO4-->


