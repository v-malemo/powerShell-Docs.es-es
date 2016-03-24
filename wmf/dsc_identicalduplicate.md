# Permitir recursos duplicados idénticos en una configuración

DSC no permite ni administra las definiciones de recursos en conflicto dentro de una configuración. En lugar de intentar resolver el conflicto, simplemente no funciona. Dado que la reutilización de la configuración se usa más en los recursos compuestos, etc., se producirán conflictos más a menudo. Cuando las definiciones de recursos en conflicto son idénticas, DSC debe ser inteligente y permitirlo. En esta versión, se admite tener varias instancias de recursos con definiciones idénticas:

```powershell
Configuration IIS_FrontEnd
{
    WindowsFeature FE_IIS         #Identical resource
    {
        Ensure = 'Present'
        Name = 'Web-Server'
    }

    WindowsFeature FTP
    {
        Ensure = 'Present'
        Name = 'Web-FTP-Server'
    }
}

Configuration IIS_Worker
{
    WindowsFeature Worker_IIS      #Identical resource
    {
        Ensure = 'Present'
        Name = 'Web-Server'
    }

    WindowsFeature ASP
    {
        Ensure = 'Present'
        Name = 'Web-ASP-Net45'
    }
}

Configuration WebApplication
{
    IIS_Frontend Web {}

    IIS_Worker ASP {}
}
```

En versiones anteriores, el resultado sería un error de compilación debido a un conflicto entre las instancias de WindowsFeature FE_IIS y WindowsFeature Worker_IIS al intentar comprobar la instalación del rol "Servidor web". Observe que *todas* las propiedades que se están configurando son idénticas en estas dos configuraciones. Puesto que *todas* las propiedades de estos dos recursos son idénticas, el resultado será una compilación correcta. 

Si cualquiera de las propiedades difiere entre los dos recursos, no se considerarán idénticas y se producirá un error de compilación:

```powershell
Configuration IIS_FrontEnd
{
    WindowsFeature FE_IIS
    {
        Ensure = 'Present'     # Ensure is Present here
        Name = 'Web-Server'
    }

    WindowsFeature FTP
    {
        Ensure = 'Present'
        Name = 'Web-FTP-Server'
    }
}

Configuration IIS_Worker
{
    WindowsFeature Worker_IIS
    {
        Ensure = 'Absent'      # Ensure property is Absent instead of Present
        Name = 'Web-Server'
    }

    WindowsFeature ASP
    {
        Ensure = 'Present'
        Name = 'Web-ASP-Net45'
    }
}

Configuration WebApplication
{
    IIS_Frontend Web {}

    IIS_Worker ASP {}
}
```

Esta configuración muy similar se podrá realizarse porque los recursos WindowsFeature FE_IIS y WindowsFeature Worker_IIS ya no son idénticos y, por tanto, están en conflicto.<!--HONumber=Mar16_HO2-->
