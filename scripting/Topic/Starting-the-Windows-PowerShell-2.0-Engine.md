---
title: Iniciar el motor de Windows PowerShell 2.0
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: edafc2fa-7576-49c2-bbba-9336f4bcfc28
---
# Iniciar el motor de Windows PowerShell 2.0
En esta sección se explica cómo iniciar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] en [!INCLUDE[winblue_client_2](../Token/winblue_client_2_md.md)], [!INCLUDE[winblue_server_2](../Token/winblue_server_2_md.md)], [!INCLUDE[win8_client_2](../Token/win8_client_2_md.md)] y [!INCLUDE[win8_server_2](../Token/win8_server_2_md.md)], que incluyen el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], así como en otros sistemas que tengan instalado [!INCLUDE[psversion2](../Token/psversion2_md.md)], [!INCLUDE[psversion3](../Token/psversion3_md.md)] y [!INCLUDE[psversion4](../Token/psversion4_md.md)].

[!INCLUDE[psversion4](../Token/psversion4_md.md)] y [!INCLUDE[psversion3](../Token/psversion3_md.md)] están diseñados para ser compatibles con versiones anteriores de [!INCLUDE[psversion2](../Token/psversion2_md.md)]. Los cmdlets, proveedores, complementos, módulos y scripts escritos para [!INCLUDE[psversion2](../Token/psversion2_md.md)] se ejecutan sin cambios en [!INCLUDE[psversion4](../Token/psversion4_md.md)] y [!INCLUDE[psversion3](../Token/psversion3_md.md)]. Sin embargo, debido a un cambio en la directiva de activación en tiempo de ejecución de Microsoft .NET Framework 4, los programas host de [!INCLUDE[wps_2](../Token/wps_2_md.md)] escritos para [!INCLUDE[psversion2](../Token/psversion2_md.md)] y compilados con Common Language Runtime (CLR) 2.0 no se puede ejecutar sin modificaciones en [!INCLUDE[psversion3](../Token/psversion3_md.md)] ni [!INCLUDE[psversion4](../Token/psversion4_md.md)] compilados con CLR 4.0. El motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] está destinado a usarse solo cuando un programa host o un script existente no se puede ejecutar porque no es compatible con [!INCLUDE[psversion4](../Token/psversion4_md.md)], [!INCLUDE[psversion3](../Token/psversion3_md.md)] o Microsoft .NET Framework 4. Estos casos deben ser infrecuentes.

Muchos programas que requieren el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] lo inician automáticamente. Estas instrucciones se incluyen para las situaciones excepcionales en que necesite iniciar el motor de forma manual.

## Instalar y habilitar los programas requeridos
Antes de iniciar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], habilite el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] y Microsoft .NET Framework 3.5 con Service Pack 1. Para obtener instrucciones, consulte [instalar Windows PowerShell](../Topic/Installing-Windows-PowerShell.md).

Los sistemas con [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) o [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)] instalado tienen todos los componentes necesarios. No es necesario realizar ninguna otra configuración. Para obtener información acerca de cómo instalar [Windows Management Framework 4.0](http://go.microsoft.com/fwlink/?LinkID=293881) o [!INCLUDE[ps_wmf_3.0](../Token/ps_wmf_3.0_md.md)], consulte [Instalar Windows PowerShell](../Topic/Installing-Windows-PowerShell.md).

## Cómo iniciar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]
Al iniciar [!INCLUDE[mshshort](../Token/mshshort_md.md)], se inicia la versión más reciente de forma predeterminada. Para iniciar [!INCLUDE[mshshort](../Token/mshshort_md.md)] con el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], use el parámetro Version de PowerShell.exe. Puede ejecutar el comando en cualquier símbolo del sistema, incluidos Windows PowerShell y Cmd.exe.

```
PowerShell.exe -Version 2
```

## Cómo iniciar una sesión remota con el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]
Para ejecutar el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] en una sesión remota, cree una configuración de sesión (también conocida como "punto de conexión") en el equipo remoto que carga el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]. La configuración de sesión se guarda en el equipo remoto y puede usarla cualquier usuario autorizado para crear sesiones que usen el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

Se trata de una tarea avanzada que suele realizarla un administrador del sistema.

El siguiente procedimiento usa el parámetro **PSVersion** del cmdlet [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) para crear una configuración de sesión que use el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]. También puede usar el parámetro **PowerShellVersion** del cmdlet [New-PSSessionConfigurationFile](https://technet.microsoft.com/en-us/library/5f3e3633-6e90-479c-aea9-ba45a1954866) para crear un archivo de configuración de sesión para una sesión que cargue el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] y el parámetro **PSVersion** del parámetro [Set-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/b21fbad3-1759-4260-b206-dcb8431cd6ea) para cambiar una configuración de sesión para que use el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)].

Para más información sobre los archivos de configuración de sesión, vea [about_Session_Configuration_Files](https://technet.microsoft.com/en-us/library/c7217447-1ebf-477b-a8ef-4dbe9a1473b8). Para obtener información sobre las configuraciones de sesión, incluida la de configuración y seguridad,vea [about_Session_Configurations [v4]](https://technet.microsoft.com/en-us/library/a2fbe12a-350c-4d04-be50-24102824e3ab).

#### Para iniciar una sesión de [!INCLUDE[psversion2](../Token/psversion2_md.md)]remoto

1.  Para crear una configuración de sesión que requiera el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], use el parámetro **PSVersion** del cmdlet [Register-PSSessionConfiguration](https://technet.microsoft.com/en-us/library/e9152ae2-bd6d-4056-9bc7-dc1893aa29ea) con un valor de "2.0". Ejecute este comando en el equipo en el "lado servidor" o en el extremo receptor de la conexión.

    El siguiente comando de ejemplo crea la configuración de sesión PS2 en el equipo Server01. Para ejecutar este comando, inicie [!INCLUDE[psversion4](../Token/psversion4_md.md)] o [!INCLUDE[psversion3](../Token/psversion3_md.md)] con la opción **Ejecutar como administrador**.

    ```
    Register-PSSessionConfiguration -Name PS2 -PSVersion 2.0
    ```

2.  Para crear una sesión en el equipo Server01 que use la configuración de sesión PS2, use el parámetro **ConfigurationName** de los cmdlets que crean una sesión remota, como el cmdlet [New-PSSession](https://technet.microsoft.com/en-us/library/76f6628c-054c-4eda-ba7a-a6f28daaa26f).

    Cuando se inicia una sesión que usa la configuración de sesión, el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)] se carga automáticamente en la sesión.

    El comando siguiente inicia una sesión en el equipo Server01 que usa la configuración de sesión PS2. El comando guarda la sesión en la variable $s.

    ```
    $s = New-PSSession -ComputerName Server01 -ConfigurationName PS2
    ```

## Cómo iniciar un trabajo en segundo plano con el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]
Para iniciar un trabajo en segundo plano con el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)], use el parámetro **PSVersion** del cmdlet [Start-Job](https://technet.microsoft.com/en-us/library/2bc04935-0deb-4ec0-b856-d7290cca6442).

El comando siguiente inicia un trabajo en segundo plano con el motor de [!INCLUDE[psversion2](../Token/psversion2_md.md)]

```
Start-Job {Get-Process} -PSVersion 2.0
```

Para más información sobre los trabajos en segundo plano, consulte [about_Jobs [v4]](https://technet.microsoft.com/en-us/library/7362512a-8a4e-4575-b2ea-a740e5c4f002).



<!--HONumber=Apr16_HO2-->


