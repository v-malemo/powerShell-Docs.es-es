# Requisitos del sistema

- Instale las actualizaciones de Windows más recientes antes de instalar WMF 5.0 RTM.
- Puede instalar WMF 5.0 RTM solo en los siguientes sistemas operativos:

    | Sistema operativo       | Ediciones         | Requisitos previos        |  Vínculos de paquete |
    |------------------------|--------------|------------------|----------------------| --------------|
    | Windows Server 2012 R2 |  |  | [Win8.1AndW2K12R2-KB3134758-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717507) |
    | Windows Server 2012    |  |  | [W2K12-KB3134759-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717506) |
    | Windows Server 2008 R2 SP1 | Todas, excepto IA64 | [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855) y [.NET Framework 4.5 o superior](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx) están instalados.| [Win7AndW2K8R2-KB3134760-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717504)|
    | Windows 8.1 | Pro, Enterprise | | **x64:**  [Win8.1AndW2K12R2-KB3134758-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717507) </br> **x86:**  [Win8.1-KB3134758-x86.msu](http://go.microsoft.com/fwlink/?LinkID=717963)|
    | Windows 7 SP1 | Todos | [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855) y [.NET Framework 4.5 o superior](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx) están instalados. | **x64:** [Win7AndW2K8R2-KB3134760-x64.msu](http://go.microsoft.com/fwlink/?LinkId=717504)  </br> **x86:**  [Win7-KB3134760-x86.msu](http://go.microsoft.com/fwlink/?LinkID=717962)|

# Instrucciones de instalación

### Para instalar WMF 5.0 desde el Explorador de Windows (o el Explorador de archivos):

1. Desplácese a la carpeta en la que descargó el archivo MSU.

2. Haga doble clic en el archivo MSU para ejecutarlo.

### Para instalar WMF 5.0 desde el símbolo del sistema:

1. Después de descargar el paquete correcto para la arquitectura del equipo, abra una ventana del símbolo del sistema con derechos de usuario elevados (Ejecutar como administrador). En las opciones de instalación de Server Core de Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 SP1, el símbolo del sistema se abre de forma predeterminada con derechos de usuario elevados.

2. Cambie los directorios a la carpeta en la que descargó o copió el paquete de instalación de WMF 5.0.

3. Ejecute uno de los siguientes comandos:
    - En los equipos que ejecuten Windows Server 2012 R2 o Windows 8.1 x64, ejecute **Win8.1AndW2K12R2-KB3134758-x64.msu /quiet**.
    - En los equipos que ejecuten Windows Server 2012, ejecute **W2K12-KB3134759-x64.msu /quiet**.
    - En los equipos que ejecuten Windows Server 2008 R2 SP1 o Windows 7 SP1 x64, ejecute **Win7AndW2K8R2-KB3134760-x64.msu /quiet**.
    - En los equipos que ejecutan Windows 8.1 x86, ejecute **Win8.1-KB3134758-x86.msu /quiet**.
    - En los equipos que ejecutan Windows 7 SP1 x86, ejecute **Win7-KB3134760-x86.msu /quiet**.

### Notas de instalación adicionales para Windows Server 2008 R2 SP1 y Windows 7 SP1:

Asegúrese de que se cumplen los requisitos previos siguientes:
- El Service Pack más reciente está instalado.
- [WMF 4.0](http://www.microsoft.com/en-us/download/details.aspx?id=40855) está instalado.
- [.NET Framework 4.5 o superior](https://msdn.microsoft.com/en-us/library/5a4x27ek.aspx) está instalado.

**Dependencia de WMF 4.0**

Los sistemas de Windows Server 2008 R2 SP1 y Windows 7 SP1 tienen PowerShell 2.0, WinRM y WMI integrados. Los paquetes de WMF 3.0 y WMF 4.0, que actualizan estos componentes integrados, se publicaron después del lanzamiento de Windows Server 2008 R2 SP1 y Windows 7 SP1. Los paquetes de instalación y desinstalación de WMF 3.0 y WMF 4.0 detectaron algunos problemas en la ruta de actualización siguiente:

- Integrado --> WMF 4.0
- Integrado --> WMF 3.0 --> WMF4.0. 

Solucionamos todos estos problemas en los paquetes de WMF 4.0. Por lo tanto, hay un requisito previo de WMF 4.0 para la instalación de WMF 5.0 en Windows Server 2008 R2 SP1 y Windows 7 SP1. A continuación, se muestran los problemas específicos que pueden producirse si no instala WMF 4.0 antes de actualizar a WMF 5.0:

- El registro de eventos reenviados no está disponible y el registro de EventCollector no se muestra en el Visor de eventos después de desinstalar WMF 3.0 o WMF 5.0 (sin el requisito previo de WMF 4.0 instalado) en Windows 7 SP1 y en Windows Server 2008 R2 SP1 ([KB2809215](https://support.microsoft.com/en-us/kb/2809215)).
- La personalización de la variable de entorno *PSModulePath* se restablece al valor predeterminado cuando se actualiza directamente desde PowerShell 2.0 integrado a WMF 5.0 ([KB2872035](https://support.microsoft.com/en-us/kb/2872035)) o desde WMF 3.0 a WMF 5.0. ([KB2872047](https://support.microsoft.com/en-us/kb/2872047)) en Windows 7 SP1 y en Windows Server 2008 R2 SP1.

**Dependencia de WinRM**

La configuración de estado deseado (DSC) de Windows PowerShell depende de WinRM. WinRM no está habilitado de forma predeterminada en Windows Server 2008 R2 SP1 y Windows 7 SP1. Para habilitar WinRM, en una sesión de Windows PowerShell con permisos elevados, ejecute **Set-WSManQuickConfig**.

# Instrucciones de desinstalación

### Uso de la ventana del símbolo del sistema

1.  Abra el **símbolo del sistema.**

2.  Ejecute [Windows Update Standalone Launcher](https://support.microsoft.com/en-us/kb/934307) tal como se muestra a continuación:

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

1.  Abra el **Panel de control.**

2.  Abra **Programas** y, después, **Desinstalar un programa.**

3.  Haga clic en **Ver actualizaciones instaladas.**

4.  Seleccione **Windows Management Framework 5.0** en la lista de actualizaciones instaladas. Corresponde a *KB3134758*, *KB3134759* o *KB3134760*. Haga clic en **Desinstalar.**


<!--HONumber=Jun16_HO4-->


