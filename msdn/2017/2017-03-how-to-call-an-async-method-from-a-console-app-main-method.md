# How to call an async method from a console app main method

I find myself needing to look this code snippet up time to time so I thought I’d write it up.

Check out some of my other articles I wrote in regards to ASP.NET Core and .Net Core

+ How to call an async method from a console app main method
+ How to deploy a .NET Core console application to Azure, WebJob
+ .NET Core application, where is my EXE, how to publish
+ Create a VNET and access an Azure VM hosted within it from an App Services Web App
+ Create and deploy an ASP.NET Core Web API to Azure Windows

I needed to call an ASP.NET Core Web API which I discussed here, from a .NET Core Console Application, created similar to that shown in Figure 1.

![how to create a .NET Core Console Application][FIGURE1]
###### Figure 1, how to create a .NET Core Console Application

Using the GetResponseAsync() method of the System.Net.WebResponce class, as you can see it needs to be call asynchronously.

```
static private async Task callWebApi()
{
  WebResponse response = await WebRequest
      .Create("http://**?**.azurewebsites.net/api/sleepy")
      .GetResponseAsync()
      .ConfigureAwait(false);;
  WriteLine(response.StatusCode.ToString());
}
```

And in order to call and async method from the Main() method, I did the following.

```
public static void Main(string[] args)
{
  try
  {
    callWebApi().Wait();
  }
  catch (Exception ex)
  {
    WriteLine($"There was an exception: {ex.ToString()}");
  }
}
```

You can also call the asynchronous method from the Main() method of the .NET Core console application using the Task.Run() method.

```Task.Run(async () => { await callWebApi(); }).GetAwaiter().GetResult(); ``` (it is not recommend to use the GetResult() method as it will block the thread)

Here are some nice articles written about Async Programming:

+ Async Programming - Brownfield Async Development
+ Async/Await - Best Practices in Asynchronous Programming
+ Async Programming : Introduction to Async/Await on ASP.NET

[FIGURE1]: ../images/2017/msdn-1197.png "Figure 1, how to create a .NET Core Console Application"
