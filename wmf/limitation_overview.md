# Problemas y limitaciones conocidos

Los accesos directos de PowerShell se interrumpen cuando se usan por primera vez
------------------------------------------------------------

**Resolución:** realice una de las acciones siguientes.

1.  Haga clic con el botón derecho en el acceso directo de PowerShell. Seleccione "Windows PowerShell" para iniciar en modo sin privilegios elevados.
2.  Haga clic con el botón derecho en el acceso directo de PowerShell. Haga clic en "Windows PowerShell" y seleccione "Ejecutar como administrador" para iniciar en modo con privilegios elevados.

Después de llevar a cabo cualquiera de las acciones anteriores, los accesos directos de PowerShell funcionarán. Estas acciones deben realizarse una sola vez.


Los módulos de PowerShell y los recursos de DSC notifican errores sobre ExecutionPolicy en Windows 7
-------------------------------------------------------------------------------------
En Windows 7, el uso de módulos de PowerShell y recursos de DSC puede provocar la notificación de errores sobre ExecutionPolicy.

**Resolución:** establezca ExecutionPolicy en RemoteSigned. Para ello, ejecute el siguiente comando en una sesión de PowerShell con privilegios elevados (Ejecutar como administrador):

```powershell
Set-ExecutionPolicy RemoteSigned
```

La conexión a un punto de conexión de Exchange remoto antiguo causa un bloqueo
------------------------------------------------------------

El punto de conexión de Exchange le redirige a un nuevo punto de conexión. Hay un error en la lógica de redirección que causa un bloqueo.

**Resolución:** realice la conexión directamente al nuevo punto de conexión.


La característica Registro de inventario de software se detiene erróneamente después de la instalación de WMF 5.0 en Windows Server 2012 R2
-------------------------------------------------------------------------------------------------------------

Al instalar WMF 5.0 en un sistema Windows Server 2012 R2 que ya ejecuta SIL, la característica Registro de inventario de software de detiene por error después de la instalación.

**Resolución:** ejecute el cmdlet Start-SilLogging después de la instalación de WMF, ya que el proceso de instalación detendrá por error la característica Registro de inventario de software.

Get-ChildItem no funciona si -LiteralPath y –Recurse se usan juntos.
--------------------------------------------------------------------------

Si un nombre de directorio contiene un carácter comodín no válido, Get-ChildItem no producirá los resultados esperados si
-LiteralPath y -Recurse se usan juntos.

**Resolución:** no es lo ideal, pero la solución actual es implementar la recursividad en el script en lugar de depender del cmdlet.
<!--HONumber=Mar16_HO2-->
