---
title:   Escribir ayuda para configuraciones de DSC
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Escribir ayuda para configuraciones de DSC

>Se aplica a: Windows PowerShell 5.0

Puede usar la ayuda basada en comentarios en configuraciones de DSC. Los usuarios pueden acceder a la ayuda mediante una llamada a la función de la configuración con `-?`, o mediante el cmdlet [Get-Help](https://technet.microsoft.com/en-us/library/hh849696.aspx). Para obtener más información sobre la ayuda basada en comentarios de PowerShell, vea [about_Comment_Based_Help](https://technet.microsoft.com/en-us/library/hh847834.aspx).

En el ejemplo siguiente se muestra un script que contiene dos configuraciones y ayuda basada en comentarios para cada configuración:

```powershell
<#
.SYNOPSIS
A brief description of the function or script. This keyword can be used only once for each configuration.


.DESCRIPTION
A detailed description of the function or script. This keyword can be used only once for each configuration.


.PARAMETER computername
The description of a parameter. Add a .PARAMETER keyword for each parameter in the function or script syntax.

Type the parameter name on the same line as the .PARAMETER keyword. Type the parameter description on the lines following the .PARAMETER keyword. 
Windows PowerShell interprets all text between the .PARAMETER line and the next keyword or the end of the comment block as part of the parameter description. 
The description can include paragraph breaks.

The Parameter keywords can appear in any order in the comment block, but the function or script syntax determines the order in which the parameters 
(and their descriptions) appear in help topic. To change the order, change the syntax.

.PARAMETER filePath
Provide a PARAMETER section for each parameter that your script or function accepts.

.EXAMPLE
A sample command that uses the function or script, optionally followed by sample output and a description. Repeat this keyword for each example. If you have multiple examples,
there is no need to number them. PowerShell will number the examples in help text.

.EXAMPLE
This example will be labeled "EXAMPLE 2" when help is displayed to the user.
#>

configuration HelpSample1
{
    param([string]$computername,[string]$filePath)
    File f
    {
        Contents="Hello World"
        DestinationPath = "c:\Destination.txt"
    }
}
```

## Ver la ayuda de configuración

Para ver la ayuda de una configuración, use el cmdlet **Get-Help** con el nombre de la función o escriba el nombre de la función seguido de `-?`. A continuación, se muestra el resultado de la función anterior cuando se pasa a **Get-Help**:

```powershell
PS C:\> Get-Help HelpSample1

NAME
    HelpSample1
    
SYNOPSIS
    A brief description of the function or script. This keyword can be used only once for each configuration.
    
    
SYNTAX
    HelpSample1 [[-InstanceName] <String>] [[-DependsOn] <String[]>] [[-OutputPath] <String>] [[-ConfigurationData] <Hashtable>] [[-computername] 
    <String>] [[-filePath] <String>] [<CommonParameters>]
    
    
DESCRIPTION
    A detailed description of the function or script. This keyword can be used only once for each configuration.
    

RELATED LINKS

REMARKS
    To see the examples, type: "get-help HelpSample1 -examples".
    For more information, type: "get-help HelpSample1 -detailed".
    For technical information, type: "get-help HelpSample1 -full".
```

## Consulte también
* [Configuraciones DSC](configurations.md)



<!--HONumber=May16_HO3-->


