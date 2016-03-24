# Get-ChildItem tiene el parámetro -Depth
**Get-ChildItem** tiene ahora un parámetro **–Depth** que se usa con **–Recurse** para limitar la recursividad:

PS C:\\Users\\slee\\Downloads\\Example&gt; Get-ChildItem -Recurse -Depth 0

Directorio: C:\\Users\\slee\\Downloads\\Example

Nombre Hora de última escritura Longitud Nombre

---- ------------- ------ ----

d----- 4/14/2015 5:36 PM Depth0

-a---- 4/14/2015 1:19 PM 0 File1.txt

-a---- 4/14/2015 1:19 PM 0 File2.txt

-a---- 4/14/2015 1:19 PM 0 File3.txt

PS C:\\Users\\slee\\Downloads\\Example&gt; Get-ChildItem -Recurse -Depth 1

Directorio: C:\\Users\\slee\\Downloads\\Example

Nombre Hora de última escritura Longitud Nombre

---- ------------- ------ ----

d----- 4/14/2015 5:36 PM Depth0

-a---- 4/14/2015 1:19 PM 0 File1.txt

-a---- 4/14/2015 1:19 PM 0 File2.txt

-a---- 4/14/2015 1:19 PM 0 File3.txt

Directorio: C:\\Users\\slee\\Downloads\\Example\\Depth0

Nombre Hora de última escritura Longitud Nombre

---- ------------- ------ ----

d----- 4/14/2015 5:33 PM Depth1
<!--HONumber=Mar16_HO2-->
