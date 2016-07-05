---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "ARCHIVO LÉAME"
ms.technology: powershell
ms.sourcegitcommit: 47593773fb0f34e0b52d35617522d7b1db7f48e6
ms.openlocfilehash: 1cecf1b6bf5a55ed785c8ff43bdd4a0a41e96cd0

---

# Just Enough Administration
Just Enough Administration (JEA) es una tecnología de seguridad que permite la administración delegada de todo lo que se puede administrar con PowerShell.
Con JEA, se puede hacer lo siguiente:
- **Reducir el número de administradores de las máquinas** aprovechando las cuentas virtuales que realizan acciones con privilegios en nombre de usuarios normales.
- **Limitar lo que pueden hacer los usuarios** especificando qué cmdlets, funciones y comandos externos pueden ejecutar.
- **Entender mejor lo que hacen los usuarios** con transcripciones "con consentimiento temporal" que muestran exactamente qué comandos ejecutó un usuario durante una sesión.

**¿Por qué es importante?**  
Considere el escenario común en el que los servidores DNS coexisten con los controladores de dominio de Active Directory.
Los administradores DNS necesitan tener privilegios de administrador local para solucionar problemas con el servidor DNS, pero para ello tiene que hacerlos miembros del grupo de seguridad "Administradores de dominio" con privilegios elevados.
Este enfoque proporciona a los administradores de DNS el control sobre todo el dominio y les permite acceder a todos los recursos de la máquina.

Con JEA, puede configurar un rol para los administradores DNS que les proporcione acceso a todos los comandos que necesitan para realizar su trabajo, pero a nada más.
Esto significa que puede proporcionar el acceso adecuado para reparar una caché DNS dudosa sin concederles involuntariamente derechos en Active Directory, para examinar el sistema de archivos o para ejecutar scripts potencialmente peligrosos.
Y lo que es mejor, cuando la sesión de JEA está configurada para usar cuentas virtuales con privilegios de un solo uso, los administradores de DNS pueden conectarse al servidor mediante credenciales *sin privilegios* y seguir siendo capaces de ejecutar comandos con privilegios.

## Disponibilidad
JEA se está desarrollando en paralelo a Windows Server 2016, y está disponible en versiones anteriores de Windows mediante actualizaciones de Windows Management Framework.
La versión actual de JEA está disponible en las siguientes plataformas:

**Windows Server**
- Windows Server 2016 Technical Preview 4 y versiones posteriores
- Windows Server 2012 R2, 2012 y 2008 R2\* con [Windows Management Framework 5.0](https://www.microsoft.com/en-us/download/details.aspx?id=50395) instalado

**Cliente de Windows**
- Windows 10 con la actualización de noviembre (1511) instalada
- Windows 8.1, 8 y 7\* con [Windows Management Framework 5.0](https://www.microsoft.com/en-us/download/details.aspx?id=50395) instalado

\* La compatibilidad con cuentas virtuales en sesiones de JEA no está disponible en Windows Server 2008 R2 o Windows 7.


## Explorar la guía de la experiencia
¿Listo para aprender a crear, implementar y utilizar su propio punto de conexión JEA?

Esta guía incluye una rápida introducción con un punto de conexión de JEA predefinido para que se haga una idea de cómo es la experiencia del usuario final y, después, describe el proceso de crear un punto de conexión desde cero para demostrar conceptos como las configuraciones de sesión y las funcionalidades de rol.

1.  [Introducción](introduction.md)   
Revise brevemente por qué debería interesarle JEA.

2.  [Requisitos previos](prerequisites.md)  
Explica cómo configurar su entorno.

3.  [Uso de JEA](using-jea.md)  
Ayuda a entender la experiencia de usuario de JEA.

4.  [Recreación de la demostración](remake-the-demo-endpoint.md)  
Cree una configuración de sesión de JEA desde cero.

5.  [Funcionalidades de rol](role-capabilities.md)  
Aprenda cómo personalizar las funcionalidades JEA con archivos de funcionalidad de rol.

6.  [De un extremo a otro: Active Directory](end-to-end---active-directory.md)  
Cree un punto de conexión completamente nuevo para administrar Active Directory.

7.  [Implementación y mantenimiento de varias máquinas](multi-machine-deployment-and-maintenance.md)  
Descubra cómo cambia la implementación y la creación según la escala.

8.  [Generación de informes en JEA](reporting-on-jea.md)  
Descubra cómo auditar e informar sobre todas las acciones y la infraestructura de JEA.

9.  Apéndices
  - [Conceptos clave usados a lo largo de esta guía](key-concepts-used-throughout-this-guide.md)  
  -  [Creación de un controlador de dominio](creating-a-domain-controller.md)  
  -  [Acerca de la lista negra](on-blacklisting.md)  
  -  [Consideraciones al limitar comandos](considerations-when-limiting-commands.md)  
  -  [Dificultades comunes de las funcionalidades de rol](common-role-capability-pitfalls.md)

## Empiece a crear sus propios puntos de conexión de JEA
Es fácil crear un punto de conexión de JEA: lo único que necesita es un sistema habilitado para JEA y un editor de texto (como PowerShell ISE).
Se recomienda que, para empezar, cree archivos esqueleto mediante `New-PSRoleCapabilityFile -Path <path>` y `New-PSSessionCapabilityFile -Path <Path>` sin ningún otro argumento.
Estos archivos esqueleto contienen todos los campos de configuración aplicables, junto con comentarios útiles que explican para qué puede usarse cada campo.

Para facilitar la creación de puntos de conexión de JEA, consulte el [JEA Toolkit Helper](http://blogs.technet.com/b/privatecloud/archive/2015/12/20/introducing-the-updated-jea-helper-tool.aspx) (Asistente para el kit de herramientas de JEA), que proporciona una GUI con la que puede crear archivos de configuración de sesión y funcionalidad de rol.
Incluso permite generar funcionalidades de rol basadas en los registros de PowerShell para que tome como punto de partida los comandos que los usuarios ejecutan habitualmente para realizar su trabajo.




<!--HONumber=Jun16_HO4-->


