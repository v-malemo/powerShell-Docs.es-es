# Parámetro NoNewLine
**Out-File**, **Add-Content** y **Set-Content** tienen ahora un nuevo modificador **–NoNewline**, que simplemente omite una nueva línea después de la salida.
```PowerShell
PS C:\> "This is " | Out-File -FilePath Example.txt -NoNewline

PS C:\>; "a single " | Add-Content -Path Example.txt -NoNewline

PS C:\> "sentence." | Add-Content -Path Example.txt -NoNewline

PS C:\> Get-Content .\Example.txt

This is a single sentence.
```
Sin **–NoNewline** especificado, cada fragmento estaría en una línea diferente:
```PowerShell
PS C:\> "This is " | Out-File -FilePath Example.txt

PS C:\> "a single " | Add-Content -Path Example.txt

PS C:\> "sentence." | Add-Content -Path Example.txt

PS C:\> Get-Content .\Example.txt

This is

a single

sentence.
```
<!--HONumber=Mar16_HO2-->
