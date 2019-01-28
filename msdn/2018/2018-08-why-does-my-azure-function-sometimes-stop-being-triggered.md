# Why does my Azure Function sometimes stop being triggered

Azure is so awesome because the features it provides are so very dynamic and flexible.  The platform provides a starting point for the fruition of ideas, ideas only limited by ones imagination.  With its dynamic nature and the flexibility it delivers to consumers, sometimes you just need a note to get your ideas optimally flowing on the platform.

Four points (for now) you need to keep in mind when creating and managing your Azure Function App:

+ All your Azure Functions run within a Azure Function App
+ The way in which you deploy your Azure Function is important
+ The trigger type you choose comes with scenarios specific only (in some cases) to that trigger type
+ Your endpoint must trigger/bind to only one Azure Function

Like with an Azure App Service, where each runs within its own W3WP process so does the Azure Function App.  If you then access [KUDU/SCM][LINK1] and navigate to the D:\home\site\wwwroot directory via the CMD console, you will see the Azure Functions which run inside that same W3WP process, Figure 1.

![why does my Azure Function not get triggered, Azure Function stops][FIGURE1]
###### Figure 1, why does my Azure Function not get triggered, Azure Function stops

This correlates one-to-one with what is seen in the Azure portal, Figure 2.

![why does my Azure Function not get triggered, Azure Function stops][FIGURE2]
###### Figure 2, why does my Azure Function not get triggered, Azure Function stops

***There are numerous methods for deploying***, managing and creating an Azure Function, I wrote a few methods here:

+ [How to create an Azure Function in Visual Studio][LINK2]
+ [Deploy an Azure Function created from Visual Studio][LINK3]
+ [How to configure GitHub or VSTS for use with an Azure Function][LINK4]
+ [Debugging Azure Functions in Visual Studio][LINK5]

The issue most know for causing Azure Functions to not trigger is that some deployment or IDEs do not trigger a synchronization on a Consumption plan with the central listener.  See [here][LINK10] “New integrated portal for Azure Functions” and [here][LINK11] “Document deployment options related to trigger syncing”.  Those articles explain a bit more about this.

You can trigger a synchronization using the following PowerShell command.

```
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId "<Subscription Id>"
$WebAppApiVersion = "2018-02-01"
$ResourceType = "Microsoft.Web/sites"
$ResourceGroupName = "<Resource Group Name>"
$SiteName = "<Azure Function App Name>"
Write-Output "PowerShell function executed at:$(get-date)";
Invoke-AzureRmResourceAction
     -ResourceGroupName $ResourceGroupName
     -ResourceType $ResourceType
     -ResourceName $SiteName
     -Action syncfunctiontriggers
     -ApiVersion $WebAppApiVersion -Force
```

There are numerous trigger types and bindings.  Here are a few:

+ [Azure Blob storage bindings for Azure Functions][LINK6]
+ [Azure Event Hubs bindings for Azure Functions][LINK7]
+ [Azure Service Bus bindings for Azure Functions][LINK8]
+ [Azure Functions HTTP and webhook bindings][LINK9]
+ …

Study each of the binding types and you learn the unique attributes of each.

Let's say you have an Event Hub, then you create an Azure Function App, that includes an Azure Function which is triggered when messages are received into that Event Hub.  If at some point in time later, you create a new Azure Function App (it doesn't have to be a new Azure Function App, it could be a new Azure Function within the same Azure Function App) that includes an Azure Function that is triggered/bound to the same Event Hub as the other, then that one will begin getting the message and the original one will stop.  ***You can only have one binding***.  That is why you seen in Figure 2, previously, that I have disabled some of my Azure Functions.  This was becuase I had created another Azure Function App that included an Azure Function bound to the same Event Hub and Blob and disabling them was the solution for the scenario and experience I document here now.

[LINK1]: 2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK2]: tbd
[LINK3]: tbd
[LINK4]: tbd
[LINK5]: tbd
[LINK6]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob
[LINK7]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LINK8]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus
[LINK9]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook
[LINK10]: https://blogs.msdn.microsoft.com/appserviceteam/2017/04/10/new-integrated-portal-for-azure-functions/
[LINK11]: https://github.com/Azure/Azure-Functions/issues/210

[FIGURE1]: ../images/2018/msdn-0070.png "Figure 1, why does my Azure Function not get triggered, Azure Function stops"
[FIGURE2]: ../images/2018/msdn-0071.png "Figure 2, why does my Azure Function not get triggered, Azure Function stops"
