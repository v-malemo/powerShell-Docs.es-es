# Problemas y limitaciones conocidos de la configuración de estado deseado (DSC)

Cambio de última hora: los certificados usados para cifrar o descifrar contraseñas en configuraciones de DSC no funcionen después de instalar WMF 5.0 RTM
--------------------------------------------------------------------------------------------------------------------------------

En las versiones WMF 4.0 y WMF 5.0 Preview, DSC no permitía que las contraseñas de la configuración tuvieran más de 121 caracteres de longitud. DSC obligaba a usar contraseñas cortas, aunque se deseasen contraseñas largas y seguras. Este cambio de última hora permite que las contraseñas tengan una longitud arbitraria en la configuración de DSC.

**Resolución:** vuelva a crear el certificado mediante el cifrado de datos o el cifrado de clave, así como con el uso mejorado de claves de cifrado de documentos (1.3.6.1.4.1.311.80.1). En el artículo de TechNet <https://technet.microsoft.com/es-es/library/dn807171.aspx> puede encontrar más información.


Los cmdlets de DSC puede producir un error después de instalar WMF 5.0 RTM
------------------------------------------------------------------------------------
Start-DscConfiguration y otros cmdlets de DSC pueden generar el siguiente error después de instalar WMF 5.0 RTM:
```powershell
    LCM failed to retrieve the property PendingJobStep from the object of class dscInternalCache .
    + CategoryInfo : ObjectNotFound: (root/Microsoft/...gurationManager:String) [], CimException
    + FullyQualifiedErrorId : MI RESULT 6
    + PSComputerName : localhost
```

**Resolución:** elimine DSCEngineCache.mof. Para ello, ejecute el siguiente comando en una sesión de PowerShell con privilegios elevados (Ejecutar como administrador).
    
```powershell
Remove-Item -Path $env:SystemRoot\system32\Configuration\DSCEngineCache.mof
```


Es posible que los Cmdlets de DSC no funcionen si WMF 5.0 RTM se instala encima de WMF 5.0 Production Preview
------------------------------------------------------
**Resolución:** elimine DSCEngineCache.mof. Para ello, ejecute el siguiente comando en una sesión de PowerShell con privilegios elevados (Ejecutar como administrador).
```powershell
    mofcomp $env:windir\system32\wbem\DscCoreConfProv.mof
```


LCM puede entrar en un estado inestable al usar Get-DscConfiguration en DebugMode
-------------------------------------------------------------------------------

Si LCM está en DebugMode, al presionar CTRL+C para detener el procesamiento de Get-DscConfiguration puede causar que LCM entre en un estado inestable que impida funcionar a la mayoría de los cmdlets de DSC.

**Resolución:** no presione CTRL+C mientras se depura el cmdlet Get-DscConfiguration.


Stop-DscConfiguration puede colgarse en DebugMode
------------------------------------------------------------------------------------------------------------------------
Si LCM está en DebugMode, Stop-DscConfiguration puede colgarse cuando intenta detener una operación iniciada por Get-DscConfiguration

**Resolución:** finalice la depuración de la operación iniciada por Get-DscConfiguration, como se describe en la sección '[Depuración de scripts de recursos de DSC](#dsc-resource-script-debugging)'.


No se muestran mensajes de error detallados en DebugMode
-----------------------------------------------------------------------------------
Si LCM está en DebugMode, no se muestra ningún mensaje de error detallado de recursos de DSC.

**Resolución:** deshabilite *DebugMode* para ver mensajes detallados del recurso


Las operaciones Invoke-DscResource no se pueden recuperar mediante el cmdlet Get-DscConfigurationStatus
--------------------------------------------------------------------------------------
Después de usar el cmdlet Invoke-DscResource para invocar directamente los métodos de cualquier recurso, los registros de esta operación no se pueden recuperar a través de Get-DscConfigurationStatus posteriormente.

**Resolución:** ninguna.


Get-DscConfigurationStatus devuelve operaciones de ciclo de extracción como de tipo *Consistency*
---------------------------------------------------------------------------------
Cuando un nodo se establece en modo de actualización de extracción, para cada operación de extracción realizada, el cmdlet Get-DscConfigurationStatus indica el tipo de operación como *Consistency* en lugar de *Initial*

**Resolución:** ninguna.

El cmdlet Invoke-DscResource no devuelve los mensajes en el orden en que se producen
---------------------------------------------------------------------------------
El cmdlet Invoke-DscResource no devuelve los mensajes detallados, de advertencia ni de error en el orden en que los producen LCM o el recurso de DSC.

**Resolución:** ninguna.


Los recursos de DSC no se pueden depurar fácilmente cuando se usan con Invoke-DscResource
-----------------------------------------------------------------------
Cuando se ejecuta LCM en modo de depuración (consulte [Depuración de scripts de recursos de DSC](#dsc-resource-script-debugging) para obtener más detalles), el cmdlet Invoke-DscResource no ofrece información sobre el espacio de ejecución al que debe conectarse para la depuración.
**Resolución:** detecte el espacio de ejecución y conéctese a este mediante los cmdlets **Get-PSHostProcessInfo**, **Enter-PSHostProcess** , **Get-Runspace** y **Debug-Runspace** para depurar el recurso de DSC.

```powershell
# Find all the processes hosting PowerShell
Get-PSHostProcessInfo

ProcessName ProcessId AppDomainName
----------- --------- -------------
powershell 3932 DefaultAppDomain
powershell_ise 2304 DefaultAppDomain
WmiPrvSE 3396 DscPsPluginWkr_AppDomain

# Enter the process that is hosting DSC engine (WMI process with DscPsPluginWkr_Appdomain)
Enter-PSHostProcess -Id 3396 -AppDomainName DscPsPluginWkr_AppDomain

# Find all the available rusnspaces in that process
Get-Runspace

Id Name ComputerName Type State Availability
-- ---- ------------ ---- ----- ------------
2 Runspace2 localhost Local Opened InBreakpoint
5 RemoteHost localhost Local Opened Busy

# Debug the runspace that is in **InBreakpoint** availability state
Debug-Runspace -Id 2
```


Varios documentos de configuración parcial para el mismo nodo no pueden tener nombres de recurso idénticos
------------------------------------------------------------------------------------------

Para varias configuraciones parciales implementadas en un mismo nodo, los nombres idénticos de recursos causan un error en tiempo de ejecución.

**Resolución:** use nombres diferentes para la mismos recursos en distintas configuraciones parciales.


Start-DscConfiguration –UseExisting no funciona con -Credential
------------------------------------------------------------------

Si se usa Start-DscConfiguration con el parámetro –UseExisting, el parámetro –Credential se ignora. DSC usa la identidad de proceso predeterminada para continuar la operación. Esto provoca un error cuando se necesita una credencial distinta para continuar en el nodo remoto.

**Resolución:** use una sesión CIM para las operaciones remotas de DSC:
```powershell
$session = New-CimSession -ComputerName $node -Credential $credential
Start-DscConfiguration -UseExisting -CimSession $session
```


Direcciones IPv6 como nombres de nodo en configuraciones de DSC
--------------------------------------------------
No se admiten direcciones IPv6 como nombres de nodo en los scripts de configuración de DSC en esta versión.

**Resolución:** ninguna.


Depuración de los recursos de DSC basados en clases
--------------------------------------
En esta versión no se admite la depuración de los recursos de DSC basado en clases.

**Resolución:** ninguna.


Las variables y las funciones que se definen en el ámbito de $script en el recurso basado en clases de DSC no se conservan en varias llamadas a un recurso de DSC 
-------------------------------------------------------------------------------------------------------------------------------------

Varias llamadas consecutivas a Start-DSCConfiguration producirán un error si la configuración usa cualquier recurso basado en clases que tenga variables o funciones definidas en el ámbito de $script.

**Resolución:** defina todas las variables y funciones de la propia clase de recurso de DSC. No usar funciones o variables de ámbito $script.


Depuración de recursos de DSC cuando un recurso usa PSDscRunAsCredential
----------------------------------------------------------------------
La depuración de recursos de DSC cuando un recurso usa la propiedad *PSDscRunAsCredential* en la configuración no se admite en esta versión.

**Resolución:** ninguna.


PsDscRunAsCredential no se admite para los recursos compuestos de DSC
----------------------------------------------------------------

**Resolución:** usar la propiedad Credential si está disponible. ServiceSet y WindowsFeatureSet de ejemplo


*Get-DscResource -Syntax* no refleja PsDscRunAsCredential correctamente
-------------------------------------------------------------------------
Get-DscResource -Syntax no refleja correctamente la propiedad PsDscRunAsCredential cuando el recurso la marca como obligatoria o no la admite.

**Resolución:** ninguna. Sin embargo, la configuración de creación en ISE refleja los metadatos correctos acerca de la propiedad PsDscRunAsCredential al usar IntelliSense.


WindowsOptionalFeature no está disponible en Windows 7
-----------------------------------------------------

El recurso de DSC WindowsOptionalFeature no está disponible en Windows 7. Este recurso requiere el módulo DISM, así como los cmdlets DISM que están disponibles a partir de Windows 8 y versiones más recientes del sistema operativo Windows.

En el caso de los recursos de DSC basados en clases, puede que Import-DscResource -ModuleVersion no funcione según lo previsto   
------------------------------------------------------------------------------------------
Si el nodo de compilación tiene varias versiones de un módulo de recursos de DSC basados en clases, `Import-DscResource -ModuleVersion` no podrá seleccionar la versión especificada y genera el siguiente error de compilación.

```
ImportClassResourcesFromModule : Exception calling "ImportClassResourcesFromModule" with "3" argument(s): "Keyword 'MyTestResource' already defined in the configuration."
At C:\Windows\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateConfiguration\PSDesiredStateConfiguration.psm1:2035 char:35
+ ... rcesFound = ImportClassResourcesFromModule -Module $mod -Resources $r ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [ImportClassResourcesFromModule], MethodInvocationException
    + FullyQualifiedErrorId : PSInvalidOperationException,ImportClassResourcesFromModule
```

**Resolución:** importe la versión necesaria mediante la definición del objeto *ModuleSpecification* en `-ModuleName` con la clave `RequiredVersion` especificada como sigue:
``` PowerShell  
Import-DscResource -ModuleName @{ModuleName='MyModuleName';RequiredVersion='1.2'}  
```  



<!--HONumber=May16_HO1-->


