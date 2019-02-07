# Azure Functions – HTTP Trigger

The labs contained in this article show how to create, configure, code and monitor an Azure Function with an HTTP Trigger.  There is a detailed document [here][LINK1] “Azure Functions HTTP triggers and bindings” which discusses the HTTP Trigger in detail so I will not readdress that content.

I have written an AzureFunctionConsumer program which I host on GitHub [here][LINK2].  You can use this code to consume the Azure Functions discussed in [this][LINK3] series of articles.

For this lab, you will use ***Visual Studio*** to create, code and deploy the Azure Function HTTP trigger.

In order to create Azure Functions in Visual Studio (I am using VS2017 Community), you need to install the “Azure Functions and Web Jobs Tools” extension.  To achieve that, in Visual Studio select Tools –> Extensions and Updates… menu items and install it.  You will see something similar to the following, Figure 1

![Figure 1, how to create an Azure Function with an HTTP trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an HTTP trigger

Then, select New –> Project and Azure Function, as shown in Figure 2.

![Figure 2, how to create an Azure Function with an HTTP trigger][FIGURE2]
###### Figure 2, how to create an Azure Function with an HTTP trigger

Then select the HTTP Trigger template from the next window, Figure 3.

![Figure 3, how to create an Azure Function with an HTTP trigger][FIGURE3]
###### Figure 3, how to create an Azure Function with an HTTP trigger

Here is the default Run() method.

```
[FunctionName("Function1")]
public static async Task<IActionResult> 
Run([HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequest req, ILogger log)
{
   log.LogInformation("C# HTTP trigger function processed a request.");
   string name = req.Query["name"];
   string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
   dynamic data = JsonConvert.DeserializeObject(requestBody);
   name = name ?? data?.name;
   return name != null
      ? (ActionResult)new OkObjectResult($"Hello, {name}")
      : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

***NOTE***, when you publish the trigger with the above code, the function name will be that which is included in the FunctionName declaration, I.e. Function1.

***[FunctionName("Function1")]***

You can change that name to be more elegant.  I.e. change Function1 to “CoolHttpTrigger” or something…

## Lab 7

In this lab you will run and consume the HTTP triggered function on your local workstation.  Press the Run button or F5 to startup the Function Job Host.  When running you will see a window, similar to that shown in Figure 4.  This is the HOST which provides the access to the function.  It is what has to startup when you are running on the Azure platform.

![Figure 4, how to create an Azure Function with an HTTP trigger][FIGURE4]
###### Figure 4, how to create an Azure Function with an HTTP trigger

Figure 5 is a representation of the Job Host and its dependencies which get loaded into the HOST, which then contains the Function Assemblies, Bindings (ex: WindowsAzure.Storage) and the Function dependencies.

![Figure 5, how to create an Azure Function with an HTTP trigger][FIGURE5]
###### Figure 5, how to create an Azure Function with an HTTP trigger

Notice the URL that is in the red rectangle in Figure 4.  Open up a new cmd window and consume that URL using curl (or from a different browser instance/tab), as seen in Figure 6.

***curl http :// localhost:7071/api/HttpTrigger?name=Benjamin***

![Figure 6, how to create an Azure Function with an HTTP trigger][FIGURE6]
###### Figure 6, how to create an Azure Function with an HTTP trigger

As you can see from the default source code above, the trigger is expecting a parameter called ‘name’ and the code uses that value to send back a friendly Hello message.  Isn’t that nice, very polite, thank you.

## Lab 7.1

In this lab you will deploy the HTTP trigger to the Azure platform and consume it using the AzureFunctionConsumer.
Right-click on the project name and select Publish… as seen in Figure 7.

![Figure 7, how to create an Azure Function with an HTTP trigger][FIGURE7]
###### Figure 7, how to create an Azure Function with an HTTP trigger

Select the Create New radio button and then Publish, Figure 8.

![Figure 8, how to create an Azure Function with an HTTP trigger][FIGURE8]
###### Figure 8, how to create an Azure Function with an HTTP trigger

Then make the selections and press the Create button, Figure 9.

![Figure 9, how to create an Azure Function with an HTTP trigger][FIGURE9]
###### Figure 9, how to create an Azure Function with an HTTP trigger

Once successfully deployed, navigate to the Azure Portal (*) and click on the function you just deployed.  You will see something like the following.  Click on that link to get the URL and the function key which is necessary to consume or trigger the HTTP function.

***</> Get function URL***

Run the AzureFunctionsConsumer and select the following to consume / trigger the HTTP function, Figure 10.

![Figure 10, how to create an Azure Function with an HTTP trigger][FIGURE10]
###### Figure 10, how to create an Azure Function with an HTTP trigger

The output, when triggered, resembles something like the following.  You can also view the output via the Monitor blade for the given Function.

```
2018-10-17T11:05:06.362 [Information] Executing 'HttpTrigger' 
   (Reason='This function was programmatically called via the host APIs.', Id=...cf522)
2018-10-17T11:05:06.363 [Information] C# HTTP trigger function processed a request.
2018-10-17T11:05:06.414 [Information] Executed 'HttpTrigger' (Succeeded, Id=...cf522)
```

## Lab 7.2

In this lab you will send a message to the Queue Storage trigger created in Lab 11.  If you have not done that one yet, please do that one before you continue with this one…

+ Add the NuGet package:  WindowsAzure.Storage
+ Add the using keyword for: 
  + Microsoft.WindowsAzure.Storage
  + Microsoft.WindowsAzure.Storage.Queue
+ Update the code x2 to match your specific configuration (can you find them?)
+ Update the code of you HTTP trigger in Visual Studio as shown here.

```
[FunctionName("Function1")]
public static async Task<IActionResult>
Run([HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequest req, ILogger log)
{
   log.LogInformation("C# HTTP trigger function processed a request.");
   string name = req.Query["name"];
   string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
   dynamic data = JsonConvert.DeserializeObject(requestBody);
   name = name ?? data?.name;
   var message = $"Message-From-{name}-HTTP-Trigger-{DateTime.Now.Minute}";
 
   try
   {
      log.LogInformation($"Sending message: {message}");
      var StorageQueueConnectionString = Environment.GetEnvironmentVariable("AzureWebJobsStorage");
      CloudStorageAccount storageAccount = CloudStorageAccount.Parse(StorageQueueConnectionString);
      CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
      storageAccountQueue = queueClient.GetQueueReference("csharpguitar");
      await storageAccountQueue.AddMessageAsync(new CloudQueueMessage(message));
   }
   catch (StorageException se)
   {
      log.LogInformation($"StorageException: {se.Message}");
   }
   catch (Exception ex)
   {
      log.LogInformation($"{DateTime.Now} > Exception: {ex.Message}");
   }
 
   return message != null
      ? (ActionResult)new OkObjectResult($"This message, {message} was sent to the Message Queue")
      : new BadRequestObjectResult("Something bad happened, check the logs...");
}
```

Then republish the HTTP trigger as previously explained, I.e Figure 7.  If you get “Publishing failed.”, Figure 11.  Restart the function and retry.  You can view the details of the exception in the Output window in Visual Studio.

![Figure 1, how to create an Azure Function with an HTTP trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an HTTP trigger

Run the AzureFunctionsConsumer to consume / trigger the HTTP function, same as Figure 10, previously.  While running have the Logs console open for both functions in different browser windows.  You will see something similar to the following happening in close sequence, notice the timestamps…

```
2018-10-17T12:03:13 Welcome, you are now connected to log-streaming service.
2018-10-17T12:04:11.767 [Information] 
  Executing 'HttpTrigger' (Reason='This function was programmatically called via the host APIs.', Id=...fcf)
2018-10-17T12:04:12.010 [Information] C# HTTP trigger function processed a request.
2018-10-17T12:04:12.011 [Information] Sending message: Message-From-Benjamin-HTTP-Trigger-4
2018-10-17T12:04:12.413 [Information] Executed 'HttpTrigger' (Succeeded, Id=...fcf)
2018-10-17T12:04:43.188 [Information] 
  Executing 'Functions.QueueTrigger' (Reason='New queue message detected on 'csharpguitar'.', Id=...7204)
2018-10-17T12:04:43.189 [Information] AsString: Message-From-Benjamin-HTTP-Trigger-4
2018-10-17T12:04:43.189 [Information] DequeueCount: 1
2018-10-17T12:04:43.189 [Information] ExpirationTime: 10/24/2018 12:04:12 PM +00:00
2018-10-17T12:04:43.189 [Information] Queue message Id: 55cd4cfb-f9ce-4c02-92b6-dfcc6d7bb348
2018-10-17T12:04:43.189 [Information] InsertionTime: 10/17/2018 12:04:12 PM +00:00
2018-10-17T12:04:43.190 [Information] CurrentTime: 10/17/2018 12:04:43 PM
2018-10-17T12:04:43.190 [Information] MaxMessageSize: 65536
2018-10-17T12:04:43.190 [Information] MaxNumberOfMessagesToPeek: 32
2018-10-17T12:04:43.190 [Information] MaxVisibilityTimeout: 7.00:00:00
2018-10-17T12:04:43.190 [Information] NextVisibleTime: 10/17/2018 12:14:43 PM +00:00
2018-10-17T12:04:43.190 [Information] PopReceipt: AgAAAAMAAAAAAAAAk2V5/BJm1AE=
2018-10-17T12:04:43.190 [Information] Executed 'Functions.QueueTrigger' (Succeeded, Id=...7204)
```

Congratulations, you have now configured, coded, published and consumed an HTTP triggered Azure Function, using Visual Studio that sends the message to an Azure Storage Queue.

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook
[LINK2]: https://github.com/benperk/AzureFunctionConsumer
[LINK3]: azure-functions-labs-information-and-setup-instructions.md

[FIGURE1]: ../images/2019/azure-0043.png "Figure 1, how to create an Azure Function with an Blob trigger"
[FIGURE2]: ../images/2019/azure-0044.png "Figure 2, how to create an Azure Function with an Blob trigger"
[FIGURE3]: ../images/2019/azure-0045.png "Figure 3, how to create an Azure Function with an Blob trigger"
[FIGURE4]: ../images/2019/azure-0046.png "Figure 4, how to create an Azure Function with an Blob trigger"
[FIGURE5]: ../images/2019/azure-0047.png "Figure 5, how to create an Azure Function with an Blob trigger"
[FIGURE6]: ../images/2019/azure-0048.png "Figure 6, how to create an Azure Function with an Blob trigger"
[FIGURE7]: ../images/2019/azure-0049.png "Figure 7, how to create an Azure Function with an Blob trigger"
[FIGURE8]: ../images/2019/azure-0050.png "Figure 8, how to create an Azure Function with an Blob trigger"
[FIGURE9]: ../images/2019/azure-0051.png "Figure 9, how to create an Azure Function with an Blob trigger"
[FIGURE10]: ../images/2019/azure-0052.png "Figure 10, how to create an Azure Function with an Blob trigger"
[FIGURE11]: ../images/2019/azure-0053.png "Figure 11, how to create an Azure Function with an Blob trigger"
