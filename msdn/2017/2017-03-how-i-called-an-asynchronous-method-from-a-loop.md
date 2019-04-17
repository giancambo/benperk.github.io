# How I called an asynchronous method from a loop

Check out some of my other articles I wrote in reagrds to ASP.NET Core and .Net Core

+ [How to call an async method from a console app main method][https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/08/how-to-call-an-async-method-from-a-console-app-main-method/]
+ How to deploy a .NET Core console application to Azure, WebJob
+ .NET Core application, where is my EXE, how to publish
+ Create a VNET and access an Azure VM hosted within it from an App Services Web App
+ Create and deploy an ASP.NET Core Web API to Azure Windows

I wrote this article here where I showed "How to call an async method from a console app main method", but I needed also to call it numerous times.  Here is the code snippet that illustrates how I did that.

```
public static void Main(string[] args)
{
 try
 {
  int i = 0;
  while (i < 10)
  {
   Task.Run(() => callWebApi()).Wait();
   i++;
  }
 }
 catch (Exception ex)
 {
  WriteLine($"[MAIN] There was an exception: {ex.ToString()}");
 }
}
Simply, I used the Wait() method of the Task.
```

https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/08/how-to-call-an-async-method-from-a-console-app-main-method/
https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/07/how-to-deploy-a-net-core-console-application-to-azure-webjob/
https://blogs.msdn.microsoft.com/benjaminperkins/2017/03/07/net-core-application-where-is-my-exe-how-to-publish/
https://blogs.msdn.microsoft.com/benjaminperkins/2017/02/01/create-a-vnet-and-access-an-azure-vm-hosted-within-it-from-an-app-serivces-web-app/
https://blogs.msdn.microsoft.com/benjaminperkins/2017/01/03/create-and-deploy-an-asp-net-core-web-api-to-azure-windows/
