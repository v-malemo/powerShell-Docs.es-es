# Separación de los identificadores de nodo y de configuración

## Introducción

Con el fin de proporcionar una experiencia más flexible y simplificada al usar DSC en modo de extracción, hemos agregado una serie de características en esta versión. Estas características están diseñadas para que pueda tener la flexibilidad de configurar e implementar con facilidad las configuraciones en varios nodos, y seguir realizando el seguimiento de la información de estado y generación de informes de cada nodo individualmente. Estas características son:

* Un nombre de configuración que identifica la configuración de un equipo. Este nombre pueden compartirlo varios nodos de destino. 
* Un identificador de agente que identifica de forma única un solo nodo.
* Un paso de registro que solo se produce la primera vez que un nodo de destino se conecta a un servidor de extracción.

**Nota:** Estas características y funciones se han agregado, pero no reemplazan los conceptos ni las características de extracción existentes. Puede usar estas nuevas características o las anteriores con el nuevo servidor de extracción que se incluye en esta versión.

## Id. de agente

Esta característica es para usuarios que no quieren configurar ni administrar identificadores únicos para cada nodo de destino. Ahora, ya no existe contabilidad de los GUID necesarios. DSC genera automáticamente un identificador de agente que usa al comunicarse con el servidor de extracción. Este identificador lo usa el servidor de extracción para identificar de forma única toda la información con un nodo dado. También significa que no es necesario configurar cada nodo de destino con una metaconfiguración única que contenga un identificador único para que una sola metaconfiguración puedan usarla muchos nodos de destino, al tiempo que se mantiene la identidad única de cada nodo. 

## Nombre de la configuración

El nombre de configuración es un nombre descriptivo que define ese nombre de la configuración a la que se aplicará un nodo de destino. Los cambios asociados son los siguientes:  

### Nombre descriptivo

Puede ser cualquier cadena. No necesita estar en el formato de un GUID para que una configuración que configura SQL Server se pueda denominar simplemente "SQL_Server". Esto facilita enormemente identificar qué hace una configuración determinada.

### Asignación

La configuración a la que se asigna a un nodo de destino se administra de manera centralizada en el servidor de extracción. Se puede iniciar mediante la definición de la propiedad ConfigurationName en la metaconfiguración, pero solo se utiliza durante el registro. Después del registro, el servidor indica al nodo de destino qué configuración obtendrá. Para el servidor de extracción local, esta asignación entre las configuraciones y los nodos de destino solo puede realizarse durante el registro, pero en Automatización de Azure pueden realizarse fácilmente estos cambios en la interfaz de usuario o a través de sus cmdlets. Para cambiar la configuración asignada a un nodo de destino para el servidor de extracción local puede volver a ejecutar el registro.

### Varias configuraciones o configuraciones parciales

Si se asignan varios nombres de configuración a un nodo de destino, estos se tratarán como configuraciones parciales. Para que funcione, la metaconfiguración en el nodo de destino debe configurarse para aceptar las configuraciones parciales. **Nota:** Esto solo se admite en el servidor de extracción local. Automatización de Azure no admite las configuraciones parciales.

## Registro

Dado que los nombres de configuración ya no son GUID (ahora son nombres descriptivos), cualquier persona puede adivinarlos. Para mitigar el problema de seguridad que esto conlleva, hemos agregado un paso de registro antes de que un nodo pueda iniciar la solicitud de configuraciones desde un servidor. Un nodo se registra con el servidor de extracción con un secreto compartido (que ya conocen el nodo y el servidor) y, opcionalmente, el nombre de la configuración que solicitará. Este secreto compartido no tiene que ser único para cada equipo. Suposición: el secreto compartido es un identificador difícil de adivinar, como un GUID. Este secreto compartido está definido por la propiedad **RegistrationKey** en la metaconfiguración del nodo de destino.

### Ejemplo de metaconfiguración

```powershell
[DscLocalConfigurationManager()]
Configuration SampleMetaConfig
{
    Settings
    {
        RefreshMode = "PULL";
        AllowModuleOverwrite = $true;
        RebootNodeIfNeeded = $true;
        ConfigurationModeFrequencyMins = 60;
    }

    ConfigurationRepositoryWeb ConfigurationManager
    {
        ServerURL = “https://PullServerMachine:8080/psdscpullserver.svc”
        RegistrationKey = "140a952b-b9d6-406b-b416-e0f759c9c0e4"
        ConfigurationNames = @(“WebRole”)
    }
}

SampleMetaConfig
```

El valor de RegistrationKey debe definirse en el servidor de extracción. Para hacerlo al configurar el servidor de extracción, cree un archivo de texto con el nombre **RegistrationKeys.txt** en una ubicación determinada y, a continuación, establezca esta ubicación en el archivo web.config del servidor de extracción, tal como se muestra a continuación.  

```XML
<add key="ConfigurationPath" value="C:\Program Files\WindowsPowerShell\DscService\Configuration">

<add key="ModulePath" value="C:\Program Files\WindowsPowerShell\DscService\Modules">

<add key="RegistrationKeyPath" value="C:\Program Files\WindowsPowerShell\DscService">
```

Use la versión más reciente del recurso de DSC xDSCWebService para implementar completamente un servidor de extracción local para su uso con este. Consulte [Configuración de ejemplo](https://github.com/grayzu/PSSummitEU2015/blob/master/PullServer/02%20-%20PullServer%20Config.ps1) para ver una configuración completa.

**Nota:** Esta característica no se admite cuando el servidor de extracción está configurado para ser un recurso compartido de archivos. Solo se admite para el servidor de extracción basada en Web.<!--HONumber=Mar16_HO2-->
