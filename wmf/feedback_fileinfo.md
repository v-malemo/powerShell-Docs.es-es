# Actualizaciones del objeto FileInfo
La información de la versión de archivo puede ser errónea, especialmente en casos en el archivo se revisó. Esta versión de WMF 5.0 agrega las nuevas propiedades de script **FileVersionRaw** y **ProductVersionRaw** 
a los objetos FileInfo. Estas son las propiedades tal como se muestran para powershell.exe (teniendo en cuenta que $pid es el identificador del proceso de PowerShell):

```powershell
PS C:\> Get-Process -Id $pid -FileVersionInfo | fl *version* -Force


FileVersionRaw    : 10.0.10586.117
ProductVersionRaw : 10.0.10586.117
FileVersion       : 10.0.10586.117 (th2_release.160212-2359)
ProductVersion    : 10.0.10586.117


<!--HONumber=Apr16_HO3-->


