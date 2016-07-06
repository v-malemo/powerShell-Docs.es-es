# New-Guid
A menudo en un script (o quizás al escribir un recurso de DSC), tiene la necesidad de un identificador único. Los GUID funcionan bien y es fácil llamar a la clase Guid de .NET Framework para generar uno. No obstante, tener un cmdlet facilita la detección a los usuarios finales que todavía no están familiarizados con la clase de .NET Framework:

PS C:\\&gt; New-Guid

GUID

----

e19d6ea5-3cc2-4db9-8095-0cdaed5a703d


<!--HONumber=Jun16_HO4-->


