# Azure Functions Labs – Information and setup instructions
I started my learning of Azure Functions, I.e. Service-less computing by writing this .NET Core Console application which calls the following Azure Functions each with a different trigger type.  I hosted the console application on GitHub here.  You will need the .NET Core runtime installed on your work station to run the console app, check this out.
## Labs
Before beginning the following labs, be sure to complete the [Getting Started][ANCHORLINK1] section further down in this article.

| Lab Name | Lab Location | Deatils | 
| -------- | ------------ | ------- |
| Lab 1: Azure Functions - Blob Storage    | [Lab 1][LINK13], [Lab 1.1][LINK14]                    | [Blob Storage][LABLINK1]   |
| Lab 2: Azure Functions - Cosmos DB       | [Lab 2][LINK15], [Lab 2.1][LINK16], [Lab 2.2][LINK17] | [Cosmos DB][LABLINK2]      |
| Lab 3: Azure Functions - Event Grid      | TBD                     | [Event Grid][LABLINK3]      |
| Lab 4: Azure Functions - Event Hub       | [Lab 4][LINK18], [Lab 4.1][LINK19]                    | [Event Hubs][LABLINK4]     |
| Lab 5: Azure Functions                   |                         |                             |
| Lab 6: Azure Functions                   |                         |                             |
| Lab 7: Azure Functions - HTTP trigger    | [Lab 7][LINK20], [Lab 7.1][LINK21], [Lab 7.2][LINK22] | [HTTP Triggers][LABLINK7]  |
| Lab 8: Azure Functions - Microsoft Graph | [Lab 8][LINK23]                                       | [Microsoft Graph][LABLINK8]|
| Lab 9: Azure Functions                   |                         |                             |
| Lab 10: Azure Functions                  |                         |                             |
| Lab 11: Azure Functions – Storage Queue  | [Lab 11][LINK24]                                      | [Storage Queue][LABLINK11] | 
| Lab 12: Azure Functions - SendGrid       |                         |                             |
| Lab 13: Azure Functions - Service Bus    | [Lab 13][LINK25], [Lab 13.1][LINK26]                  | [Service Bus][LABLINK13]   |
| Lab 14: Azure Functions - SignalR        |                         |                             |
| Lab 15: Azure Functions - Table Storage  |                         |                             |
| Lab 16: Azure Functions - Timer          | [Lab 16][LINK27], [Lab 16.1][[LINK28]                 | [Timer trigger][LABLINK16] |
| Lab 17: Azure Functions                  |                         |                            |
| Lab 18: Durable Functions                |                         |                            |
| Lab 19: Binding expressions and patterns |                         | [Patterns][LABLINK19]      |

**NOTE:** For all of the trigger types an endpoint is a requirement.  An endpoint, in generic terms, is nothing more than a accessible unique identifier that is used trigger code.
## .NET Framework vs. .NET Core Framework
Did you know Azure Functions 1.x runs(ran) with .NET Framework and 2.x runs with the .NET Core Framework
## Azure Functions languages
Although these labs focus mostly on C#, because that is my favorite, you can write Azure Functions in many languages.  See the list here.
## Host.json
To be an expert in regards to Azure Functions, you need to know all the attributes and configurations possible for a Function App.  Read about them [here][LINK1] “host.json reference for Azure Functions”.  Note that the host.json file lives in the wwwroot directory of the Function App and is therefore a logical conclusion that changes to that file impact the Function App, I.e. not only the function.  There is a function.json file which can be used to make function specific configurations.  You must also recognize, perfect and understand the specific configurations per binding type.  Each trigger type has some unique settings, like:
+ maxBatchSize, prefetchCount, batchCheckpointFrequency
+ GatewayMode, Protocol, leasePrefix
+ and many, many more…
## Bindings and integrations
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
 
+ The source code for the extensions is [here][LINK2].
+ The list of supported bindings are [here][LINK3].
+ See [here][LINK12] to compare messaging services.

**TIP:** If you ever need to install an extension manually, add the extension to the extensions.csproj file and run the following command in KUDU/SCM.

```dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget```
## Declarative vs. Imperative Azure Functions
By default, Azure Functions use a declarative binding pattern that utilizes the function.json configuration file to define both input and output binding definitions.  For example, a default declarative binding pattern for an HTTP Triggered Azure Functions is similar to the following.  It includes the authLevel, the name of the input parameter passed into the Azure Function Run() method and the direction, etc.
```
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

With an HTTP Triggered Azure Function there is also a default output binding that returns the Task<HttpResponseMessage> to the client that triggered the Azure Function.  An Event Hub or Blob Trigger Azure Function, for example, do not have a default output binding.  You can add additional inputs or outputs for an Azure Function using the portal, Figure 2, or if you know the pattern you can update the json file directly,  Manually making the change to the json file via the portal can lead the issue I discussed [here][LINK4].

![create an Azure Function App, configuring inputs and outputs][FIGURE2]

If there is a need to add an additional output binding for a Blob Storage account, then the function.json file would be updated to include this snippet.  It includes the type, the name of the output parameter that I can reference from the parameters in the Run() method, the direction and some others…


```
{
      "type": "blob",
      "name": "outputBlob",
      "path": "test/{rand-guid}",
      "connection": "AzureWebJobsDashboard",
      "direction": "out"
}
```

Now, onto imperative.  Think of the difference between Declarative and Imperative in the same way you think about design-time vs. runtime.  When you know from the beginning which combination of bindings the Azure Function requires, then create them declaratively, however, if, based on the processing of the input to the Azure Function, the output binding can have different endpoints, then imperative binding is necessary.
The way you imperatively (or at runtime) define bindings for an Azure Function is by passing the [Binder][LINK5] input parameter into the Run() method of the Azure Function, similar to the following.

```
Run(string input, Binder binder)
```

Then, within the Run() method, you can use the BindAsync<T>() method to dynamically create the output binding and utilize it.  For example, if instead of the outputBlob writing to path “test/” (which was declared declaratively) that can be identified at runtime, as shown in the following snippet.
 
```
public static async Task Run(string input, string name, Binder binder, TraceWriter log)
{
	string path = $"coolcontainer\{name}";
	var attributes = new Attribute[]
    {    
        new BlobAttribute(path),
        new StorageAccountAttribute("")
    };
	using (var writer = await binder.BindAsync(attributes))
    {
        writer.Write(input);
    }
}
```
**NOTE:** do not declaratively declare an output binding if you plan to define them imperatively in the code.
Here are more links that can add more details about runtime descriptions of out bound triggers.
+ [Triggers and Bindings - Binding at runtime][LINK6]
+ [C# class – Binding at runtime][LINK7]
+ [C# script – Binding at runtime][LINK8]

**NOTE:** I am actually leaning for towards using the client libraries for outbound triggers instead of the Binder approach.  Let’s see how this progresses over time.
## Inbound vs. Outbound triggers
All Function Apps cannot create all outbound types of connections…

![inbound vs. outbound triggers][FIGURE]

## Dedicated vs. Consumption vs. Premium
The full description is [here][LINK9] “Azure Functions scale and hosting” and will therefore not recover it.  NOTE:  In the article, dedicated is often referred to as App Service plan.  In summary, when you run an Azure Function App, which can contain many Azure Functions in the dedicated plan, it is not ‘really’ server-less because you are running it in an App Service Plan which you have already created, this is useful, however, when your Azure Function App needs more than 1.5GB of memory for example, you can also enable AlwaysOn and avoid any startup latencies.  Additionally, on a dedicated plan you are not limited to 5 or 10 minute execution times.  Running an Azure Function App in consumption mode is truly server-less, meaning, after a given amount of time where the your Azure Function is not used, the VM will be de-configured and returned to the pool of available resources.  When the Azure Function needs to run again, the VM will come back on-line and run.  You pay only for the time in which the code it executing.  So super awesome!

# Getting Started
Create a Function App.  To complete the labs, you will need an Azure Function App.  It is assumed you already have an Azure Subscription and understand the concepts of Resource Groups, App Service Plans and the difference between a Dedicated versus Consumption based Azure Function App, oh…and the difference between an Azure Function App and an Azure Function would also be good to know…
## Creating an Azure Function App
Login to the Azure portal and click the +Create a resource link on the top left of the page.  Search for ‘Function App’ and then select the Function App, as seen in Figure 3.  There is also some nice information [here][LINK10] “Create your first function in the Azure portal”.

![create an Azure Function App][FIGURE3]

Enter the required details for creating the Azure Function App, similar to that seen in Figure 4.
The **App name** must be unique as it is a global, internet accessible endpoint.
For these labs I recommend you create a new **Resource Group**.  I recommend that because once you are finished with these labs, you know which resources you created while working them and can delete them all once you finish.
Windows **OS**, and chose a Consumption Plan, **Hosting Plan** based Azure Function App for the labs but I cannot think of a reason why any of the labs wouldn’t work in dedicated or premium mode.

![create an Azure Function App][FIGURE3]

For **Location**, pick the one closest to you or, in a real situation, close to your customers/users. Make sure both **Storage** and **Application Insights** are in the same region as **Location**.  I have created one **Storage** account for all the Azure Function Apps, I have not seen any negatives of doing that.  Again, be sure to have the storage in the same region as the Azure Function App for performance reasons.  **NOTE:**  AFAIK there is no logic in the portal that puts a lock on or link between the Azure Function App and the selected storage account.  If you, for some reason, perhaps when you are going through your subscription resources, looking for resources to delete, you remove the storage account, you destroy all the Azure Function you have built, there is no [recovery][LINK11].  This is why I have 1 storage account for all Azure Function Apps and I also give it a recognizable name…

Click the Create button to create the Azure Function App.  Done!  Once created, you are ready to begin the labs.


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
[LINK4]: https://blogs.msdn.microsoft.com/benjaminperkins/2018/08/07/why-does-my-azure-function-sometimes-stop-being-triggered/
[LINK5]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs
[LINK6]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-triggers-bindings#binding-at-runtime
[LINK7]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-dotnet-class-library#binding-at-runtime
[LINK8]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference-csharp#binding-at-runtime
[LINK9]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale
[LINK10]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-azure-function
[LINK11]: https://blogs.msdn.microsoft.com/benjaminperkins/2018/08/09/where-are-my-azure-functions-dissappeared
[LINK12]: https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services
[LINK13]: azure-functions-blob-storage.md
[LINK14]: azure-functions-blob-storage.md#lab-11
[LINK15]: azure-functions-azure-cosmos-db.md
[LINK16]: azure-functions-azure-cosmos-db.md#lab-21
[LINK17]: azure-functions-azure-cosmos-db.md#lab-22
[LINK18]: azure-functions-event-hub.md
[LINK19]: azure-functions-event-hub.md#lab-41
[LINK20]: azure-functions-http-trigger.md
[LINK21]: azure-functions-http-trigger.md#lab-71
[LINK22]: azure-functions-http-trigger.md#lab-72
[LINK23]: azure-functions-microsoft-graph.md
[LINK24]: azure-functions-storage-queue.md
[LINK25]: azure-function-service-bus.md
[LINK26]: azure-function-service-bus.md#lab-131
[LINK27]: azure-functions-timer.md
[LINK28]: azure-functions-timer.md#lab-161

[ANCHORLINK1]: azure-functions-labs-information-and-setup-instructions.md#getting-started

[FIGURE1]: ../images/azure-0002.png "Figure 1, create an Azure Function App, installing extensions"
[FIGURE2]: ../images/azure-0003.png "Figure 2, create an Azure Function App, configuring inputs and outputs"
[FIGURE]: ../images/azure-0004.png "Figure , inbound vs. outbound triggers]"
[FIGURE3]: ../images/azure-0005.png "Figure 3, create an Azure Function App]"
[FIGURE4]: ../images/azure-0006.png "Figure 4, create an Azure Function App]"
