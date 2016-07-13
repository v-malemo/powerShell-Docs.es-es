---
title: Preparativos para usar Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6dc7052d-cc5a-4220-950f-98f963a2b587
---
# Preparativos para usar Windows PowerShell
Cuando instale e inicie Windows PowerShell ISE, considere las siguientes opciones de configuración. Puede realizar estas tareas en cualquier momento.

-   **Instalar los archivos de ayuda.** Los cmdlets que se incluyen en Windows PowerShell 3.0 no se suministran con archivos de ayuda. Sin embargo, puede usar el cmdlet [Update-Help](https://technet.microsoft.com/en-us/library/93e1d870-ace6-432b-8778-8920291d7545) para descargar e instalar los archivos de ayuda más recientes en el equipo. Una vez instalados los archivos, puede usar el cmdlet [Get-Help](https://technet.microsoft.com/en-us/library/1f46eeb4-49d7-4bec-bb29-395d9b42f54a) para que se muestren directamente en la línea de comandos. Para más información, consulte [about_Updatable_Help](https://technet.microsoft.com/en-us/library/10bba75c-f4ac-4ca1-bbf3-8f34dd521ffe) (Acerca de la ayuda actualizable).

    Si decide no instalar los archivos de ayuda, puede leer los temas de ayuda en línea. Para encontrar la versión en línea del tema de ayuda de cualquier cmdlet, escriba: `Get-Help <CmdletName> -Online`. Para examinar los temas de ayuda de Windows PowerShell en la biblioteca de TechNet, comience en [http://go.microsoft.com/fwlink/?LinkID=107116](http://go.microsoft.com/fwlink/?LinkID=107116).

-   **Ejecutar scripts.** Para proteger Windows PowerShell, la directiva de ejecución predeterminada en Windows PowerShell es **Restringido**. Esta directiva permite ejecutar cmdlets, pero no scripts. Para ejecutar scripts, use el cmdlet [Set-ExecutionPolicy[PSITPro5_Security]](https://technet.microsoft.com/en-us/library/5690a0e1-495b-4e63-8280-65ead7bf01ab) para cambiar la directiva de ejecución a **AllSigned** o **RemoteSigned**. Solo los miembros del grupo Administradores en el equipo pueden ejecutar este cmdlet. Para más información, consulte [about_Execution_Policies [v4]](https://technet.microsoft.com/en-us/library/347708dc-1515-4d74-978b-8334603472e6) (Acerca de las directivas de ejecución [v4]).

-   **Habilitar la comunicación remota.** El sistema ya está configurado para que pueda ejecutar comandos remotos en otros equipos. En Windows Server 2012 R2 y Windows Server 2012, el sistema también está configurado para recibir comandos remotos, es decir, para permitir que otros equipos ejecuten comandos remotos en el equipo local. Para habilitar equipos que ejecuten otras versiones de Windows para recibir comandos remotos, ejecute el cmdlet [Enable-PSRemoting](https://technet.microsoft.com/en-us/library/19437c28-33b8-4ac1-9113-8439cc8beffb) en el equipo que desea administrar de forma remota. Solo los miembros del grupo Administradores en el equipo pueden ejecutar este cmdlet. Para más información, consulte [about_Remote](https://technet.microsoft.com/en-us/library/9b4a5c87-9162-4adf-bdfe-fbc80b9b8970) (Acerca de los comandos remotos).

    NOTA: Si la comunicación remota está habilitada en un equipo que ejecuta Windows Server 2.0, lo sigue estando después de instalar Windows Management Framework 3.0. Sin embargo, en Windows Server 2008 (no Windows Server 2008 R2), debe volver a habilitar la comunicación remota después de instalar Windows Management Framework 3.0.

## Consulte también
[Instalar Windows PowerShell](../setup/Installing-Windows-PowerShell.md)
[Iniciar Windows PowerShell [ps]](https://technet.microsoft.com/en-us/library/8ec8c2d7-8e7c-4722-a3d2-498fe5739a8e)



<!--HONumber=May16_HO2-->


