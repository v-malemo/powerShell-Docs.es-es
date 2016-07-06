# Registro de inventario de software (SIL)

**IMPORTANTE:** * Al instalar WMF 5.0 en un servidor de Windows Server 2012 R2 que ya ejecuta SIL, es necesario ejecutar el cmdlet Start-SilLogging una vez después de la instalación de WMF, ya que el proceso de instalación detendrá por error la característica Registro de inventario de software.*

La característica Registro de inventario de software ayuda a reducir los costos operativos derivados de la obtención de información precisa sobre el software de Microsoft instalado de forma local en un servidor y, en especial, en numerosos servidores en un entorno de TI (suponiendo que esté instalado y se ejecute en dicho entorno). Siempre que haya uno configurado, puede reenviar estos datos a un servidor de agregación y recopilar los datos de registro en un mismo lugar mediante un proceso automático y uniforme.

Aunque también se pueden registrar datos de inventario de software con una consulta directa a cada equipo, la característica Registro de inventario de software puede, mediante una arquitectura de reenvío (a través de la red) que se inicia en cada servidor, superar los retos de detección de servidores típicos en muchos escenarios relativos al inventario y a la administración de activos de software. Registro de inventario de software usa SSL para proteger los datos que se reenvían a través de HTTPS a un servidor de agregación. Almacenar los datos en un mismo lugar facilita su análisis y manipulación, así como compartirlos cuando sea necesario.

Estos datos no se envían a Microsoft como parte de la funcionalidad de la característica. Solo el propietario con licencia del software del servidor y los administradores pueden hacer uso de los datos y de la funcionalidad del registro de inventario de software.

Para más información y documentación acerca de los cmdlets de Registro de inventario de software, consulte los recursos en línea de Windows Server 2012 R2 en <http://technet.microsoft.com/library/dn383584.aspx>.


<!--HONumber=Jun16_HO4-->


