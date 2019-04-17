# IIS on Nano server

If you have not seen my full post about all the IIS 10 new features, check it out here.

***NOTE*** that is it not recommended to run IIS on Nano.  Instead you should be using the self-hosted .NET Core scenario.  See here for more information on Server Core with IIS.

Also, if you have not already reviewed my other articles on using IIS on Nano, then you might want to take a look at them.  Just so you know the perspective I am coming from and the context in which I work within.

+ What’s new in IIS 10
+ How to list Packages available when creating a Nano server
+ How to install IIS and Tracing on a Nano for Windows Server 2016
+ How to copy files or deploy code to a Nano Server
+ How to connect and configure IIS running on Nano
+ IIS on Nano server
+ ASP.NET Core with IIS on Nano Server
+ Microsoft IIS Administration on Nano Server –> important for manage.iis.net

Nano server is like Server Core but smaller, cooler some might say.  You can read about it in detail here.  As with Server Core, Nano Server has no GUI so be prepared to perform all the commands to deploy IIS using command line.  To install IIS 10 on a Nano Server, you could use the following, however there are many options, approaches and methods to accomplish this.

```New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd 
     -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package```
     
Then, to install any of the IIS features, you can use DISM accomplish, for example, to install the Application Initialization on Nano, execute the following:

```dism /Enable-Feature /online /featurename:IIS-ApplicationInit /all```

To disable it, execute this command:

```dism /Disable-Feature /online /featurename:IIS-ApplicationInit```

Some other IIS installable features for Nano:

```
IIS-DefaultDocument
IIS-DirectoryBrowsing
IIS-HttpErrors
IIS-StaticContent
IIS-HttpRedirect
IIS-HttpLogging
IIS-CustomLogging
IIS-RequestMonitor
IIS-HttpTracing
IIS-HttpCompressionDynamic
IIS-HttpCompressionStatic
IIS-RequestFiltering
IIS-BasicAuthentication
IIS-ClientCertificateMappingAuthentication
IIS-DigestAuthentication
IIS-IISCertificateMappingAuthentication
IIS-IPSecurity
IIS-URLAuthorization
IIS-WindowsAuthentication
IIS-ApplicationInit
IIS-CGI
IIS-ISAPIExtensions
IIS-ISAPIFilter
IIS-ServerSideIncludes
IIS-WebSockets
```

Some other points to consider:

+ Targets ASP.NET Core
+ Full .NET Framework will not run
+ Supports Java and PHP as well
