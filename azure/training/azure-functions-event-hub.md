# Azure Functions – Event Hub

The labs contained in this article show how to create, configure, code and monitor an Azure Function with an Event Hub.  There is a detailed document [here][LINK1] “Azure Event Hubs bindings for Azure Functions” which discusses the Event Hub trigger in detail so I will not readdress that content.  There is also a helpful article [here][LINK2] “Choose between Azure services that deliver messages” that discusses some difference between Event Grid ([-][LINK3]), Event Hub ([-][LINK4]) and Service Bus ([-][LINK5]).  As per that article, use an Event Hub when the following are essential.

| Purpose | Type | When to use |
| ------- | ---- | ----------- |
| Big data pipeline | Event streaming (series) | Telemetry and distributed data streaming| 

I have written an AzureFunctionConsumer program which I host on GitHub [here][LINK6].  You can use this code to consume the Azure Functions discussed in [this][LINK7] series of articles.

## Create an Event Hub

***NOTE*** – to complete this lab you will need an Event Hub Namespace and an Event Hub, follow [these][LINK8] instructions to create the namespace and hub.  For this lab, I have created something similar to that shown in Figure 1.

![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger

I.e. my Event Hub Namespace is ***csharpguitar***, that includes a hub named ***brain***.  Like Service Bus, the endpoint uses the sb:// protocol and has a hostname pattern containing **.servicebus.windows.net.**

## Create an Event Hub triggered Azure Function

Add a new Function to the Function App by pressing the + sign next the the Functions menu item, as seen in Figure 2.

![Figure 2, how to create an Azure Function with an Event Hub trigger][FIGURE2]
###### Figure 2, how to create an Azure Function with an Event Hub trigger

Then find and select the Event Hub trigger template, as seen in Figure 3.

![Figure 3, how to create an Azure Function with an Event Hub trigger][FIGURE3]
###### Figure 3, how to create an Azure Function with an Event Hub trigger

Install the ***Microsoft.Azure.WebJobs.Extension.EventHubs*** extension, if prompted.  Once installed, check that the following extension has been added to your extensions.csproj file.

```
<ItemGroup>
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.EventHubs" Version="3.0.0" />
</ItemGroup>
```

Next, select the new link (see Figure 5) to create an Event Hub connection.  When the new link is selected, you will be prompted by a window similar to that seen in Figure 4.  The Event Hub connection will contain the RootManageSharedAccessKey which is part of the Event Hub connection string and accessible via the “csharpguitar_RootManageSharedAccessKey_EVENTHUB” application setting.

![Figure 4, how to create an Azure Function with an Event Hub trigger][FIGURE4]
###### Figure 4, how to create an Azure Function with an Event Hub trigger

Finally, similar as that seen in Figure 5, give the function a Name, leave Event Hub consumer group set to $Default and add the the Event Hub Name which you created (review Figure 1) and selected in Figure 4.

![Figure 5, how to create an Azure Function with an Event Hub trigger][FIGURE5]
###### Figure 5, how to create an Azure Function with an Event Hub trigger

Once created you get the default Run() method shown here.

```
using System;
 
public static void Run(string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

This article [here][LINK9] “Azure Event Hubs bindings for Azure Functions” identify the metadata properties available to the Event Hub triggered Azure Function.  This metadata are the properties present within the Microsoft.Azure.EventHubs class described [here][LINK10].

## Lab 4

Using those properties metadata, update the function code to resemble the following.

```
#r "Microsoft.Azure.EventHubs"
 
using System;
using System.Text;
using Microsoft.Azure.EventHubs;
 
public static void Run(EventData myEventHubMessage, ILogger log)
{
   log.LogInformation($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
   log.LogInformation($"Message = {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
   foreach(KeyValuePair<string, object> props in myEventHubMessage.Properties)
   {
      log.LogInformation($"Properties Key = {props.Key }");
      log.LogInformation($"Properties Value = {props.Value}");
   }
   log.LogInformation("System Properties EnqueuedTimeUtc = " +
       $"{myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
   log.LogInformation($"System Properties Offset = {myEventHubMessage.SystemProperties.Offset}");
   log.LogInformation($"System Properties PartitionKey = {myEventHubMessage.SystemProperties.PartitionKey}");
   log.LogInformation("System Properties SequenceNumber = " +
      $"{myEventHubMessage.SystemProperties.SequenceNumber}");
}
```

The items to note are:

1. I include a reference to the Microsoft.Azure.EventHubs assembly.
2. I changed the type of myEventHubMessage from string to EventData.

Run the AzureFunctionsConsumer and select the following to add a message to an Event Hub which will trigger the Azure Function, Figure 5.  You will find the value for “Enter your Event Hub connection string:” as the ***VALUE*** item for “csharpguitar_RootManageSharedAccessKey_EVENTHUB” (or the VALUE of the name which you gave it, review Figure 5). Note that the value for “Enter your Event Hub name” should equal the Event Hub name you set previously as well.

![Figure 6, how to create an Azure Function with an Event Hub trigger][FIGURE6]
###### Figure 6, how to create an Azure Function with an Event Hub trigger

Review the Event Hub and check the Messages metric and you will see the messages having been received, Figure 7.

![Figure 7, how to create an Azure Function with an Event Hub trigger][FIGURE7]
###### Figure 7, how to create an Azure Function with an Event Hub trigger

The Log output, when triggered, resembles something like the following.

```
2018 [Information] Executing 'Functions.EventHubTrigger' (Reason='', Id=0d-45cd-bb03-4daee348ab53)
2018-10-15T12:16:28.840  [Information] C# Event Hub trigger function processed a message: 
2018-10-15T12:16:28.840 [Information] Message = Message 0
2018-10-15T12:16:28.840 [Information] System Properties EnqueuedTimeUtc  = 10/15/2018 12:16:28 PM
2018-10-15T12:16:28.841 [Information] System Properties Offset  = 6064
2018-10-15T12:16:28.841 [Information] System Properties PartitionKey  =
2018-10-15T12:16:28.841 [Information] System Properties SequenceNumber  = 119
2018 [Information] Executed 'Functions.EventHubTrigger' (Succeeded, Id=-0d-45cd-bb03-4daee348ab53)
2018 [Information] Executing 'Functions.EventHubTrigger' (Reason='', Id=82-c680-40a7-9987-136b8ac59722)
2018-10-15T12:16:29.109 [Information] C# Event Hub trigger function processed a message:
2018-10-15T12:16:29.109 [Information] Message = Message 1
...
```

You can also view the output via the Monitor blade for the given Function, as seen in Figure 8.

![Figure 8, how to create an Azure Function with an Event Hub trigger][FIGURE8]
###### Figure 8, how to create an Azure Function with an Event Hub trigger

I decompiled the code using Just Decompile, like I often do to find what properties and methods were available, as seen in Figure 9.  I could have used Reflection, but decided to mix things up a little.

![Figure 9, how to create an Azure Function with an Event Hub trigger][FIGURE9]
###### Figure 9, how to create an Azure Function with an Event Hub trigger

You can find the Microsoft.Azure.EventHubs assembly via [KUDU/SCM][LINK11] in the /bin folder

Since version 2, adding the DLL manually for the extensions is no longer required, as I discussed [here][LINK12] “How to add assembly references to an Azure Function App” I added the implicit #r for a custom assembly, but this is no longer required, in most cases, for the extensions.

NOTES:

I noticed that I could use the same endpoint pattern with my Event Hub Azure Function as used with my Service Bus endpoint.  Where the protocol was sb:// and the host name contained .servicebus.windows.net.  meaning to me that they are both built on the same platform but each deliver a set of different capabilities.

## Lab 4.1

Before proceeding with this lab make sure you have completed Lab 1 and Lab 1.1.  The blob trigger in that lab (Lab 1) will insert a blob and then delete it (Lab 1.1).  In this lab, you will upload a blob into an Azure Storage container using the Microsoft.WindowsAzure.Storage CloudBlobClient class.

Note that the function.json file remains default, as that is where you would declaratively configure a inbound or outbound trigger.  In this case you use the client SDK for runtime outbound activity.

Update the Azure Function code so that it resembles the following.

```
#r "Microsoft.Azure.EventHubs"
#r "Microsoft.WindowsAzure.Storage"
 
using System;
using System.Text;
using Microsoft.Azure.EventHubs;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
 
private static CloudBlobClient blobClient;
 
public static void Run(EventData myEventHubMessage, ILogger log)
{
  log.LogInformation(
      $"C# Event Hub trigger function processed a message: {
        Encoding.UTF8.GetString(myEventHubMessage.Body)}");
  log.LogInformation($"Message = {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
  foreach(KeyValuePair<string, object> props in myEventHubMessage.Properties)
  {
    log.LogInformation($"Properties Key = {props.Key }");
    log.LogInformation($"Properties Value = {props.Value}");
  }
  log.LogInformation(
      $"System Properties EnqueuedTimeUtc = {
      myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
  log.LogInformation($"System Properties Offset = {myEventHubMessage.SystemProperties.Offset}");
  log.LogInformation($"System Properties PartitionKey = {
      myEventHubMessage.SystemProperties.PartitionKey}");
  log.LogInformation($
      "System Properties SequenceNumber = {myEventHubMessage.SystemProperties.SequenceNumber}");
  try
  {
    var blobConnectionString = Environment.GetEnvironmentVariable("AzureWebJobsStorage");
    log.LogInformation($"The blob connection string is: {blobConnectionString}");
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(blobConnectionString);
    blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer blobContainer = blobClient.GetContainerReference("csharpguitar");
    log.LogInformation($"The blob name is: {Encoding.UTF8.GetString(myEventHubMessage.Body)}.txt");
    CloudBlockBlob blockBlob = blobContainer.GetBlockBlobReference($
      "{Encoding.UTF8.GetString(myEventHubMessage.Body)}.txt");
    log.LogInformation($
      "The blob content is: Hello, World! It is currently {
      myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    blockBlob.UploadTextAsync($
      "Hello, World! It is currently {myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"The blob has been uploaded.");
  }
  catch (StorageException se)
  {
    log.LogInformation($"StorageException: {se.Message}");
  }
  catch (Exception ex)
  {
    log.LogInformation($"{DateTime.Now} > Exception: {ex.Message}");
  }    
}
```

Execute AzureFunctionsConsumer, same as before as seen in Figure 5, and review the results in the Logs.  Here you see that a message was sent to the Event Hub which in turn triggered the bound Azure Function.  You can also see the Information logs showing the name of the blob, it’s contents and the uploading of it.

```
2018 [Information] Executing 'Functions.EventHubTrigger' (Reason='', Id=...04af)
2018-10-15T13:34:53.164 [Information] C# Event Hub trigger function processed a message: Message-0
2018-10-15T13:34:53.164 [Information] Message = Message-0
2018-10-15T13:34:53.164 [Information] System Properties EnqueuedTimeUtc  = 10/15/2018 1:34:52 PM
2018-10-15T13:34:53.164 [Information] System Properties Offset  = 6800
2018-10-15T13:34:53.164 [Information] System Properties PartitionKey  =
2018-10-15T13:34:53.164 [Information] System Properties SequenceNumber  = 133
2018-10-15T13:34:53.164 [Information] The blob connection string is: DefaultEndpointsProtocol=...
2018-10-15T13:34:53.165 [Information] The blob name is: Message-0.txt
2018-10-15T13:34:53.165 [Information] The blob content is: Hello, World! It's currently 10/15/2018 1:34:52 PM
2018-10-15T13:34:53.165 [Information] The blob has been uploaded.
2018-10-15T13:34:53.166 [Information] Executed 'Functions.EventHubTrigger' (Succeeded, Id=...04af)
```

Click on the Monitor link for the Event Hub trigger and you will see the same, similar output, Figure 10.

![Figure 10, how to create an Azure Function with an Event Hub trigger][FIGURE10]
###### Figure 10, how to create an Azure Function with an Event Hub trigger

You can also see that the blob was uploaded to the blob storage container “csharpguitar” which triggered the blob trigger function.  Notice the timestamp and file name are the same, similar in both logs.

```
2018 [Information] 'Functions.BlobTrigger' (Reason='New blob detected: csharpguitar/Message-0.txt', Id=..3ca)
2018-10-15T13:34:55.861 [Information] The name of the blob is: Message-0.txt
2018-10-15T13:34:55.861 [Information] Name: AbsolutePath Value: /csharpguitar/Message-0.txt
2018-10-15T13:34:55 [Information] Name: AbsoluteUri Value: https:/...windows.net/csharpguitar/Message-0.txt
2018-10-15T13:34:55.861 [Information] Name: LocalPath Value: /csharpguitar/Message-0.txt
2018-10-15T13:34:55.861 [Information] Name: Authority Value: ....blob.core.windows.net
...
2018-10-15T13:34:55.864 [Information] About to delete: Message-0.txt
2018-10-15T13:34:55.869 [Information] Deleting: Message-0.txt
2018-10-15T13:34:55.870 [Information] Deleted: Message-0.txt
2018-10-15T13:34:55.870 [Information] Executed 'Functions.BlobTrigger' (Succeeded, Id=...3ca)
```

Check the Monitor link and page to see similar output, Figure 11.

![Figure 11, how to create an Azure Function with an Event Hub trigger][FIGURE11]
###### Figure 11, how to create an Azure Function with an Event Hub trigger

Congratulations, you have now configured, developed and consumed an Azure Function with an inbound Event Hub and coded the upload of a blob to a container which triggered another function.  The flow is like that shown in Figure 12.

![Figure 12, how to create an Azure Function with an Event Hub trigger][FIGURE12]
###### Figure 12, how to create an Azure Function with an Event Hub trigger

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LINK2]: https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services
[LINK3]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-grid
[LINK4]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LINK5]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus
[LINK6]: https://github.com/benperk/AzureFunctionConsumer
[LINK7]: azure-functions-labs-information-and-setup-instructions.md
[LINK8]: https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create
[LINK9]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs#trigger---event-metadata
[LINK10]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs
[LINK11]: https://blogs.msdn.microsoft.com/benjaminperkins/2014/03/24/using-kudu-with-windows-azure-web-sites/
[LINK12]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/04/13/how-to-add-assembly-references-to-an-azure-function-app/

[FIGURE1]: ../images/2019/azure-0031.png "Figure 1, how to create an Azure Function with an Event Hub trigger"
[FIGURE2]: ../images/2019/azure-0032.png "Figure 2, how to create an Azure Function with an Event Hub trigger"
[FIGURE3]: ../images/2019/azure-0033.png "Figure 3, how to create an Azure Function with an Event Hub trigger"
[FIGURE4]: ../images/2019/azure-0034.png "Figure 4, how to create an Azure Function with an Event Hub trigger"
[FIGURE5]: ../images/2019/azure-0035.png "Figure 5, how to create an Azure Function with an Event Hub trigger"
[FIGURE6]: ../images/2019/azure-0036.png "Figure 6, how to create an Azure Function with an Event Hub trigger"
[FIGURE7]: ../images/2019/azure-0037.png "Figure 7, how to create an Azure Function with an Event Hub trigger"
[FIGURE8]: ../images/2019/azure-0038.png "Figure 8, how to create an Azure Function with an Event Hub trigger"
[FIGURE9]: ../images/2019/azure-0039.png "Figure 9, how to create an Azure Function with an Event Hub trigger"
[FIGURE10]: ../images/2019/azure-0040.png "Figure 10, how to create an Azure Function with an Event Hub trigger"
[FIGURE11]: ../images/2019/azure-0041.png "Figure 11, how to create an Azure Function with an Event Hub trigger"
[FIGURE12]: ../images/2019/azure-0042.png "Figure 12, how to create an Azure Function with an Event Hub trigger"
