# .NET Core application, where is my EXE, how to publish

Short answer, there isn’t one.  When you compile your .NET Core console application for example, you get a DLL.  Then you execute it using the DOTNET command from the .NET Core SDK found [here](https://www.microsoft.com/net/core).

Check out some of my other articles I wrote in regards to ASP.NET Core and .Net Core

+ [How to call an async method from a console app main method](https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/08/how-to-call-an-async-method-from-a-console-app-main-method/)
+ [How to deploy a .NET Core console application to Azure, WebJob](https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/07/how-to-deploy-a-net-core-console-application-to-azure-webjob/)
+ [.NET Core application, where is my EXE, how to publish](https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/07/net-core-application-where-is-my-exe-how-to-publish/)
+ [Create a VNET and access an Azure VM hosted within it from an App Services Web App](https://blogs.msdn.microsoft.com/benjaminperkins/2017/02/01/create-a-vnet-and-access-an-azure-vm-hosted-within-it-from-an-app-serivces-web-app/)
+ [Create and deploy an ASP.NET Core Web API to Azure Windows](https://blogs.msdn.microsoft.com/benjaminperkins/2017/01/03/create-and-deploy-an-asp-net-core-web-api-to-azure-windows/)

I wrote 3 other articles about a .NET Core console application [here](https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/08/how-to-call-an-async-method-from-a-console-app-main-method/), [here](https://blogs.msdn.microsoft.com/benjaminperkins/2017/01/03/create-and-deploy-an-asp-net-core-web-api-to-azure-windows/) and [here](https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/07/net-core-application-where-is-my-exe-how-to-publish/) which resulted in the creation of a DLL called: helloworldsleepy-webjob.dll

I right-clicked my .NET Core console application and select the Publish… menu item, which opened a wizard, similar to that shown in Figure 1.

![how to publish a .NET Core application][FIGURE1]
###### Figure 1, how to publish a .NET Core application

Select the location where you want the PublishOutput to be placed and click the Publish button, see Figure 2.

![how to publish a .NET Core application, no EXE?][FIGURE2]
###### Figure 2, how to publish a .NET Core application, no EXE?

For testing, I then copied the contents of that directory into C:\Temp\helloworldsleepy-webjob and ran DOTNET helloworldsleepy-webjob.dll as shown in Figure 3.

![how to execute a .NET Core console application][FIGURE3]
###### Figure 3, how to execute a .NET Core console application

It runs as expected.

[FIGURE1]: ../images/2017/msdn-1188.png "Figure 1, how to publish a .NET Core application"
[FIGURE2]: ../images/2017/msdn-1189.png "Figure 2, how to publish a .NET Core application, no EXE?"
[FIGURE3]: ../images/2017/msdn-1190.png "Figure 3, how to execute a .NET Core console application"
