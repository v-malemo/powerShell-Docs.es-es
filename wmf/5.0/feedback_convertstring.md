# Convert-String
**Convert-String** expone la funcionalidad "reemplazar por arte de magia". Proporcione ejemplos del antes y el después del aspecto que quiere que tenga el texto que se va a buscar y **Convert-String** dará formato al texto automáticamente. Aquí tiene una demostración: tomamos el nombre y el apellido de alguien, los reemplazamos por su apellido, una coma, la inicial del apellido y un punto. Pruébelo con una regex y vea cuánto tarda en hacerlo.

```powershell
"Lee Holmes", "Steve Lee", "Jeffrey Snover" | Convert-String -Example "Bill Gates=Gates, B.","John Smith=Smith, J."

Holmes, L.
Lee, S.
Snover, J.
```
<!--HONumber=Mar16_HO2-->
