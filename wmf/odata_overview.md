# Generar cmdlets de PowerShell basados en el punto de conexión de OData
Generar cmdlets de Windows PowerShell basados en el punto de conexión de OData
--------------------------------------------------------------

**Export-ODataEndpointProxy** es un cmdlet que genera un conjunto de cmdlets de Windows PowerShell basados en la funcionalidad expuesta por un punto de conexión de OData determinado.

El siguiente ejemplo muestra cómo usar este nuevo cmdlet:

\# Caso de uso básico de Export-ODataEndpointProxy

```powershell
Export-ODataEndpointProxy -Uri 'http://services.odata.org/v3/(S(snyobsk1hhutkb2yulwldgf1))/odata/odata.svc' -OutputModule C:\Users\user\Generated.psd1

ipmo 'C:\Users\user\Generated.psd1'
# Cmdlets are created based on the following heuristics
# New-<EntityType> -<Key> [-<Other Attributes>]
#
# Get-<EntityType> [-<Key> -Top –Skip –Filter -OrderBy]
# # If there is a complex key, the keys will actually be -<Key1> -<Key2>…
# # Note this rule applies to any other instances of the key
#
# Set-<EntityType> -<Key> [-<Other Attributes>]
#
# Remove-<EntityType> -<Key>
#
# Invoke-<EntityType><Action> [-<Key> -<Other Parameters>]
#
#
# Cmdlets from associations (Note: Get and Remove get additional parameter sets)
# Get-<EntityType> -<AssociatedEntity>
# New-<EntityType> -<AssociatedEntity> -<Key>
# Remove-<EntityType> -<AssociatedEntity> -<Key>
#
#
# Note: Every cmdlet has the –ConnectionURI parameter for explicitly setting the URI of the endpoint. This normally uses the same address that you gave the Export-ODataEndpointProxy cmdlet, but can be overridden in this fashion for the sake of similar endpoints.
#
```

Todavía hay partes de casos de uso clave en el desarrollo de esta funcionalidad. Son, sin limitación:
-   Asociaciones
-   Flujos de paso

Generar cmdlets de Windows PowerShell basados en un punto de conexión de OData con ODataUtils
------------------------------------------------------------------------------
El módulo ODataUtils permite la generación de cmdlets de Windows PowerShell desde los puntos de conexión de REST que admiten OData. Las siguientes mejoras incrementales se incluyen en el módulo Microsoft.PowerShell.ODataUtils de Windows PowerShell.
-   Canalización de información adicional del punto de conexión del lado servidor al lado cliente.
-   Compatibilidad con la paginación del lado cliente.
-   Filtrado del lado servidor mediante el parámetro -Select.
-   Compatibilidad con los encabezados de solicitud web.

Los cmdlets de proxy que genera el cmdlet Export-ODataEndPointProxy proporcionan información adicional (no mencionada en los $metadatos usados durante la generación de proxy del lado cliente) del punto de conexión de OData lado servidor en el flujo de información (una nueva característica de Windows PowerShell 5.0). A continuación se ofrece un ejemplo de cómo obtener esa información.
```powershell
Import-Module Microsoft.PowerShell.ODataUtils -Force
$generatedProxyModuleDir = Join-Path -Path $env:SystemDrive -ChildPath 'ODataDemoProxy'
$uri = "http://services.odata.org/V3/(S(fhleiief23wrm5a5nhf542q5))/OData/OData.svc/"
Export-ODataEndpointProxy -Uri $uri -OutputModule $generatedProxyModuleDir -Force -AllowUnSecureConnection -Verbose -AllowClobber
Import-Module $generatedProxyModuleDir -Force

# In the below command, we are retrieving top 1 product.
# By specifying -IncludeTotalResponseCount parameter,
# we are getting the total count of all the Product records
# available on the server side. This information
# is surfaced on the client side through the Information stream.
$product = Get-Product -Top 1 -AllowUnsecureConnection -AllowAdditionalData -IncludeTotalResponseCount -InformationVariable infoStream
# The Information stream contains the additional
# information sent from the server side.
$additionalInfo = $infoStream.GetEnumerator() | % MessageData
# 'Odata.Count' indicates the total product records
# available on the server side Odata endpoint.
$additionalInfo['odata.count']
```

Puede obtener los registros del lado servidor en lotes mediante la compatibilidad con la paginación del lado cliente. Resulta útil cuando se necesita obtener una gran cantidad de datos del servidor a través de la red.
```powershell
$skipCount = 0
$batchSize = 3
# Client-Side Paging Support: The records from the server side
# are retrieved in batches of $batchSize
while($skipCount -le $additionalInfo['odata.count'])
{
Get-Product -AllowUnsecureConnection -AllowAdditionalData -Top $batchSize -Skip $skipCount
$skipCount += $batchSize
}
```

Los cmdlets de proxy generados admiten el parámetro –Select, que puede usar como un filtro para recibir solamente las propiedades de registro que el cliente necesita. Esto reduce la cantidad de datos que se transfieren a través de la red, ya que el filtrado se produce en el lado servidor.
```powershell
# In the below example only the Name property of the
# Product record is retrieved from the server side.
Get-Product -Top 2 -AllowUnsecureConnection -AllowAdditionalData -Select Name
```

El cmdlet Export-ODataEndpointProxy y los cmdlets de proxy que este genera admite ahora el parámetro Headers (proporcione valores como una tabla hash), que puede usar para canalizar cualquier información adicional que espere el punto de conexión de OData del lado servidor. En el ejemplo siguiente, puede canalizar una clave de suscripción a través de los encabezados de los servicios que esperan una clave de suscripción para la autenticación.
```powershell
# As an example, in the below command 'XXXX' is the authentication used by the
# Export-ODataEndpointProxy cmdlet to interact with the server-side
# OData endpoint accessed through $endPointUri.

Export-ODataEndpointProxy -Uri $endPointUri -OutputModule $generatedProxyModuleDir -Force -AllowUnSecureConnection -Verbose -Headers @{'subscription-key'='XXXX'}
```
<!--HONumber=Mar16_HO2-->
