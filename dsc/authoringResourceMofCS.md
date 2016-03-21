# Creación de un recurso de DSC en C`#`

> Se aplica a: Windows PowerShell 4.0, Windows PowerShell 5.0

Normalmente, un recurso personalizado de configuración de estado deseado (DSC) de Windows PowerShell se implementa en un script de PowerShell. Sin embargo, también puede implementar la funcionalidad de un recurso de DSC personalizado mediante la escritura de cmdlets en C#. Para ver una introducción sobre la escritura de cmdlets en C#, consulte [Escribir un cmdlet de Windows PowerShell](https://technet.microsoft.com/en-us/library/dd878294.aspx).

Además de implementar el recurso en C# como cmdlets, el proceso de creación del esquema MOF, la creación de la estructura de carpetas, la importación y la utilización del recurso personalizado de DSC son idénticos a cómo se describen en [Escribir un recurso de DSC personalizado con MOF](authoringResourceMOF.md).

## Escribir un recurso basado en cmdlets
En este ejemplo, se implementará un recurso simple que administra un archivo de texto y su contenido.

### Escribir el esquema MOF

A continuación se muestra la definición del recurso MOF.

```
[ClassVersion("1.0.0"), FriendlyName("xDemoFile")]
class MSFT_XDemoFile : OMI_BaseResource
{
                [Key, Description("path")] String Path;
                [Write, Description("Should the file be present"), ValueMap{"Present","Absent"}, Values{"Present","Absent"}] String Ensure;
                [Write, Description("Contentof file.")] String Content;                   
};
```

### Configurar el proyecto de Visual Studio
#### Configurar un proyecto de cmdlet

1. Abra Visual Studio.
1. Cree un proyecto de C# y proporcione el nombre.
1. Seleccione **Biblioteca de clases** en las plantillas de proyecto disponibles.
1. Haga clic en **Aceptar**.
1. Agregue al proyecto una referencia de ensamblado a System.Automation.Management.dll.
1. Cambie el nombre del ensamblado para que coincida con el nombre del recurso. En este caso, el ensamblado debe tener como nombre **MSFT_XDemoFile**.

### Escribir el código del cmdlet

El siguiente código de C# implementa los cmdlets **Get-TargetResource**, **Set-TargetResource** y **Test-TargetResource**.

```C#
Get-TargetResource
       [OutputType(typeof(System.Collections.Hashtable))]
       [Cmdlet(VerbsCommon.Get, "TargetResource")]
       public class GetTargetResource : PSCmdlet
       {
              [Parameter(Mandatory = true)]
              public string Path { get; set; }
 
///<summary>
/// Implement the logic to write the current state of the resource as a 
/// Hash table with keys being the resource properties 
/// and the Values are the corresponding current values on the target machine.
 
///</summary>
              protected override void ProcessRecord()
              {
// Download the zip file at the end of this blog to see sample implementation.
 }
 
Set-TargetResouce
         [OutputType(typeof(void))]
    [Cmdlet(VerbsCommon.Set, "TargetResource")]
    public class SetTargetResource : PSCmdlet
    {
        privatestring _ensure;
        privatestring _content;
        
[Parameter(Mandatory = true)]
        public string Path { get; set; }
        
[Parameter(Mandatory = false)]      
       [ValidateSet("Present", "Absent", IgnoreCase = true)]
       public string Ensure {
            get
            {
                // set the default to present.
               return (this._ensure ?? "Present");
            }
            set
            {
                this._ensure = value;
            }
           } 
            public string Content {
            get { return (string.IsNullOrEmpty(this._content) ? "" : this._content); }
            set { this._content = value; }
        }
 
///<summary>
        /// Implement the logic to set the state of the machine to the desired state.
        ///</summary>
        protected override void ProcessRecord()
        {
//Implement the set method of the resource 
/* Uncomment this section if your resource needs a machine reboot.
PSVariable DscMachineStatus = new PSVariable("DSCMachineStatus", 1, ScopedItemOptions.AllScope);
                this.SessionState.PSVariable.Set(DscMachineStatus);
*/     
  }
    }
Test-TargetResource    
       [Cmdlet("Test", "TargetResource")]
    [OutputType(typeof(Boolean))]
    public class TestTargetResource : PSCmdlet
    {   
        
        private string _ensure;
        private string _content;
 
        [Parameter(Mandatory = true)]
        public string Path { get; set; }
 
        [Parameter(Mandatory = false)]
        [ValidateSet("Present", "Absent", IgnoreCase = true)]
        public string Ensure
        {
            get
            {
                // set the default to present.
                return (this._ensure ?? "Present");
            }
            set
            {
                this._ensure = value;
            }
        }
 
        [Parameter(Mandatory = false)]
        public string Content
        {
            get { return (string.IsNullOrEmpty(this._content) ? "“:this._content);}
            set { this._content = value; }
        }
 
///<summary>
/// Write a Boolean value which indicates whether the current machine is in    
/// desired state or not.
        ///</summary>
        protected override void ProcessRecord()
        {
                // Implement the test method of the resource.
        }
}
```

### Implementar el recurso

El archivo dll compilado debe guardarse en una estructura de archivos similar a un recurso de script. A continuación se muestra la estructura de carpetas de este recurso.

```
$env: psmodulepath (folder)
    |- MyDscResources (folder)
        |- DSCResources (folder)
            |- MSFT_XDemoFile (folder)
                |- MSFT_XDemoFile.psd1 (file, optional)
                |- MSFT_XDemoFile.dll (file, required)
                |- MSFT_XDemoFile.schema.mof (file, required)
```

### Consulte también
#### Conceptos
[Escribir un recurso de DSC personalizado con MOF](authoringResourceMOF.md)
#### Otros recursos
[Escribir un cmdlet de Windows PowerShell](https://msdn.microsoft.com/en-us/library/dd878294.aspx)<!--HONumber=Feb16_HO4-->
