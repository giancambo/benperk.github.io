# Azure Functions Labs – Information and setup instructions
I started my learning of Azure Functions, I.e. Service-less computing by writing this .NET Core Console application which calls the following Azure Functions each with a different trigger type.  I hosted the console application on GitHub here.  You will need the .NET Core runtime installed on your work station to run the console app, check this out.
## Labs
Before beginning the following labs, be sure to complete the Getting Started section further down in this article.

| Lab Name | Lab Location | Deatils | 
| -------- | ------------ | ------- |
| Lab 1: Azure Functions - Blob Storage    | Lab 1, Lab 1.1          | [Blob Storage][LABLINK1]   |
| Lab 2: Azure Functions - Cosmos DB       | Lab 2, Lab 2.1, Lab 2.2 | [Cosmos DB][LABLINK2]      |
| Lab 3: Azure Functions - Event Grid      | TBD                     | [Event Grid][LABLINK3]     |
| Lab 4: Azure Functions - Event Hub       | Lab 4, Lab 4.1          | [Event Hubs][LABLINK4]     |
| Lab 5: Azure Functions                   |                         |                            |
| Lab 6: Azure Functions                   |                         |                            |
| Lab 7: Azure Functions - HTTP trigger    | Lab 7, Lab 7.1, Lab 7.2 | [HTTP Triggers][LABLINK7]  |
| Lab 8: Azure Functions - Microsoft Graph | Lab 8                   | [Microsoft Graph][LABLINK8]|
| Lab 9: Azure Functions                   |                         |                            |
| Lab 10: Azure Functions                  |                         |                            |
| Lab 11: Azure Functions – Storage Queue  | Lab 11                  | [Storage Queue][LABLINK11] | 
| Lab 12: Azure Functions - SendGrid       |                         |                            |
| Lab 13: Azure Functions - Service Bus    | Lab 13, Lab 13.1        | [Service Bus][LABLINK13]   |
| Lab 14: Azure Functions - SignalR        |                         |                            |
| Lab 15: Azure Functions - Table Storage  |                         |                            |
| Lab 16: Azure Functions - Timer          | Lab 16                  | [Timer trigger][LABLINK16] |
| Lab 17: Azure Functions                  |                         |                            |
| Lab 18: Durable Functions                |                         |                            |
| Lab 19: Binding expressions and patterns |                         | [Patterns][LABLINK19]      |

**NOTE:** For all of the trigger types an endpoint is a requirement.  An endpoint, in generic terms, is nothing more than a accessible unique identifier that is used trigger code.
# .NET Framework vs. .NET Core Framework
Did you know Azure Functions 1.x runs(ran) with .NET Framework and 2.x runs with the .NET Core Framework
# Azure Functions languages
Although these labs focus mostly on C#, because that is my favorite, you can write Azure Functions in many languages.  See the list here.
# Host.json
To be an expert in regards to Azure Functions, you need to know all the attributes and configurations possible for a Function App.  Read about them [here][LINK1] “host.json reference for Azure Functions”.  Note that the host.json file lives in the wwwroot directory of the Function App and is therefore a logical conclusion that changes to that file impact the Function App, I.e. not only the function.  There is a function.json file which can be used to make function specific configurations.  You must also recognize, perfect and understand the specific configurations per binding type.  Each trigger type has some unique settings, like:
+ maxBatchSize, prefetchCount, batchCheckpointFrequency
+ GatewayMode, Protocol, leasePrefix
+ and many, many more…
# Bindings and integrations
Bindings in Azure Functions 1.0 were native which made them hard to update (.NET Framework 4.6).  It was difficult because the native bindings were bound to the runtime and making a change to the binding required a change to the runtime.  With Azure Function 2.0 (.NET Standard 2.0) the bindings can be versioned independent of the runtime and are developed separately.   This means each team can develop capabilities at their own pace.  Here is a list of Azure Function v2 bindings:
+ HTTP
+ Timer
+ Microsoft.Azure.WebJobs.Extensions.Storage
+ Microsoft.Azure.WebJobs.Extensions.ServiceBus
+ Microsoft.Azure.WebJobs.Extensions.EventHubs
+ Microsoft.Azure.WebJobs.Extensions.CosmosDB
+ Microsoft.Azure.WebJobs.Extensions..EventGrid
+ Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph
+ Microsoft.Azure.WebJobs.Extensions.DurableTask
+ Microsoft.Azure.WebJobs.Extensions.SignalRService
When you create a trigger using one of the above types, you are prompted to install the required extension for the selected binding.  Something similar to that shown in Figure 1.
 
![create an Azure Function App, installing extensions][FIGURE1]
 
The source code for the extensions is [here][LINK2].
The list of supported bindings are [here][LINK3].
TIP: If you ever need to install an extension manually, add the extension to the extensions.csproj file and run the following command in KUDU/SCM.
```dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget```




[LABLINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-blob
[LABLINK2]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb-v2
[LABLINK3]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LABLINK4]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LABLINK7]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook
[LABLINK8]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-microsoft-graph
[LABLINK11]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-storage-queue
[LABLINK13]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus
[LABLINK16]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-timer
[LABLINK19]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings#binding-expressions-and-patterns

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-host-json
[LINK2]: https://github.com/Azure/azure-webjobs-sdk-extensions
[LINK3]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-versions#bindings

[FIGURE1]: ../images/azure-0002.png "Figure 1, create an Azure Function App, installing extensions"


