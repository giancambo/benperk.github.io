# Azure Functions – Timer

The labs contained in this article show how to create, configure, code and monitor an Azure Function with a Timer.  There is a detailed document [here][LINK1] “Timer trigger for Azure Functions” which discusses the Timer trigger in detail so I will not readdress that content.

I have written an AzureFunctionConsumer program which I host on GitHub [here][LINK2].  You can use this code to consume the Azure Functions discussed in [this][LINK3] series of articles.

As you will see later, the default Run() method for a Timer Trigger contains a parameter of type TimerInfo as defined [here][LINK4] on GitHub.  Looking at the TimerInfo class code on GitHub you can see the different methods and accessible public properties.  In addition to that, later I will use [Reflection][LINK5] to dump them out, just for some extra fun.

When you want an Azure Function to run at a certain time interval you can achieve that using a Timer trigger.   You might also consider using a WebJob, the underlying SDK is the same and if you already an App Service Plan ([x][LINK6]) then you might save some money.  Here are some articles which I wrote about WebJobs:

+ [How to setup CRON to run a WebJob on the Azure App Service platform][LINK7]
+ [Using the Azure WebJob API][LINK8]
+ [Helloworld WebJob on Microsoft Azure – Web App][LINK9]
+ All my WebJob articles [here][LINK10]
+ A GitHub troubleshooting article, [Investigating and reporting issues with timer triggered functions not firing][LINK11]

Add a new Function to the Function App by pressing the + sign next the the Functions menu item, as seen in Figure 1.

![Figure 1, how to create an Azure Function with an Timer trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Timer trigger

Then find and select the Timer trigger template, as seen in Figure 2.

![Figure 2, how to create an Azure Function with an Timer trigger][FIGURE2]
###### Figure 2, how to create an Azure Function with an Timer trigger

Then provide the Name and the CRON Trigger sequence as seen in Figure 3.  Here and here are some examples of CRON expressions.  In Figure 3, a C# function with the name of TimerTrigger will run every 2 minutes, or 30 times per hour, 720 times per day.

![Figure 3, how to create an Azure Function with an Timer trigger][FIGURE3]
###### Figure 3, how to create an Azure Function with an Timer trigger

Here is an example of the default Run() method.

```
using System;
 
public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

## Lab 16

Just for fun, find out using reflection which properties and methods exist within the TimerInfo class, update the Azure Function so that it resembles the following.

```
using System;
using System.Reflection;
 
public static void Run(TimerInfo myTimer, ILogger log)
{
   log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
   Type timerInfo = typeof(TimerInfo);
   log.LogInformation("TimerInfo Methods:");
   MethodInfo[] methodInfo = timerInfo.GetMethods();
   foreach (MethodInfo mInfo in methodInfo)
   {
      log.LogInformation($"{mInfo.ToString()}");
   }
   log.LogInformation("TimerInfo Members:");
   MemberInfo[] memberInfo = timerInfo.GetMembers();
   foreach (MemberInfo mbInfo in memberInfo)
   {
      log.LogInformation($"{mbInfo.ToString()}");
   }
   log.LogInformation($"Is past due: {myTimer.IsPastDue.ToString()}");
   log.LogInformation($"Schedule: {myTimer.Schedule.ToString()}");
   log.LogInformation($"Schedule Status Last: {myTimer.ScheduleStatus.Last.ToString()}");
   log.LogInformation($"Schedule Status Next: {myTimer.ScheduleStatus.Next.ToString()}");
   log.LogInformation($"Schedule Status LastUpdated: {myTimer.ScheduleStatus.LastUpdated.ToString()}");
}
```

The items to note here are:

1. I included the reference to System.Reflection.
2. I cast an instance of the TimerInfo class into the timerInfo object
3. Then I called the GetMethods() and GetMembers() methods and cycled through the results within a foreach statement

The output, when run from the portal looked something like the following, I would expect the same output when it is triggered.

```
[Information] Executing 'Functions.TimerTrigger' (Reason='Timer fired at 2018-10-16T12:51:59', Id=...4aa)
2018-10-16T12:49:35.848 [Information] C# Timer trigger function executed at: 10/16/2018 12:49:35 PM
2018-10-16T12:49:35.848 [Information] TimerInfo Methods:....
2018-10-16T12:49:35.849 [Information] TimerInfo Members:...
2018-10-16T12:49:35.850 [Information] Boolean IsPastDue
2018-10-16T12:49:35.850 [Information] Is past due: False
2018-10-16T12:49:35.850 [Information] Schedule: Cron: '0,2,4,6,8,10,12,14,16,18,20,...,32,34,... * * * *'
2018-10-16T12:49:35.850 [Information] Schedule Status Last: 10/16/2018 12:48:00 PM
2018-10-16T12:49:35.850 [Information] Schedule Status Next: 10/16/2018 12:50:00 PM
2018-10-16T12:49:35.850 [Information] Schedule Status LastUpdated: 10/16/2018 12:48:00 PM
2018-10-16T12:49:35.851 [Information] Executed 'Functions.TimerTrigger' (Succeeded, Id=...4aa)
```

You can also view the output via the Monitor blade for the given Function, as seen in Figure 4.

![Figure 4, how to create an Azure Function with an Timer trigger][FIGURE4]
###### Figure 4, how to create an Azure Function with an Timer trigger

Again, [here][LINK12] are some interesting troubleshooting notes for a timer triggered function –> “Investigating and reporting issues with timer triggered functions not firing”.

## Lab 16.1

Before completing this lab make sure you have completed Lab 1, Lab 1.1, Lab 4 and Lab 4.1.  The blob trigger in Lab 1 inserted a blob and then in Lab 1.1 it is deleted.  In Lab 4, you have created an Event Hub trigger that processes a message.  In Lab 4.1, you create a blob using the CloudBlockBlob class to create a blob using the contents of the Event Hub message and stored it in a storage container.  In this lab you will update the Timer function to send a message to the Event Hub which will send the message (as a blob) to the blob storage which will dump the details and delete it.  Similar to that shown in Figure 5.

![Figure 5, how to create an Azure Function with an Timer trigger][FIGURE5]
###### Figure 5, how to create an Azure Function with an Timer trigger

Update the Azure Function code so that it resemble the following.

```
#r "Microsoft.Azure.EventHubs"
 
using System;
using System.Reflection;
using System.Text;
 
using Microsoft.Azure.EventHubs;
 
private static EventHubClient eventHubClient;
 
public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    Type timerInfo = typeof(TimerInfo);
    log.LogInformation("TimerInfo Methods:");
    MethodInfo[] methodInfo = timerInfo.GetMethods();
    foreach (MethodInfo mInfo in methodInfo)
    {
       log.LogInformation($"{mInfo.ToString()}");
    }
    log.LogInformation("TimerInfo Members:");
    MemberInfo[] memberInfo = timerInfo.GetMembers();
    foreach (MemberInfo mbInfo in memberInfo)
    {
       log.LogInformation($"{mbInfo.ToString()}");
    }
    log.LogInformation($"Is past due: {myTimer.IsPastDue.ToString()}");
    log.LogInformation($"Schedule: {myTimer.Schedule.ToString()}");
    log.LogInformation($"Schedule Status Last: {myTimer.ScheduleStatus.Last.ToString()}");
    log.LogInformation($"Schedule Status Next: {myTimer.ScheduleStatus.Next.ToString()}");
    log.LogInformation($"Schedule Status LastUpdated: {myTimer.ScheduleStatus.LastUpdated.ToString()}");
    try
    {
       string eventHubConnectionString = Environment
         .GetEnvironmentVariable("csharpguitar_RootManageSharedAccessKey_EVENTHUB");
       log.LogInformation($"eventHubConnectionString: {eventHubConnectionString}");
       var connectionStringBuilder = new EventHubsConnectionStringBuilder(eventHubConnectionString)
       {
          EntityPath = "brain"
       };
       eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
       var date = DateTime.Now;
       var message = $"A-message-from-Timer-trigger-{date.Hour}-{date.Minute}";
       log.LogInformation($"Sending message: {message}");
       eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message))).Wait();
       eventHubClient.Close();
     }
     catch (EventHubsCommunicationException ehce)
     {
       log.LogInformation($"EventHubsCommunicationException: {ehce.Message}");
     }
     catch (EventHubsException ehe)
     {
       log.LogInformation($"EventHubsException: {ehe.Message}");
     }
     catch (Exception ex)
     {
       log.LogInformation($"{DateTime.Now} > Exception: {ex.Message}");
     }
}
```

Now wait for 2 minutes and you should then see the following output in the Logs window for the Timer trigger.

```
[Information] Executing 'Functions.TimerTrigger' (Reason='Timer fired at 2018-10-16T13:54', Id=...6eea)
2018-10-16T13:54:00.078 [Information] C# Timer trigger function executed at: 10/16/2018 1:54:00 PM
2018-10-16T13:54:00.078 [Information] TimerInfo Methods:...
2018-10-16T13:54:00.079 [Information] TimerInfo Members:...
2018-10-16T13:54:00.080 [Information] Boolean IsPastDue
2018-10-16T13:54:00.080 [Information] Is past due: False
2018-10-16T13:54:00.080 [Information] Schedule: Cron: '0 0,2,4,6,8,10,12,14,16,18,... * * * *'
2018-10-16T13:54:00.080 [Information] Schedule Status Last: 10/16/2018 1:52:00 PM
2018-10-16T13:54:00.081 [Information] Schedule Status Next: 10/16/2018 1:54:00 PM
2018-10-16T13:54:00.081 [Information] Schedule Status LastUpdated: 10/16/2018 1:52:00 PM
2018-10-16T13:54:00.081 [Information] eventHubConnectionString: 
    Endpoint=sb://....servicebus.windows.net/;
    SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=QhYa8=;EntityPath=brain
2018-10-16T13:54:00.081 [Information] Sending message: A-message-from-Timer-trigger-13-54
2018-10-16T13:54:03.514 [Information] Executed 'Functions.TimerTrigger' (Succeeded, Id=...6eea)
```

Something similar in the Logs window of the Event Hub trigger.

```
2018-10-16T13:54:03.515 [Information] Executing 'Functions.EventHubTrigger' (Reason='', Id=...d715)
2018-10-16T13:54:03.585 [Information] C# Event Hub trigger function 
                                      processed a message: A-message-from-Timer-trigger-13-54
2018-10-16T13:54:03.586 [Information] Message = A-message-from-Timer-trigger-13-54
2018-10-16T13:54:03.586 [Information] System Properties EnqueuedTimeUtc = 10/16/2018 1:54:02 PM
2018-10-16T13:54:03.586 [Information] System Properties Offset = 7472
2018-10-16T13:54:03.586 [Information] System Properties PartitionKey =
2018-10-16T13:54:03.586 [Information] System Properties SequenceNumber = 139
2018-10-16T13:54:03.586 [Information] The blob connection string is: ...
2018-10-16T13:54:03.586 [Information] The blob name is: A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:03.586 [Information] The blob content is: Hello, World! It's currently 10/16/2018 1:54:02PM
2018-10-16T13:54:03.586 [Information] The blob has been uploaded.
2018-10-16T13:54:03.587 [Information] Executed 'Functions.EventHubTrigger' (Succeeded, Id=...d715)
```

And finally, this in the Blob trigger Log window.

```
[Information] Executing 'Functions.BlobTrigger' 
             (Reason='New blob detected: csharpguitar/A-message-from-Timer-trigger-13-54.txt', Id=...aafe)
2018-10-16T13:54:13.686 [Information] The name of the blob is: A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.686 [Information] Name: 
              AbsolutePath Value: /csharpguitar/A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.686 [Information] Name: 
              AbsoluteUri Value: https://.../../A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.686 [Information] Name: 
              LocalPath Value: /csharpguitar/A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.688 [Information] About to delete: A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.688 [Information] Deleting: A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.694 [Information] Deleted: A-message-from-Timer-trigger-13-54.txt
2018-10-16T13:54:13.695 [Information] Executed 'Functions.BlobTrigger' (Succeeded, Id=...aafe)
```

And of course you can look at the graphical representation of the logs by clicking the Monitor link for each of the functions.

Congratulations, you have now configured, developed and consumed an Azure Function to run every 2 minutes that sends a message to an Event Hub which then stores the message as a blob, which triggers a blob Azure Function that dumps its properties and deletes it.

Finally, disable the timer so you do not consume the resources for no reason, see Figure 6.

![Figure 6, how to create an Azure Function with an Timer trigger][FIGURE6]
###### Figure 6, how to create an Azure Function with an Timer trigger

[FIGURE1]: ../images/2019/azure-0076.png "Figure 1, how to create an Azure Function with an Timer trigger"
[FIGURE2]: ../images/2019/azure-0077.png "Figure 2, how to create an Azure Function with an Timer trigger"
[FIGURE3]: ../images/2019/azure-0078.png "Figure 3, how to create an Azure Function with an Timer trigger"
[FIGURE4]: ../images/2019/azure-0079.png "Figure 4, how to create an Azure Function with an Timer trigger"
[FIGURE5]: ../images/2019/azure-0080.png "Figure 5, how to create an Azure Function with an Timer trigger"
[FIGURE6]: ../images/2019/azure-0081.png "Figure 6, how to create an Azure Function with an Timer trigger"

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-timer
[LINK2]: https://github.com/benperk/AzureFunctionConsumer
[LINK3]: azure-functions-labs-information-and-setup-instructions.md
[LINK4]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs
[LINK5]: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/reflection
[LINK6]: https://blogs.msdn.microsoft.com/benjaminperkins/2014/10/01/azure-website-hosting-plans-whp/
[LINK7]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/01/how-to-setup-cron-to-run-a-webjob-on-the-azure-app-service-platform/
[LINK8]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/02/01/using-the-azure-webjob-api/
[LINK9]: https://blogs.msdn.microsoft.com/benjaminperkins/2015/06/01/helloworld-webjob-on-microsoft-azure-web-app/
[LINK10]: https://blogs.msdn.microsoft.com/benjaminperkins/tag/webjob/
[LINK11]: https://github.com/Azure/azure-functions-host/wiki/Investigating-and-reporting-issues-with-timer-triggered-functions-not-firing
[LINK12]: https://github.com/Azure/azure-functions-host/wiki/Investigating-and-reporting-issues-with-timer-triggered-functions-not-firing
