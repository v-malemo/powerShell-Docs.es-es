# Requisitos del sistema

- Instale las actualizaciones de Windows más recientes antes de instalar WMF 5.0 RTM.
- Puede instalar WMF 5.0 RTM solo en los siguientes sistemas operativos:

    | Sistema operativo       | Ediciones         | Requisitos previos        |  Vínculos de paquete |
    |------------------------|--------------|------------------|----------------------| --------------|
    | Windows Server 2012 R2 |  |  | <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkId=717507)">Win8.1AndW2K12R2-KB3134758-x64.msu</ctype="x-NOTFOUND"> |
    | Windows Server 2012    |  |  | <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkId=717506)">W2K12-KB3134759-x64.msu</ctype="x-NOTFOUND"> |
    | Windows Server 2008 R2 SP1 | Todos, excepto IA64 | <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://www.microsoft.com/en-us/download/details.aspx?id=40855)">WMF 4.0</ctype="x-NOTFOUND"> y <ctype="x-NOTFOUND" mdpre="[" mdpost="](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx)">.NET Framework 4.5 o posterior</ctype="x-NOTFOUND"> están instalados | <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkId=717504)">Win7AndW2K8R2-KB3134760-x64.msu</ctype="x-NOTFOUND">|
    | Windows 8.1 | Pro, Enterprise | | <ctype="x-NOTFOUND" mdpre="**" mdpost="**">x64:</ctype="x-NOTFOUND"> <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkId=717507)">Win8.1AndW2K12R2-KB3134758-x64.msu</ctype="x-NOTFOUND"> </br> <ctype="x-NOTFOUND" mdpre="**" mdpost="**">x86:</ctype="x-NOTFOUND">  <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkID=717963)">Win8.1-KB3134758-x86.msu</ctype="x-NOTFOUND">|
    | Windows 7 SP1 | Todos | <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://www.microsoft.com/en-us/download/details.aspx?id=40855)">WMF 4.0</ctype="x-NOTFOUND"> y <ctype="x-NOTFOUND" mdpre="[" mdpost="](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx)">.NET Framework 4.5 o superior</ctype="x-NOTFOUND"> están instalados | <ctype="x-NOTFOUND" mdpre="**" mdpost="**">x64:</ctype="x-NOTFOUND">  <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkId=717504)">Win7AndW2K8R2-KB3134760-x64.msu</ctype="x-NOTFOUND">  </br> <ctype="x-NOTFOUND" mdpre="**" mdpost="**">x86:</ctype="x-NOTFOUND">  <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://go.microsoft.com/fwlink/?LinkID=717962)">Win7-KB3134760-x86.msu</ctype="x-NOTFOUND">|

# Instrucciones de instalación

### Para instalar WMF 5.0 desde el Explorador de Windows (o el Explorador de archivos):

1. Desplácese a la carpeta en la que descargó el archivo MSU.

2. Haga doble clic en el archivo MSU para ejecutarlo.

### Para instalar WMF 5.0 desde el símbolo del sistema:

1. Después de descargar el paquete correcto para la arquitectura del equipo, abra una ventana del símbolo del sistema con derechos de usuario elevados (Ejecutar como administrador). En las opciones de instalación de Server Core de Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 SP1, el símbolo del sistema se abre de forma predeterminada con derechos de usuario elevados.

2. Cambie los directorios a la carpeta en la que descargó o copió el paquete de instalación de WMF 5.0.

3. Ejecute uno de los siguientes comandos:
    - En los equipos que ejecuten Windows Server 2012 R2 o Windows 8.1 x64, ejecute <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Win8.1AndW2K12R2-KB3134758-x64.msu /quiet</ctype="x-NOTFOUND">.
    - En los equipos que ejecuten Windows Server 2012, ejecute <ctype="x-NOTFOUND" mdpre="**" mdpost="**">W2K12-KB3134759-x64.msu /quiet</ctype="x-NOTFOUND">.
    - En los equipos que ejecuten Windows Server 2008 R2 SP1 o Windows 7 SP1 x64, ejecute <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Win7AndW2K8R2-KB3134760-x64.msu /quiet</ctype="x-NOTFOUND">.
    - En los equipos que ejecuten Windows 8.1 x86, ejecute <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Win8.1-KB3134758-x86.msu /quiet</ctype="x-NOTFOUND">.
    - En los equipos que ejecuten Windows 7 SP1 x86, ejecute <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Win7-KB3134760-x86.msu /quiet</ctype="x-NOTFOUND">.

### Notas de instalación adicionales para Windows Server 2008 R2 SP1 y Windows 7 SP1:

Asegúrese de que se cumplen los requisitos previos siguientes:
- El Service Pack más reciente está instalado.
- <ctype="x-NOTFOUND" mdpre="[" mdpost="](http://www.microsoft.com/en-us/download/details.aspx?id=40855)">WMF 4.0</ctype="x-NOTFOUND"> está instalado.
- <ctype="x-NOTFOUND" mdpre="[" mdpost="](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx)">.NET Framework 4.5 o posterior</ctype="x-NOTFOUND"> está instalado.

**Dependencia de WMF 4.0**

Los sistemas de Windows Server 2008 R2 SP1 y Windows 7 SP1 tienen PowerShell 2.0, WinRM y WMI integrados. Los paquetes de WMF 3.0 y WMF 4.0, que actualizan estos componentes integrados, se publicaron después del lanzamiento de Windows Server 2008 R2 SP1 y Windows 7 SP1. Los paquetes de instalación y desinstalación de WMF 3.0 y WMF 4.0 detectaron algunos problemas en la ruta de actualización siguiente:

- Integrado --> WMF 4.0
- Integrado --> WMF 3.0 --> WMF4.0. 

Solucionamos todos estos problemas en los paquetes de WMF 4.0. Por lo tanto, hay un requisito previo de WMF 4.0 para la instalación de WMF 5.0 en Windows Server 2008 R2 SP1 y Windows 7 SP1. A continuación, se muestran los problemas específicos que pueden producirse si no instala WMF 4.0 antes de actualizar a WMF 5.0:

- El registro de eventos reenviados no está disponible y el registro de EventCollector no se muestra en el Visor de eventos después de desinstalar WMF 3.0 o WMF 5.0 (sin el requisito previo de WMF 4.0 instalado) en Windows 7 SP1 y en Windows Server 2008 R2 SP1 (<ctype="x-NOTFOUND" mdpre="[" mdpost="](https://support.microsoft.com/en-us/kb/2809215)">KB2809215</ctype="x-NOTFOUND">).
- La personalización de la variable de entorno <ctype="x-NOTFOUND" mdpre="*" mdpost="*">PSModulePath</ctype="x-NOTFOUND"> se restablece al valor predeterminado cuando se actualiza directamente desde PowerShell 2.0 integrado a WMF 5.0 (<ctype="x-NOTFOUND" mdpre="[" mdpost="](https://support.microsoft.com/en-us/kb/2872035)">KB2872035</ctype="x-NOTFOUND">) o desde WMF 3.0 a WMF 5.0. (<ctype="x-NOTFOUND" mdpre="[" mdpost="](https://support.microsoft.com/en-us/kb/2872047)">KB2872047</ctype="x-NOTFOUND">) en Windows 7 SP1 y en Windows Server 2008 R2 SP1.

**Dependencia de WinRM**

La configuración de estado deseado (DSC) de Windows PowerShell depende de WinRM. WinRM no está habilitado de forma predeterminada en Windows Server 2008 R2 SP1 y Windows 7 SP1. Para habilitar WinRM, en una sesión de Windows PowerShell con permisos elevados, ejecute <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Set-WSManQuickConfig</ctype="x-NOTFOUND">.

# Instrucciones de desinstalación

### Uso de la ventana del símbolo del sistema

1.  Abra el <ctype="x-NOTFOUND" mdpre="**" mdpost="**">símbolo del sistema.</ctype="x-NOTFOUND">

2.  Ejecute <ctype="x-NOTFOUND" mdpre="[" mdpost="](https://support.microsoft.com/en-us/kb/934307)">Windows Update Standalone Launcher</ctype="x-NOTFOUND"> tal como se muestra a continuación:

En Windows Server 2012 R2 y Windows 8.1:
```powershell
wusa /uninstall /kb:3134758
```
En Windows Server 2012:
```powershell
wusa /uninstall /kb:3134759
```
En Windows Server 2008 R2 SP1 y Windows 7 SP1:
```powershell
wusa /uninstall /kb:3134760
```

### Uso del Panel de control

1.  Abra el <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Panel de control</ctype="x-NOTFOUND">.

2.  Abra <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Programas</ctype="x-NOTFOUND"> y, después, <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Desinstalar un programa.</ctype="x-NOTFOUND">

3.  Haga clic en <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Ver actualizaciones instaladas</ctype="x-NOTFOUND">.

4.  Seleccione <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Windows Management Framework 5.0</ctype="x-NOTFOUND"> en la lista de actualizaciones instaladas. Corresponde a <ctype="x-NOTFOUND" mdpre="*" mdpost="*">KB3134758</ctype="x-NOTFOUND">, <ctype="x-NOTFOUND" mdpre="*" mdpost="*">KB3134759</ctype="x-NOTFOUND"> o <ctype="x-NOTFOUND" mdpre="*" mdpost="*">KB3134760</ctype="x-NOTFOUND">. Haga clic en <ctype="x-NOTFOUND" mdpre="**" mdpost="**">Desinstalar.</ctype="x-NOTFOUND">


<!--HONumber=Mar16_HO4-->


