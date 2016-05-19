---
title: Cómo usar perfiles en ISE de Windows PowerShell
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0219626a-6da5-4acc-b630-d058e8b29cc6
---
# Cómo usar perfiles en ISE de Windows PowerShell
En este tema se explica cómo usar perfiles en el Entorno de scripting integrado (ISE) de Windows PowerShell®. Antes de realizar las tareas de esta sección, se recomienda consultar [about_Profiles [v4]](https://technet.microsoft.com/en-us/library/e1d9e30a-70cc-4f36-949f-fc7cd96b4054). O bien, en el panel de consola, escriba "get-help about_profiles" y presione **ENTRAR**..

Un perfil es un script de Windows PowerShell ISE que se ejecuta automáticamente cuando se inicia una nueva sesión.  Puede crear uno o varios perfiles de Windows PowerShell para Windows PowerShell ISE y usarlos para configurar el entorno de Windows PowerShell o Windows PowerShell ISE y prepararlo para su uso, con las variables, los alias, las funciones, y las preferencias de color y fuente que desee que estén disponibles. Un perfil afecta a todas las sesiones de Windows PowerShell ISE que inicia.

> [!NOTE]
> La directiva de ejecución de Windows PowerShell determina si puede ejecutar scripts y cargar un perfil. La directiva de ejecución predeterminada, "Restricted", impide que se ejecuten todos los scripts, incluidos los perfiles. Si usa la directiva "Restricted", no se puede cargar el perfil. Para más información sobre la directiva de ejecución, consulte [about_Execution_Policies [v4]](https://technet.microsoft.com/en-us/library/347708dc-1515-4d74-978b-8334603472e6)..

## Seleccionar un perfil para usarlo en Windows PowerShell ISE
Windows PowerShell ISE admite perfiles para el usuario actual y para todos los usuarios. También admite los perfiles de Windows PowerShell que se aplican a todos los hosts.

El perfil que use viene determinado por la forma en que se usa Windows PowerShell y Windows PowerShell ISE.

-   Si solo usa Windows PowerShell ISE para ejecutar Windows PowerShell, guarde todos los elementos en uno de los perfiles específicos de ISE, como el perfil CurrentUserCurrentHost de Windows PowerShell ISE o el perfil AllUsersCurrentHost de Windows PowerShell ISE.

-   Si usa varios programas host para ejecutar Windows PowerShell, guarde sus funciones, alias, variables y comandos en un perfil que afecte a todos los programas host, como el perfil CurrentUserAllHosts o AllUsersAllHosts y guarde las características específicas de ISE, como la personalización de color y fuente en el perfil CurrentUserCurrentHost de Windows PowerShell ISE o el perfil AllUsersCurrentHost de Windows PowerShell ISE.

Los siguientes son perfiles que se pueden crear y usar en Windows PowerShell ISE. Cada perfil se guarda en su propia ruta de acceso específica.

|Tipo de perfil|Ruta de acceso al perfil|
|----------------|----------------|
|"Usuario actual, PowerShell ISE"|$profile.CurrentUserCurrentHost o $profile|
|"Todos los usuarios, PowerShell ISE"|$profile.AllUsersCurrentHost|
|"Usuario actual, todos los hosts"|$profile.CurrentUserAllHosts|
|"Todos los usuarios, todos los hosts"|$profile.AllUsersAllHosts|

## Para crear un nuevo perfil
Para crear un nuevo perfil "Usuario actual, Windows PowerShell ISE", ejecute este comando:

```
if (!(test-path $profile )) 
{new-item -type file -path $profile -force}
```

Para crear un nuevo perfil "Todos los usuarios, Windows PowerShell ISE", ejecute este comando:

```
if (!(test-path $profile.AllUsersCurrentHost)) 
{new-item -type file -path $profile.AllUsersCurrentHost -force}
```

Para crear un nuevo perfil "Usuario actual, todos los hosts", ejecute este comando:

```
if (!(test-path $profile.CurrentUserAllHosts)) 
{new-item -type file -path $profile.CurrentUserAllHosts -force}
```

Para crear un nuevo perfil "Todos los usuarios, todos los hosts", escriba:

```
if (!(test-path $profile.AllUsersAllHosts)) 
{new-item -type file -path $profile.AllUsersAllHosts-force}
```

## Para editar un perfil

1.  Para abrir el perfil, ejecute el comando psedit con la variable que especifica el perfil que quiere editar. Por ejemplo, para abrir el perfil "Usuario actual, Windows PowerShell ISE", escriba: `psEdit $profile`

2.  Agregue algunos elementos a su perfil. Estos son algunos ejemplos para comenzar:

    -   Para cambiar el color de fondo predeterminado del panel de consola a azul, en el archivo de perfil, escriba: `$psISE.Options.OutputPaneBackground = 'blue'` . Para más información sobre la variable $psISE, consulte [Windows PowerShell ISE Object Model Reference](https://technet.microsoft.com/en-us/library/e1a9e7d1-0fd5-47de-8d9b-f1be1ed13b0c) (Referencia del modelo de objetos de Windows PowerShell ISE)..

    -   Para cambiar el tamaño de la fuente a 20, en el archivo de perfil, escriba: `$psISE.Options.FontSize =20`

3.  Para guardar el archivo de perfil, en el menú **Archivo**, haga clic en **Guardar**. La próxima vez que abra Windows PowerShell ISE, se aplicarán sus opciones personalizadas.

## Consulte también
[about_Profiles [v4]](https://technet.microsoft.com/en-us/library/e1d9e30a-70cc-4f36-949f-fc7cd96b4054)
[Usar Windows PowerShell ISE](Using-the-Windows-PowerShell-ISE.md)



<!--HONumber=May16_HO2-->


