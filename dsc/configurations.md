---
title:   Configuraciones DSC
ms.date:  2016-05-16
keywords:  powershell,DSC
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
---

# Configuraciones DSC

>Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

Las configuraciones DSC son scripts de PowerShell que definen un tipo especial de función. 
Para definir una configuración, se utiliza la palabra clave de PowerShell __Configuration__.

```powershell
Configuration MyDscConfiguration {

    Node "TEST-PC1" {
        WindowsFeature MyFeatureInstance {
            Ensure = "Present"
            Name =  "RSAT"
        }
        WindowsFeature My2ndFeatureInstance {
            Ensure = "Present"
            Name = "Bitlocker"
        }
    }
}
```

Guarde el script como un archivo .ps1.

## Sintaxis de la configuración

Un script de configuración consta de las partes siguientes:

- El bloque **Configuration**. Este es el bloque del script más externo. Para definirlo, se usa la palabra clave **Configuration** y se facilita un nombre. En este caso, el nombre de la configuración es "MyDscConfiguration".
- Uno o varios bloques **Node**. Estos definen los nodos (equipos o máquinas virtuales) que se están configurando. En la configuración anterior, hay un bloque **Node** cuyo destino es un equipo denominado "TEST-PC1".
- Uno o varios bloques de recursos. Es aquí donde la configuración establece las propiedades de los recursos que se están configurando. En este caso, hay dos bloques de recursos, cada uno de los cuales llama al recurso "WindowsFeature".

Dentro de un bloque **Configuration**, puede hacer todo lo que se puede realizar normalmente en una función de PowerShell. Por ejemplo, en el ejemplo anterior, si no quisiera codificar de forma rígida el nombre del equipo de destino en la configuración, podría agregar un parámetro para el nombre del nodo:

```powershell
Configuration MyDscConfiguration {

    param(
        [string[]]$ComputerName="localhost"
    )
    Node $ComputerName {
        WindowsFeature MyFeatureInstance {
            Ensure = "Present"
            Name =  "RSAT"
        }
        WindowsFeature My2ndFeatureInstance {
            Ensure = "Present"
            Name = "Bitlocker"
        }
    }
}
```

En este ejemplo, especifique el nombre del nodo, pasando como parámetro $ComputerName cuando [compile la configuración](# Compiling the configuration). El nombre se establece como el valor predeterminado "localhost".

## Compilación de la configuración
Antes de que se pueda establecer una configuración, debe compilarla en un documento MOF. Para ello, llame a la configuración, como lo haría con una función de PowerShell.
>__Nota:__ Para llamar a una configuración, la función debe estar en el ámbito global (como ocurre con cualquier otra función de PowerShell). Puede conseguirlo si precede el script con el operador punto ".", o si ejecuta el script de configuración presionando F5 o haciendo clic en el botón __Ejecutar Script__ del ISE. Para anteponer el operador punto al script, ejecute el comando `. .\myConfig.ps1`, donde `myConfig.ps1` es el nombre del archivo de script que contiene la configuración.

Cuando llama a la configuración, se crea:

- Una carpeta en el directorio actual con el mismo nombre que la configuración.
- Un archivo denominado _NodeName_.mof en el nuevo directorio, donde _NodeName_ es el nombre del nodo de destino de la configuración. Si hay más de un nodo, se creará un archivo MOF por cada nodo.

>__Nota__: El archivo MOF contiene toda la información de configuración del nodo de destino. Por este motivo, es importante protegerlo adecuadamente. Para obtener más información, consulte [Proteger el archivo MOF](secureMOF.md).

Cuando se compila la primera configuración anterior, el resultado es la estructura de carpetas siguiente:

```powershell
PS C:\users\default\Documents\DSC Configurations> . .\MyDscConfiguration.ps1
PS C:\users\default\Documents\DSC Configurations> MyDscConfiguration
    Directory: C:\users\default\Documents\DSC Configurations\MyDscConfiguration
Mode                LastWriteTime         Length Name                                                                                              
----                -------------         ------ ----                                                                                         
-a----       10/23/2015   4:32 PM           2842 TEST-PC1.mof
```  

Si la configuración toma un parámetro, como en el segundo ejemplo, se debe proporcionar en tiempo de compilación. Este sería su aspecto:

```powershell
PS C:\users\default\Documents\DSC Configurations> . .\MyDscConfiguration.ps1
PS C:\users\default\Documents\DSC Configurations> MyDscConfiguration -ComputerName 'MyTestNode'
    Directory: C:\users\default\Documents\DSC Configurations\MyDscConfiguration
Mode                LastWriteTime         Length Name                                                                                              
----                -------------         ------ ----                                                                                         
-a----       10/23/2015   4:32 PM           2842 MyTestNode.mof
```      

## Uso de DependsOn
Una palabra clave de DSC bastante útil es __DependsOn__. Normalmente (aunque no necesariamente siempre), DSC aplica a los recursos en el orden en que aparecen en la configuración. Sin embargo, __DependsOn__ especifica qué recursos dependen de otros recursos y el LCM se asegura de que se apliquen en el orden correcto, independientemente del orden en que se definan las instancias de los recursos. Por ejemplo, una configuración puede especificar que una instancia del recurso __User__ depende de la existencia de una instancia de __Group__:

```powershell
Configuration DependsOnExample {
    Node Test-PC1 {
        Group GroupExample {
            Ensure = "Present"
            GroupName = "TestGroup"
        }

        User UserExample {
            Ensure = "Present"
            UserName = "TestUser"
            FullName = "TestUser"
            DependsOn = "[Group]GroupExample"
        }
    }
}
```

## Uso de nuevos recursos en la configuración
Si ha ejecutado los ejemplos anteriores, habrá observado que se le mostró una advertencia respecto a que se estaba utilizando un recurso sin importarlo explícitamente.
En la actualidad, DSC incluye 12 recursos como parte del módulo PSDesiredStateConfiguration. Otros recursos de los módulos externos deben colocarse en `$env:PSModulePath` para que el LCM los reconozca. Un cmdlet nuevo, [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx), puede utilizarse para determinar qué recursos están instalados en el sistema y están disponibles para que el LCM los use. 
Cuando estos módulos se hayan colocado en `$env:PSModulePath` y [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) los reconozca correctamente, es necesario que se carguen en la configuración. __Import-DscResource__ es una palabra clave dinámica que solo puede reconocerse dentro de un bloque __Configuration__ (es decir, no es un cmdlet). __Import-DscResource__ admite dos parámetros:
* __ModuleName__ es la manera recomendada de usar __Import-DscResource__. Acepta el nombre del módulo que contiene los recursos que se importarán (así como una matriz de cadenas de nombres de módulo). 
* __Name__ es el nombre del recurso que se importará. Este no es el nombre descriptivo que [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) devuelve como "Name", sino el nombre de clase que se usa al definir el esquema del recurso (que [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) devuelve como __ResourceType__). 

## Consulte también
* [Información general sobre la configuración de estado deseado de Windows PowerShell](overview.md)
* [Recursos de DSC](resources.md)
* [Configuración del administrador de configuración local](metaConfig.md)



<!--HONumber=May16_HO3-->


