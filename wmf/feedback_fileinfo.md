# Actualizaciones del objeto FileInfo
La información de la versión de archivo puede ser errónea, especialmente en casos en el archivo se revisó. Esta versión de WMF Production Preview agrega las nuevas propiedades de script **FileVersionRaw** y **ProductVersionRaw** a los objetos FileInfo. Estas son las propiedades tal como se muestran para powershell.exe:

PS C:\\&gt; Get-Process -Id $pid -FileVersionInfo | fl \*version\* -Force

FileVersionRaw: 10.0.10055.0

ProductVersionRaw: 10.0.10055.0

FileVersion: 10.0.10055.0 (fbl\_srv2.150402-1826)

ProductVersion: 10.0.10055.0
<!--HONumber=Mar16_HO2-->
