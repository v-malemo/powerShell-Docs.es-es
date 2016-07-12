---
description: 
manager: dongill
ms.topic: article
author: jpjofre
ms.prod: powershell
keywords: powershell,cmdlet,jea
ms.date: 2016-06-22
title: "Generación de informes en JEA"
ms.technology: powershell
translationtype: Human Translation
ms.sourcegitcommit: 7504fe496a8913718847e45115d126caf4049bef
ms.openlocfilehash: d867a6462e9fa8b6e16c8c2103899c72b380116c

---

# Generación de informes en JEA
El registro y la auditoría son muy importantes, ya que JEA permite que usuarios sin privilegios se ejecuten en un contexto con privilegios.
En esta sección veremos las herramientas que puede usar como ayuda para el registro y la creación de informes.

## Acciones de generación de informes en JEA
### Transcripción con consentimiento temporal
Una de las formas más rápidas de obtener un resumen de lo que ocurre en una sesión de PowerShell es mirar por encima del hombro de la persona que está escribiendo.
De este modo ve los comandos y la salida de los comandos y comprueba que todo está bien.
O no, pero al menos lo sabe.
La transcripción de PowerShell está diseñada para proporcionarle una vista similar a posteriori.

Cuando se usa el campo "TranscriptDirectory" en la configuración de sesión, PowerShell graba automáticamente una transcripción de todas las acciones realizadas en una sesión determinada.
Verá transcripciones de sus sesiones en el documento que se encuentra aquí: "$env:ProgramData\JEAConfiguration\Transcripts".

Como puede imaginar, la transcripción registra información sobre el usuario conectado, el usuario de ejecución, los comandos que se ejecutaron en la sesión, etc.
Para obtener más información sobre la transcripción de PowerShell, consulte [esta entrada de blog](http://blogs.msdn.com/b/powershell/archive/2015/06/09/powershell-the-blue-team.aspx).

### Registros de eventos de PowerShell
Una vez que ha activado el registro de módulos, también se registran todas las acciones de PowerShell en los registros de eventos de Windows normales.
Esto es ligeramente más complicado que las transcripciones, pero el nivel de detalle que ofrece puede resultarle útil.

En el registro operativo "PowerShell", el identificador de evento 4104 registrará cada comando que se invoque si ha habilitado el registro del módulos.

### Otros registros de eventos
A diferencia de los registros y las transcripciones de PowerShell, otros mecanismos de registro no capturan al "usuario conectado".
Deberá establecer alguna correlación entre los demás registros y los registros de PowerShell para que las acciones realizadas coincidan.

En el registro operativo "Administración remota de Windows", el identificador de evento 193 registrará el SID y el nombre del usuario que se conecta, así como el SID de la cuenta virtual de ejecución para ayudar a esta correlación.
Probablemente habrá observado que el nombre de la cuenta virtual de ejecución incluye al final el dominio y el nombre del usuario que se conecta.

## Configuración de la generación de informes en JEA
### Get-PSSessionConfiguration
Para realizar informes sobre el estado de su entorno, es importante saber cuántos puntos de conexión de JEA ha configurado en la máquina.
`Get-PSSessionConfiguration` hace precisamente eso.

### Get-PSSessionCapability
La realización manual de informes sobre las funcionalidades de un usuario dado a través de un punto de conexión de JEA puede ser bastante compleja.
Probablemente tendría que inspeccionar varias funcionalidades de rol.
Por suerte, el cmdlet "Get-PSSessionCapability" lo hace automáticamente.

Para probarlo, ejecute el comando siguiente desde un símbolo del sistema de administrador de PowerShell:
```PowerShell
Get-PSSessionCapability -Username 'CONTOSO\OperatorUser' -ConfigurationName JEADemo
```




<!--HONumber=Jul16_HO1-->


