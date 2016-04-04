# Introducción a la configuración de estado deseado de PowerShell #

En esta guía se describe cómo empezar a crear documentos de configuración de estado deseado de PowerShell y aplicarlos a las máquinas. Supone una familiarización básica con los cmdlets, módulos y funciones de PowerShell. 


## Crear una configuración ##

Las [**configuraciones**](https://msdn.microsoft.com/en-us/powershell/dsc/configurations) son documentos que describen un entorno. Los entornos constan de elementos "**nodo**", que normalmente son máquinas físicas o virtuales. 

Las configuraciones pueden presentarse de diversas formas. La manera más fácil de crear una nueva configuración es crear un archivo .ps1 (script de PowerShell). Para hacerlo, abra el editor que prefiera. PowerShell ISE es una buena opción, ya que entiende DSC de forma nativa. Guarde lo siguiente como un archivo PS1:

```powershell
configuration MyFirstConfiguration
{
    Import-DscResource -Name WindowsFeature

    Node localhost
    {
        WindowsFeature IIS
        {
            Name = "IIS"

        }
        
    }

}
```
## Partes de una configuración ##
**Configuration** es una palabra clave que se agregó a PowerShell 4.0. Indica un tipo especial de función de PowerShell que utiliza la configuración de estado deseado. En este ejemplo, la función se denomina myFirstConfiguration. 

La siguiente línea es una instrucción import, similar a la importación de un módulo. Se explicará más adelante.

"Node" define el nombre de la máquina en que actuará esta configuración. Aunque esta configuración se modifica localmente, las configuraciones pueden llegar a los nodos remotos y configurarlos. 

Los nodos pueden ser los nombres de máquinas o direcciones IP. Puede tener varios nodos en un documento de configuración único. Mediante [datos de configuración](https://msdn.microsoft.com/en-us/powershell/dsc/configdata), también puede hacer que se aplique la misma configuración a varios nodos. En este caso, el nodo es "localhost", que significa el equipo local. 

El elemento siguiente es un [**recurso**](https://msdn.microsoft.com/en-us/powershell/dsc/resources). Los recursos son bloques de creación de configuraciones. Cada recurso es un módulo que define la lógica de implementación de un único aspecto de una máquina. Puede ver todos los recursos en su máquina si ejecuta **Get-DscResource** en PowerShell. Los recursos deben estar presentes en la máquina local y haberse importado antes de que se puedan utilizar en una configuración con **Import-DscResource**, que se encuentra en la segunda línea de esta configuración. 

**Establecer una configuración**

Si el script anterior se guarda y se ejecuta, no se producirá ningún resultado. Esto se debe a que una configuración es simplemente una función y el script anterior ha definido la función, pero aún no la ha ejecutado. Después de definir la función, se debe invocar:
```powershell
myFirstConfiguration
```

Cuando se ejecuta, las funciones de configuración validan que la configuración sea válida. No debe tener errores de sintaxis, los recursos deben tener todos los parámetros obligatorios definidos y todos los recursos deben importarse antes de la ejecución.

Cuando se ejecuta la configuración, crea una carpeta con el nombre de la configuración que contiene un **archivo .MOF** para cada uno de los nodos de la configuración. El archivo .MOF es un formato de administración basado en estándares que utiliza la DSC de PowerShell para comunicarse a través de la red.

Para establecer la configuración:
```powershell
Start-DscConfiguration -Path ./myFirstConfiguration
```
Esto crea un trabajo de PowerShell que llega a los nodos de la configuración y los configura. Para ver la salida del trabajo, use -Wait. 
```powershell
Start-DscConfiguration -Path ./myFirstConfiguration -Wait
```



<!--HONumber=Mar16_HO1-->


