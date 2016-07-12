---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Conceptos clave usados a lo largo de esta guía"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: 178fea44987b0c457b8e5d23fbe851ee12f03b31

---

# Conceptos clave usados a lo largo de esta guía
**¿Qué es exactamente JEA?**

JEA es una extensión de [puntos de conexión restringidos](http://blogs.technet.com/b/heyscriptingguy/archive/2014/03/31/introduction-to-powershell-endpoints.aspx) de PowerShell que agrega definiciones de rol, cuentas virtuales y otras mejoras que ayudan a bloquear los puntos de conexión de administración.
Un punto de conexión de JEA consta de un archivo de configuración de sesión de PowerShell y uno o más archivos de funcionalidad de rol.

**¿Qué son los archivos de configuración de sesión y de funcionalidad de rol?**

Los archivos de configuración de sesión de PowerShell (.pssc) definen *quién* se puede conectar a un punto de conexión de PowerShell y *cómo* está configurado.
Aquí es donde se asignan los usuarios y los grupos de seguridad a los roles de administración específicos y se configuran valores globales como las cuentas virtuales y las directivas de transcripción.
Los archivos de configuración de sesión son específicos de cada máquina, lo que le permite controlar el acceso por máquina, si le interesa.

Los archivos de funcionalidad de rol de PowerShell (.psrc) definen *qué* pueden hacer en el sistema los usuarios que pertenecen a cada rol.
Aquí puede restringir qué cmdlets, funciones, proveedores y programas externos puede usar un usuario en su sesión de JEA.
Los archivos de funcionalidad de rol suelen ser genéricos del rol al que se está atendiendo (administrador de DNS, departamento de soporte técnico de nivel 1, auditoría de inventario de solo lectura, etc.) y pertenecen a los módulos de PowerShell, lo que facilita compartirlos con el entorno y con otros usuarios.

**¿Cómo aprovecha JEA las cuentas virtuales?**

En el archivo de configuración de sesión de PowerShell, puede configurar las sesiones de JEA para que usen cuentas de ejecución virtuales.
Las cuentas virtuales son cuentas con privilegios de un solo uso creadas para el usuario específico que se conecta en esa sesión específica en el contexto en el que se ejecutan los comandos del usuario.
Las cuentas virtuales pertenecen de forma predeterminada al grupo de seguridad local "Administradores", pero se pueden configurar opcionalmente para que solo pertenezcan a los grupos de seguridad que especifique.

**Comunicación remota de PowerShell**: permite ejecutar comandos de PowerShell en máquinas remotas.
Puede operar en uno o varios equipos y usar conexiones temporales o permanentes.
En esta demostración, obtuvo acceso de manera remota a la máquina local con una sesión interactiva.
JEA restringe la funcionalidad disponible a través de Comunicación remota de PowerShell.
Para obtener más información sobre Comunicación remota de PowerShell, ejecute `Get-Help about_Remote`.

**Usuario de ejecución**: en JEA, un usuario sin privilegios de administrador se ejecuta como una cuenta virtual con privilegios.
La cuenta virtual solo dura mientras se produzca la sesión remota.
Es decir, se crea cuando un usuario se conecta al punto de conexión y se destruye cuando el usuario finaliza la sesión.
De forma predeterminada, la cuenta virtual es miembro del grupo de administradores local.
En un controlador de dominio, es miembro de los administradores de dominio.
Las cuentas virtuales son locales en la máquina en la que se crean y no tienen permisos fuera de esa máquina.
Esto significa que no están registradas en Active Directory (no se les ha asignado ningún RID).
Además, si un comando o script permitido intenta obtener acceso a recursos fuera de la máquina local, obtendrá acceso a esos recursos con la identidad de la máquina, no con la identidad de la cuenta virtual.

**Usuario conectado**: usuario sin privilegios de administrador que se conecta al punto de conexión de JEA y al que se asignan roles.
Los comandos que ejecute este usuario se ejecutan en el contexto del usuario de ejecución o de la cuenta virtual.




<!--HONumber=Jul16_HO1-->


