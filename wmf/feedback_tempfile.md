# New-TemporaryFile
A veces es necesario crear un archivo temporal en los scripts. Puede hacerlo fácilmente con el cmdlet **New-TemporaryFile**:

PS C:\\&gt; $tempFile = New-TemporaryFile

PS C:\\&gt; $tempFile.FullName

C:\\Users\\slee\\AppData\\Local\\Temp\\tmp375.tmp
<!--HONumber=Mar16_HO2-->
