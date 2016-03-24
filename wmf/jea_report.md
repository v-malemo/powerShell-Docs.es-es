# Generación de informes en JEA
Para informar sobre el estado de la configuración de JEA, puede usar:
1.  **Get-PSSessionConfiguration** para devolver una lista de todos los puntos de conexión registrados en una máquina determinada.
2.  **Get-PSSessionCapability** para informar de las funcionalidades que cualquier usuario determinado tiene en un punto de conexión concreto.

Este es un ejemplo de **Get-PSSessionCapability**:
```powershell
Get-PSSessionCapability -ConfigurationName Maintenance -Username "CONTOSO\JohnDoe"

CommandType     Name                                               Version    Source           
-----------     ----                                               -------    ------           
Alias           clear -> Clear-Host                                                            
Alias           cls -> Clear-Host                                                              
Alias           exsn -> Exit-PSSession                                                         
Alias           gcm -> Get-Command                                                             
Alias           measure -> Measure-Object                                                      
Alias           select -> Select-Object                                                        
Function        Clear-Host                                                                     
Function        Exit-PSSession                                                                 
Function        Get-Command                                                                    
Function        Get-FormatData                                                                 
Function        Get-Help                                                                       
Function        Get-UserInfo                                                                   
Function        Measure-Object                                                                 
Function        Out-Default                                                                    
Function        Select-Object                                                                  
Cmdlet          Restart-Service                                    3.0.0.0 Microsof...


```

Para informar sobre las _acciones_ que los usuarios emprendieron durante una sesión de JEA, puede hacer lo siguiente:
1. Habilitar las transcripciones de "consentimiento temporal" para ese punto de conexión de JEA y consulte el directorio de transcripción para obtener un registro completo de las acciones de cada usuario
2. Active el registro del módulo de PowerShell e inspeccione los registros de eventos de PowerShell.<!--HONumber=Mar16_HO2-->
