# Azure Function – Service Bus

The labs contained in this article show how to create, configure, code and monitor an Azure Function with a Service Bus.  There is a detailed document [here][LINK1] “Azure Service Bus bindings for Azure Functions” which discusses the Service Bus trigger in detail so I will not readdress that content.  There is also a helpful article [here][LINK2] “Choose between Azure services that deliver messages” that discusses some difference between Event Grid ([x][LINK3]), Event Hub ([x][LINK4]) and Service Bus ([x][LINK5]).  As per that article, use a Service Bus when the following are essential.

| Purpose | Type | When to use |
| ------- | ---- | ----------- |
| High-value enterprise messaging | Message | Order processing and financial transactions |

I have written an AzureFunctionConsumer program which I host on GitHub [here][LINK6].  You can use this code to consume the Azure Functions discussed in [this][LINK7] series of articles.

## Create a Service Bus

***NOTE*** – to complete this lab you will need an Service Bus Namespace and a Service Bus Queue, follow [these][LINK8] instructions to create the namespace and queue.  For this lab, I have created something similar to that shown in Figure 1.

![Figure 1, how to create an Azure Function with a Service Bus trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with a Service Bus trigger

I.e. my Service Bus Namespace is ***csharpguitar-servicebus*** that include a queue named ***brain***.  Like Event Hub, the endpoint uses the sb:// protocol and has a hostname pattern containing x.servicebus.windows.net.

## Create a Service Bus triggered Azure Function

Add a new Function to the Function App by pressing the + sign next the the Functions menu item, as seen in Figure 2.

![Figure 2, how to create an Azure Function with a Service Bus trigger][FIGURE2]
###### Figure 2, how to create an Azure Function with a Service Bus trigger

Then find and select the Event Hub trigger template, as seen in Figure 3.

![Figure 3, how to create an Azure Function with a Service Bus trigger][FIGURE3]
###### Figure 3, how to create an Azure Function with a Service Bus trigger

Install the ***Microsoft.Azure.WebJobs.Extension.ServiceBus*** extension, if prompted.  Once installed, check that the following extension has been added to your extensions.csproj file.

```
<ItemGroup>
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.ServiceBus" Version="3.0.0" />
</ItemGroup>
```

Next, select the new link (see Figure 5) to create an Service Bus connection.  When the new link is selected, you will be prompted by a windows similar to that seen in Figure 4.  The Service Bus connection will contain the RootManageSharedAccessKey which is part of the Service Bus connection string and accessible via the “csharpguitar_RootManageSharedAccessKey_SERVICEBUS” application setting.

![Figure 4, how to create an Azure Function with a Service Bus trigger][FIGURE4]
###### Figure 4, how to create an Azure Function with a Service Bus trigger

Finally, similar as that seen in Figure 5, give the function a ***Name*** and add the the ***Queue Name*** which you created (review Figure 1).  Create the function by pressing the Create button.

![Figure 5, how to create an Azure Function with a Service Bus trigger][FIGURE5]
###### Figure 5, how to create an Azure Function with a Service Bus trigger

Once created you get the default Run() method shown here.

```
using System;
using System.Threading.Tasks;
 
public static void Run(string myQueueItem, ILogger log)
{
    log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

This article [here][LINK9] “Azure Service Bus bindings for Azure Functions” identify the metadata properties available to the Service Bus triggered Azure Function.  This metadata are the properties present within the Microsoft.ServiceBus.Messaging class described [here][LINK10]. See also Microsoft.Azure.Service.Message [here][LINK11] and Microsoft.Azure.WebJobs.ServiceBus.

## Lab 13

Using those properties metadata, update the function code to resemble the following.

```
using System;
using System.Threading.Tasks;
 
public static void Run(string myQueueItem, Int32 DeliveryCount, string DeadLetterSource, 
        DateTime ExpiresAtUtc, DateTime EnqueuedTimeUtc, string MessageId, 
        string ContentType, string ReplyTo, Int64 SequenceNumber, string To, 
        string Label, string CorrelationId, ILogger log)
{    
    try
    {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       log.LogInformation($"DeliveryCount: {DeliveryCount}");
       log.LogInformation($"DeadLetterSource: {DeadLetterSource}");
       log.LogInformation($"ExpiresAtUtc: {ExpiresAtUtc}");
       log.LogInformation($"EnqueuedTimeUtc: {EnqueuedTimeUtc}");
       log.LogInformation($"MessageId: {MessageId}");
       log.LogInformation($"ContentType: {ContentType}");
       log.LogInformation($"ReplyTo: {ReplyTo}");
       log.LogInformation($"SequenceNumber: {SequenceNumber}");
       log.LogInformation($"To: {To}");
       log.LogInformation($"Label: {Label}");
       log.LogInformation($"CorrelationId: {CorrelationId}");
    }
    catch (Exception ex)
    {
       log.LogInformation($"Something happened: {ex.Message}");
    }
}
```

Run the AzureFunctionsConsumer and select the following to add a message to a Service Bus Queue which will trigger the Azure Function, Figure 6.  You will find the value for “Enter your Service Bus connection string:” as the ***VALUE*** item for “csharpguitar-servicebus_RootManageSharedAccessKey_SERVICEBUS” (or the VALUE of the name which you gave it, review Figure 5). Note that the value for “Enter your Queue name” should equal the Queue name you set previously in Figure 5 as well.

![Figure 6, how to create an Azure Function with a Service Bus trigger][FIGURE6]
###### Figure 6, how to create an Azure Function with a Service Bus trigger

The output, when triggered, resembles something like the following.  You can also view the output via the Monitor blade for the given Function.

```
2018-10-18T12:17:34.188 [Information] 
  Executing 'Functions.ServiceBusQueueTrigger' 
  (Reason='New ServiceBus message detected on 'brain'.', Id=...ee1b)
2018-10-18T12:17:34.188 [Information] 
  C# ServiceBus queue trigger function processed message: Message-0-44307ab3e2d444b299589171fa9e2d88-17
2018-10-18T12:17:34.188 [Information] DeliveryCount: 1
2018-10-18T12:17:34.188 [Information] DeadLetterSource:
2018-10-18T12:17:34.188 [Information] ExpiresAtUtc: 11/1/2018 12:17:34 PM
2018-10-18T12:17:34.189 [Information] EnqueuedTimeUtc: 10/18/2018 12:17:34 PM
2018-10-18T12:17:34.189 [Information] MessageId: 6beec117be604aca850c19f7d035efd6
2018-10-18T12:17:34.189 [Information] ContentType:
2018-10-18T12:17:34.189 [Information] ReplyTo:
2018-10-18T12:17:34.189 [Information] SequenceNumber: 61361544922923034
2018-10-18T12:17:34.189 [Information] To:
2018-10-18T12:17:34.189 [Information] Label:
2018-10-18T12:17:34.189 [Information] CorrelationId:
2018-10-18T12:17:34.189 [Information] 
  Executed 'Functions.ServiceBusQueueTrigger1' (Succeeded, Id=...ee1b)
```

***NOTES***:
I noticed that I could use the same endpoint pattern with my Event Hub Azure Function as a used with my Service Bus endpoint.  Where the protocol was sb:// and the host name contained *.servicebus.windows.net.  meaning to me that they are both built on the same platform but each serve a different purpose.

## Lab 13.1

Before preceding with this lab make sure you have completed Lab 1, take note of the blob endpoint and container name created in Lab 1.  In this lab you will configure the Service Bus Queue trigger to store the message as a blob using the client SDK.

Update the Azure Function code so that it resemble the following.

```
#r "Microsoft.WindowsAzure.Storage"
 
using System;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
 
private static CloudBlobClient blobClient;
 
public static void Run(string myQueueItem, Int32 DeliveryCount, string DeadLetterSource, 
        DateTime ExpiresAtUtc, DateTime EnqueuedTimeUtc, string MessageId, 
        string ContentType, string ReplyTo, Int64 SequenceNumber, string To, 
        string Label, string CorrelationId, ILogger log)
{    
    try
    {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       log.LogInformation($"DeliveryCount: {DeliveryCount}");
       log.LogInformation($"DeadLetterSource: {DeadLetterSource}");
       log.LogInformation($"ExpiresAtUtc: {ExpiresAtUtc}");
       log.LogInformation($"EnqueuedTimeUtc: {EnqueuedTimeUtc}");
       log.LogInformation($"MessageId: {MessageId}");
       log.LogInformation($"ContentType: {ContentType}");
       log.LogInformation($"ReplyTo: {ReplyTo}");
       log.LogInformation($"SequenceNumber: {SequenceNumber}");
       log.LogInformation($"To: {To}");
       log.LogInformation($"Label: {Label}");
       log.LogInformation($"CorrelationId: {CorrelationId}");
 
       var blobConnectionString = Environment.GetEnvironmentVariable("AzureWebJobsStorage");
       log.LogInformation($"The blob connection string is: {blobConnectionString}");
       CloudStorageAccount storageAccount = CloudStorageAccount.Parse(blobConnectionString);
       blobClient = storageAccount.CreateCloudBlobClient();
       CloudBlobContainer blobContainer = blobClient.GetContainerReference("csharpguitar");
       log.LogInformation($"The blob name is: {myQueueItem}.txt");
       CloudBlockBlob blockBlob = blobContainer.GetBlockBlobReference($"{myQueueItem}.txt");
       log.LogInformation($"The blob content is: Hello, World! It is currently {EnqueuedTimeUtc}");
       blockBlob.UploadTextAsync($"Hello, World! It is currently {EnqueuedTimeUtc}");
       log.LogInformation($"The blob has been uploaded.");
    }
    catch (StorageException se)
    {
       log.LogInformation($"StorageException: {se.Message}");
    }
    catch (Exception ex)
    {
       log.LogInformation($"Something happened: {ex.Message}");
    }
}
```

Execute AzureFunctionsConsumer, same as before as seen in Figure 6, and review the results in the Logs.  Here you see that a message was sent to the the Service Bus Queue which in turn triggered the bound Azure Function.  You can also see the Information logs showing the name of the blob, it’s contents and the uploading of it.

```
2018-10-18T14:00:03.695 [Information] 
  Executing 'Functions.ServiceBusQueueTrigger' 
  (Reason='New ServiceBus message detected on 'brain'.', Id=..c7c)
2018-10-18T14:00:03.857 [Information] 
  C# ServiceBus queue trigger function processed message: Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59
2018-10-18T14:00:03.857 [Information] DeliveryCount: 1
2018-10-18T14:00:03.857 [Information] DeadLetterSource:
2018-10-18T14:00:03.857 [Information] ExpiresAtUtc: 11/1/2018 2:00:02 PM
2018-10-18T14:00:03.857 [Information] EnqueuedTimeUtc: 10/18/2018 2:00:02 PM
2018-10-18T14:00:03.858 [Information] MessageId: b7fbdf9f02224aca977583cccb24701b
2018-10-18T14:00:03.858 [Information] ContentType:
2018-10-18T14:00:03.858 [Information] ReplyTo:
2018-10-18T14:00:03.858 [Information] SequenceNumber: 20266198323167256
2018-10-18T14:00:03.858 [Information] To:
2018-10-18T14:00:03.858 [Information] Label:
2018-10-18T14:00:03.858 [Information] CorrelationId:
2018-10-18T14:00:03.858 [Information] The blob connection string is:...
2018-10-18T14:00:03.864 [Information] The blob name is: Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:03.864 [Information] The blob content is: Hello, World! It's currently 10/18/2018 2:00:02 PM
2018-10-18T14:00:03.865 [Information] The blob has been uploaded.
2018-10-18T14:00:03.867 [Information] Executed 'Functions.ServiceBusQueueTrigger1' (Succeeded, Id=...c7c)
```

You can also see that the blob was uploaded to the blob storage container “csharpguitar” which triggered the blob trigger function.  Notice the timestamps are similar, and file name is the same in both logs.

```
2018-10-18T14:00:07.302 [Information] 
  Executing 'Functions.BlobTrigger' 
  (Reason='New blob detected: csharpguitar/Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt', 
Id=..2f6a)
2018-10-18T14:00:07.316 [Information] 
  The name of the blob is: Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.316 [Information] 
  Name: AbsolutePath Value: /csharpguitar/Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.322 [Information] 
  Name: AbsoluteUri Value: https://.../csharpguitar/Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.322 [Information] 
  Name: LocalPath Value: /csharpguitar/Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.322 [Information] Name: Authority Value: ...
2018-10-18T14:00:07.322 [Information] Name: HostNameType Value: Dns
2018-10-18T14:00:07.322 [Information] Name: IsDefaultPort Value: True
2018-10-18T14:00:07.333 [Information] About to delete: Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.333 [Information] Deleting: Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.343 [Information] Deleted: Message-0-2a0d771f637f4e40b1b34e9b2f495bd4-59.txt
2018-10-18T14:00:07.343 [Information] Executed 'Functions.BlobTrigger' (Succeeded, Id=...2f6a)
```

Check the Monitor link and page to see similar output.

Have both Functions open in different tabs with the Log window open so you can see the logs, in real time during execution.

Congratulations, you have now configured, developed and consumed an Azure Function with an inbound Service Bus Queue trigger and coded the upload of a blob to a container which triggered another function.

![Figure 7, how to create an Azure Function with a Service Bus trigger][FIGURE7]
###### Figure 7, how to create an Azure Function with a Service Bus trigger

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus
[LINK2]: https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services
[LINK3]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-grid
[LINK4]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LINK5]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus
[LINK6]: https://github.com/benperk/AzureFunctionConsumer
[LINK7]: azure-functions-labs-information-and-setup-instructions.md
[LINK8]: https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dotnet-get-started-with-queues
[LINK9]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus#trigger---message-metadata
[LINK10]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet
[LINK11]: https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.servicebus.message?view=azure-dotnet

[FIGURE1]: ../images/2019/azure-0069.png "Figure 1, how to create an Azure Function with a Service Bus trigger"
[FIGURE2]: ../images/2019/azure-0070.png "Figure 2, how to create an Azure Function with a Service Bus trigger"
[FIGURE3]: ../images/2019/azure-0071.png "Figure 3, how to create an Azure Function with a Service Bus trigger"
[FIGURE4]: ../images/2019/azure-0072.png "Figure 4, how to create an Azure Function with a Service Bus trigger"
[FIGURE5]: ../images/2019/azure-0073.png "Figure 5, how to create an Azure Function with a Service Bus trigger"
[FIGURE6]: ../images/2019/azure-0074.png "Figure 6, how to create an Azure Function with a Service Bus trigger"
[FIGURE7]: ../images/2019/azure-0075.png "Figure 7, how to create an Azure Function with a Service Bus trigger"
