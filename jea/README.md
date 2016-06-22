# Just Enough Administration
Just Enough Administration (JEA) es una tecnología de seguridad que permite la administración delegada de todo lo que se puede administrar con PowerShell.
Con JEA, se puede hacer lo siguiente:
- **Reducir el número de administradores de las máquinas** aprovechando las cuentas virtuales que realizan acciones con privilegios en nombre de usuarios normales.
- **Limitar lo que pueden hacer los usuarios** especificando qué cmdlets, funciones y comandos externos pueden ejecutar.
- **Entender mejor lo que hacen los usuarios** con transcripciones "con consentimiento temporal" que muestran exactamente qué comandos ejecutó un usuario durante una sesión.

¿Por qué es importante?
Considere el escenario común en el que los servidores DNS coexisten con los controladores de dominio de Active Directory.
Los administradores DNS necesitan tener privilegios de administrador local para solucionar problemas con el servidor DNS, pero para ello tiene que hacerlos miembros del grupo de seguridad "Administradores de dominio" con privilegios elevados.
Esto les concede el control sobre todo el dominio y les permite obtener acceso a todos los recursos de la máquina.

Con JEA, puede configurar un rol para los administradores DNS que les conceda acceso a todos los comandos que necesitan para realizar su trabajo, pero a nada más.
Esto significa que pueden reparar fácilmente una caché DNS dañada sin tener derechos para Active Directory, examinar el sistema de archivos o ejecutar scripts potencialmente peligrosos.
Y lo que es mejor, cuando la sesión de JEA está configurada para usar cuentas virtuales con privilegios de un solo uso, los administradores de DNS pueden conectarse al servidor mediante credenciales *sin privilegios* y seguir siendo capaces de ejecutar comandos con privilegios.

## Introducción

### Instalación
JEA se está desarrollando junto con la próxima versión de Windows Server 2016 y está disponible en versiones anteriores de Windows a través de actualizaciones de Windows Management Framework.
La versión actual de JEA está disponible en las siguientes plataformas:

**Windows Server**
- Windows Server 2016 Technical Preview 4 y versiones posteriores
- Windows Server 2012 R2, 2012 y 2008 R2\* con [Windows Management Framework 5.0](https://www.microsoft.com/en-us/download/details.aspx?id=50395) instalado

**Cliente de Windows**
- Windows 10 con la actualización de noviembre (1511) instalada
- Windows 8.1, 8 y 7\* con [Windows Management Framework 5.0](https://www.microsoft.com/en-us/download/details.aspx?id=50395) instalado

\* La compatibilidad con cuentas virtuales en sesiones de JEA no está disponible en Windows Server 2008 R2 o Windows 7.


### Conceptos principales
**¿Qué es exactamente JEA?**

JEA es una extensión de [puntos de conexión restringidos](http://blogs.technet.com/b/heyscriptingguy/archive/2014/03/31/introduction-to-powershell-endpoints.aspx) de PowerShell que agrega definiciones de rol, cuentas virtuales y otras mejoras que ayudan a bloquear los puntos de conexión de administración.
Un punto de conexión de JEA consta de un archivo de configuración de sesión de PowerShell y uno o más archivos de funcionalidad de rol.

**¿Qué son los archivos de configuración de sesión y de funcionalidad de rol?**

Los archivos de configuración de sesión de PowerShell (.pssc) definen *quién* se puede conectar a un punto de conexión de PowerShell y *cómo* está configurado.
Aquí es donde se asignan los usuarios y los grupos de seguridad a los roles de administración específicos y se configuran valores globales como las cuentas virtuales y las directivas de transcripción.
Los archivos de configuración de sesión son específicos de cada máquina, lo que le permite controlar el acceso por máquina, si le interesa.

Los archivos de funcionalidad de rol de PowerShell (.psrc) definen *qué* pueden hacer en el sistema los usuarios que pertenecen a cada rol.
Aquí puede restringir qué cmdlets, funciones, proveedores y programas externos puede usar un usuario en su sesión de JEA.
Los archivos de funcionalidad de rol suelen ser genéricos del rol al que se está atendiendo (administrador de DNS, departamento de soporte técnico de nivel 1, auditoría de inventario de solo lectura, etc.) y pertenecen a los módulos de PowerShell, lo que facilita compartirlos con el entorno y con otros usuarios.

**¿Cómo aprovecha JEA las cuentas virtuales?**

En el archivo de configuración de sesión de PowerShell, puede configurar las sesiones de JEA para que usen cuentas de ejecución virtuales.
Las cuentas virtuales son cuentas con privilegios de un solo uso creadas para el usuario específico que se conecta en esa sesión específica en el contexto en el que se ejecutan los comandos del usuario.
Las cuentas virtuales pertenecen de forma predeterminada al grupo de seguridad local "Administradores", pero se pueden configurar opcionalmente para que solo pertenezcan a los grupos de seguridad que especifique.

### Explorar la guía de la experiencia
¿Está listo para crear su primer punto de conexión de JEA?
Consulte la [guía de la experiencia de JEA](jea-uide.md) para aprender a crear, implementar y usar su propio punto de conexión de JEA.
La guía incluye una rápida introducción con un punto de conexión de JEA predefinido para que se haga una idea de cómo es la experiencia del usuario final y, después, describe el proceso de crear el punto de conexión desde cero para explicar las configuraciones de sesión y las funcionalidades de rol.

### Empiece a crear sus propios puntos de conexión de JEA
Es fácil crear un punto de conexión de JEA: lo único que necesita es un sistema habilitado para JEA y un editor de texto (como PowerShell ISE).
Se recomienda que, para empezar, cree archivos esqueleto mediante `New-PSRoleCapabilityFile -Path <path>` y `New-PSSessionCapabilityFile -Path <Path>` sin ningún otro argumento.
Estos archivos esqueleto contienen todos los campos de configuración aplicables, junto con comentarios útiles que explican para qué puede usarse cada campo.

Para facilitar la creación de puntos de conexión de JEA, consulte el [JEA Toolkit Helper](http://blogs.technet.com/b/privatecloud/archive/2015/12/20/introducing-the-updated-jea-helper-tool.aspx) (Asistente para el kit de herramientas de JEA), que proporciona una GUI con la que puede crear archivos de configuración de sesión y funcionalidad de rol.
Incluso permite generar funcionalidades de rol basadas en los registros de PowerShell para que tome como punto de partida los comandos que los usuarios ejecutan habitualmente para realizar su trabajo.


<!--HONumber=Jun16_HO3-->


