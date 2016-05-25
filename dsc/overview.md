---
title:   Información general sobre la configuración de estado deseado de Windows PowerShell 
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Información general sobre la configuración de estado deseado de Windows PowerShell 

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

En este tema se describe la característica de configuración de estado deseado (DSC) de Windows PowerShell en Windows PowerShell. Puede utilizar este tema para obtener una visión general de DSC y para encontrar los recursos de documentación que necesita para comprender y usar DSC.

## Descripción de la característica
DSC es una nueva plataforma de administración de Windows PowerShell que permite implementar y administrar datos de configuración de servicios de software y administrar el entorno en el que se ejecutan estos servicios.

DSC ofrece un conjunto de extensiones de lenguaje de Windows PowerShell, nuevos cmdlets de Windows PowerShell y recursos que se pueden usar para especificar mediante declaración cómo quiere configurar el entorno del software. También proporciona un medio para mantener y administrar las configuraciones existentes.

## Aplicaciones prácticas
A continuación se muestran algunos escenarios de ejemplo donde puede usar recursos integrados de DSC para configurar y administrar un conjunto de equipos (también conocidos como nodos de destino) de forma automática:

* Habilitar o deshabilitar características y roles de servidor.
* Administrar la configuración del Registro.
* Administrar archivos y directorios.
* Iniciar, detener y administrar procesos y servicios.
* Administrar grupos y cuentas de usuario.
* Implementar software nuevo.
* Administrar variables de entorno.
* Ejecutar scripts de Windows PowerShell.
* Corregir una configuración que se ha desplazado del estado deseado.
* Detectar el estado de la configuración real en un nodo determinado.

## Conceptos clave
DSC es una plataforma declarativa que se usa para la configuración, implementación y administración de sistemas. Consta de tres componentes principales:

* Las [configuraciones](configurations.md) son scripts de PowerShell declarativos que definen y configuran instancias de recursos. Cuando ejecuta la configuración, DSC (y los recursos a los que llama la configuración) simplemente "hará que sea así", asegurándose de que el sistema exista en el estado que disponga la configuración. Las configuraciones DSC también son idempotentes: el administrador de configuración local (LCM) seguirá garantizando que las máquinas estén configuradas en el estado que la configuración declare.
* Los recursos son los bloques de creación fundamentales de DSC que se escriben para modelar los distintos componentes de un subsistema e implementar el flujo de control de sus estados cambiantes. Residen dentro de los módulos de PowerShell y pueden escribirse para modelar algo tan genérico como un archivo o un proceso de Windows, o algo tan específico como un servidor IIS o una máquina virtual que se ejecuta en Azure.
* El administrador de configuración local (LCM) es el motor mediante el que DSC facilita la interacción entre recursos y configuraciones. El LCM sondea periódicamente el sistema mediante el flujo de control que han implementado los recursos para asegurarse de que se mantiene el estado que ha dispuesto un elemento Configuration. Si el sistema está fuera del estado, el LCM usa más lógica dentro de los recursos para "hacer que sea así" según la declaración del elemento Configuration. 

DSC también incluye una serie de nuevas palabras clave, cmdlets y herramientas del lenguaje que permiten la creación de configuraciones, ayudan a crear recursos de DSC, a invocar configuraciones y a administrar el LCM. Muchos de estos cmdlets se pueden encontrar en Windows 8.1 como parte del módulo PSDesiredStateConfiguration (entre los que se incluyen `Start-DscConfiguration`, `Set-DscLocalConfigurationManager` y `Get-DscResource`). xDscResourceDesigner (que se encuentra en la [Galería de PowerShell](https://www.powershellgallery.com/packages/xDSCResourceDesigner/)) es una recopilación de cmdlets que simplifican el desarrollo de los recursos de DSC.

## Consulte también
* [Configuraciones DSC](configurations.md)
* [Recursos de DSC](resources.md)
* [Configuración del administrador de configuración local](metaConfig.md)



<!--HONumber=May16_HO3-->


