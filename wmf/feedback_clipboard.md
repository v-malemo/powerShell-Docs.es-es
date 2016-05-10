# Cmdlets Clipboard
**Get-Clipboard** y **Set-Clipboard** facilitan la transferencia de contenido a y desde una sesión de Windows PowerShell. Por ejemplo, si usa el Explorador de Windows para copiar tres archivos
en el Portapapeles (seleccionándolos y pulsando `ctrl-c`, por ejemplo), puede tener acceso sencillo al contenido del Portapapeles como una lista de archivos:

```powershell 
PS C:\\&gt; Get-Clipboard -Format FileDropList

Directory: C:\\Users\\slee\\Downloads\\Example

Mode LastWriteTime Length Name

---- ------------- ------ ----

-a---- 4/14/2015 1:19 PM 0 File2.txt

-a---- 4/14/2015 1:19 PM 0 File3.txt

-a---- 4/14/2015 1:19 PM 0 File1.txt
```


Los cmdlets Clipboard admiten imágenes, archivos de audio, listas de archivos y texto.


<!--HONumber=Apr16_HO3-->


