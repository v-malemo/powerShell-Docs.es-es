---
title:  WinRMSecurityRedirect
ms.date:  2016-05-11
keywords:  powershell,cmdlet
description:  
ms.topic:  article
author:  eslesar
manager:  dongill
ms.prod:  powershell
redirect_url: https://msdn.microsoft.com/powershell/scripting/setup/winrmsecurity
---

# Consideraciones de seguridad de comunicación remota de PowerShell

La comunicación remota de PowerShell es la forma recomendada para administrar los sistemas de Windows. La comunicación remota de PowerShell está habilitada de forma predeterminada en Windows Server 2012 R2. En este documento se tratan cuestiones de seguridad, recomendaciones y procedimientos recomendados para el uso de la comunicación remota de PowerShell.

## ¿Qué es la comunicación remota de PowerShell?

La comunicación remota de PowerShell usa [Administración remota de Windows (WinRM)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426.aspx), que es la implementación de Microsoft del protocolo [Servicios web para administración (WS-Management)](http://www.dmtf.org/sites/default/files/standards/documents/DSP0226_1.2.0.pdf), que permite a los usuarios ejecutar comandos de PowerShell en equipos remotos. Puede encontrar más información acerca del uso de la comunicación remota de PowerShell en [Ejecutar comandos remotos](https://technet.microsoft.com/en-us/library/dd819505.aspx).

La comunicación remota de PowerShell no es lo mismo que usar el parámetro **ComputerName** de un cmdlet para ejecutarlo en un equipo remoto, que usa la llamada a procedimiento remoto (RPC) como protocolo subyacente.

##  Configuración predeterminada de la comunicación remota de PowerShell

La comunicación remota de PowerShell (y WinRM) escucha los puertos siguientes:

- HTTP: 5985
- HTTPS: 5986

De manera predeterminada, la comunicación remota de PowerShell solo permite conexiones de los miembros del grupo Administradores. Las sesiones se inician en el contexto del usuario, por lo que todos los controles de acceso del sistema operativo que se aplican a usuarios individuales y grupos continuarán aplicándose a estos mientras estén conectados a través de la comunicación remota de PowerShell.

En redes privadas, la regla de Firewall de Windows predeterminada para la comunicación remota de PowerShell acepta todas las conexiones. En redes públicas, la regla predeterminada del Firewall de Windows permite las conexiones de comunicación remota de PowerShell únicamente desde dentro de la misma subred. Tendrá que cambiar explícitamente esa regla para abrir la comunicación remota de PowerShell a todas las conexiones de una red pública.

>**Advertencia:** la regla de firewall para redes públicas está diseñada para proteger el equipo frente a los intentos de conexión externa potencialmente malintencionados. Tenga precaución al quitar esta regla.

## Aislamiento de procesos

La comunicación remota de PowerShell usa [Administración remota de Windows (WinRM)](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384426) para la comunicación entre equipos. 
WinRM se ejecuta como servicio en la cuenta servicio de red y genera procesos aislados que se ejecutan como cuentas de usuario para las instancias de host de PowerShell. Una instancia de PowerShell que se ejecuta como un usuario no tiene acceso a un proceso que ejecuta una instancia de PowerShell como otro usuario.

## Registros de eventos generados por la comunicación remota de PowerShell

FireEye ha proporcionado un buen resumen de los registros de eventos y otras pruebas de seguridad generados por las sesiones de comunicación remota de PowerShell, disponibles en  
[Investigating PowerShell Attacks](https://www.fireeye.com/content/dam/fireeye-www/global/en/solutions/pdfs/wp-lazanciyan-investigating-powershell-attacks.pdf) (Investigación de los ataques de PowerShell).

## Protocolos de transporte y cifrado

Resulta útil tener en cuenta la seguridad de una conexión de comunicación remota de PowerShell desde dos perspectivas: autenticación inicial y comunicación continua. 

Independientemente del protocolo de transporte utilizado (HTTP o HTTPS), la comunicación remota de PowerShell siempre cifra toda la comunicación tras la autenticación inicial con una clave simétrica AES-256 por sesión.
    
### Autenticación inicial

La autenticación confirma la identidad del cliente al servidor, e idealmente, del servidor al cliente.
    
Cuando un cliente se conecta a un servidor de dominio mediante su nombre de equipo (por ejemplo: servidor01 o servidor01.contoso.com), el protocolo de autenticación predeterminado es [Kerberos](https://msdn.microsoft.com/en-us/library/windows/desktop/aa378747.aspx).
Kerberos garantiza la identidad del usuario y del servidor sin enviar ningún tipo de credencial reutilizable.

Cuando un cliente se conecta a un servidor de dominio mediante su dirección IP, o se conecta a un servidor de grupo de trabajo, no es posible la autenticación de Kerberos. En ese caso, la comunicación remota de PowerShell se basa en el [protocolo de autenticación NTLM](https://msdn.microsoft.com/en-us/library/windows/desktop/aa378749.aspx). El protocolo de autenticación NTLM garantiza la identidad del usuario sin enviar ningún tipo de credenciales delegables. Para demostrar la identidad del usuario, el protocolo NTLM requiere que tanto el cliente como el servidor procesen una clave de sesión de la contraseña del usuario sin intercambiar la propia contraseña. El servidor normalmente no conoce la contraseña del usuario, por lo que se comunica con el controlador de dominio que conoce la contraseña del usuario y calcula la clave de sesión del servidor. 
      
Sin embargo, el protocolo NTLM no garantiza la identidad del servidor. De la misma forma que con todos los protocolos que usan NTLM para la autenticación, un atacante con acceso a una cuenta de equipo unido a un dominio podría invocar el controlador de dominio para procesar una clave de sesión NTLM y, por tanto, suplantar el servidor.

La autenticación basada en NTLM está deshabilitada de forma predeterminada, pero puede permitirse al configurar SSL en el servidor de destino o el valor WinRM TrustedHosts.
    
#### Uso de certificados SSL para validar la identidad del servidor durante las conexiones basadas en NTLM

Puesto que el protocolo de autenticación NTLM no puede asegurar la identidad del servidor de destino (solo que ya conoce su contraseña), puede configurar los servidores de destino para usar SSL para la comunicación remota de PowerShell. La asignación de un certificado SSL al servidor de destino (si lo emite una entidad emisora de certificados en la que el cliente también confía) habilita la autenticación basada en NTLM que garantiza la identidad del usuario y del servidor.
    
#### Omisión de errores de identidad de servidor basados en NTLM
      
Si no es factible implementar un certificado SSL en un servidor para las conexiones de NTLM, puede suprimir los errores de identidad resultantes mediante la adición del servidor a la lista WinRM **TrustedHosts**. Tenga en cuenta que la adición de un nombre de servidor a la lista TrustedHosts no se debe considerar como ninguna forma de instrucción de la confiabilidad de los mismos hosts, ya que el protocolo de autenticación NTLM no puede garantizar que, en realidad, se está conectando al host al que pretende conectarse.
En su lugar, debe considerar que el valor de TrustedHosts sea la lista de hosts de la que quiere suprimir el error generado por la imposibilidad de comprobar la identidad del servidor.
    
    
### Comunicación continua

Una vez completada la autenticación inicial, el [Protocolo de comunicación remota de PowerShell](https://msdn.microsoft.com/en-us/library/dd357801.aspx) cifra toda la comunicación continua con una clave simétrica AES-256 por sesión.  


## Creación del segundo salto

De forma predeterminada, la comunicación remota de PowerShell usa Kerberos (si está disponible) o NTLM para la autenticación. Ambos protocolos se autentican en el equipo remoto sin enviarle credenciales.
Este es el modo más seguro de autenticación, pero dado que el equipo remoto no tiene las credenciales de usuario, no puede acceder a otros equipos o servicios en nombre del usuario. 
Esto se conoce como el problema de "Salto doble".

Hay varias formas de evitar este problema:

### Delegación limitada de Kerberos

Para servidores de confianza alta, puede habilitar [Delegación limitada de Kerberos](https://technet.microsoft.com/en-us/library/cc995228.aspx). Esto permite que el servidor remoto suplante el usuario autenticado en una lista especificada de equipos y servicios.

### Confianza entre equipos remotos

Si confía en los usuarios conectados de forma remota a *Server1* a los recursos de *Server2*, puede conceder a *Server1* acceso explícito a esos recursos.

### Uso de credenciales explícitas al obtener acceso a recursos remotos

Puede pasar explícitamente las credenciales a un recurso remoto mediante el parámetro **Credential** de un cmdlet. Por ejemplo:

```powershell
$myCredential = Get-Credential
New-PSDrive -Name Tools \\Server2\Shared\Tools -Credential $myCredential 
```

### CredSSP

Puede usar el [proveedor de compatibilidad para seguridad de credenciales (CredSSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/bb931352.aspx) para la autenticación (especificando "CredSSP" como el valor del parámetro `Authentication` de una llamada al cmdlet [New-PSSession](https://technet.microsoft.com/en-us/library/hh849717.aspx)). CredSSP pasa las credenciales en texto sin formato al servidor, por lo que, al usarlo, le hace vulnerable a los ataques de robos de credenciales. Si el equipo remoto se ve comprometido, el atacante tiene acceso a las credenciales del usuario. CredSSP está deshabilitado de forma predeterminada tanto en el equipo del cliente como del servidor. Debe habilitar CredSSP solo en los entornos de mayor confianza. Por ejemplo, un administrador de dominio que se conecta a un controlador de dominio porque el controlador de dominio es de plena confianza.

Para obtener más información sobre problemas de seguridad cuando se usa CredSSP para la comunicación remota de PowerShell, vea [Accidental Sabotage: Beware of CredSSP](http://www.powershellmagazine.com/2014/03/06/accidental-sabotage-beware-of-credssp) (Sabotaje accidental: tenga cuidado con CredSSP).

Para obtener más información acerca de los ataques de robo de credenciales, consulte [Mitigating Pass-the-Hash (PtH) Attacks and Other Credential Theft](https://www.microsoft.com/en-us/download/details.aspx?id=36036) (Mitigación de ataques Pass-the-Hash (PtH) y otros robos de credenciales).










<!--HONumber=May16_HO3-->


