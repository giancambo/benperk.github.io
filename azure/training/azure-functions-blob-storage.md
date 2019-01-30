# Azure Functions – Blob Storage

The labs contained in this article show how to create, configure, code and monitor an Azure Function with a Blob Trigger.  There is a detailed document here “Azure Blob storage bindings for Azure Functions” which discusses the Blob storage trigger in detail so I will not readdress that content.
I have written an AzureFunctionConsumer program which I host on GitHub here.  You can use this code to consume the Azure Functions discussed in this series of articles.

When you want an Azure Function to execute when a blob is added to a specified storage container use this trigger type which is discussed in this lab.  (use this binding type for small working sets that are not time critical)   Here are some articles which I wrote about WebJobs/Blob storage:

+ How (I) back up my Azure Blob Storage container
+ How to map an Azure App Service Web App virtual directory to Azure Storage Container
+ Upload an image to an Azure Blob container using the Gadgeteer
+ System.Diagnostics.Trace Application Logging Log Stream on Azure App Service Function App
+ How to store and access your IIS Logs on Microsoft Azure App Service
+ How to Backup/Restore an Azure App Service using Azure PowerShell

##Create a Storage Account

***NOTE*** – to complete this lab you will need a Storage Account.

By default, a storage account is created when the Azure Function is created, as seen in Figure 1.  In a real, production scenario the blob container would likely not be the same as the one which stores the Function code.

![Figure 1, how to create an Azure Function with an Blob trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Blob trigger

If you would like to store the blobs into a different account, use this set of instruction to create an Azure Storage Account.  For this lab, you do not need to create the storage container as the code in the AzureFunctionConsumer will do this for you later, but you can if you want.

## Create a Blob triggered Azure Function

Add a new Function to the Function App by pressing the + sign next the the Functions menu item, as seen in Figure 2.

![Figure 2, how to create an Azure Function with an Blob trigger][FIGURE2]
###### Figure 2, how to create an Azure Function with an Blob trigger

Then find and select the Blob trigger template, as seen in Figure 3.

![Figure 3, how to create an Azure Function with an Blob trigger][FIGURE3]
###### Figure 3, how to create an Azure Function with an Blob trigger

Install the Microsoft.Azure.WebJobs.Extension.Storage extension, if prompted.  Once installed, check that the following extension has been added to your extensions.csproj file.

```
<ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.0" />
</ItemGroup>
```

Once complete, continue to the next step.

***TIP:*** If you ever need to install an extension manually, add the extension to the extensions.csproj file and run the following command in KUDU/SCM.

``` dotnet build extensions.csproj -o bin --no-incremental --packages D:\home\.nuget ```

Next, give the New Function a ***Name***, add the ***Path*** where the first attribute is the storage container name (can already exists or it will be created via the AzureFunctionConsumer) and leave {name} ‘as-is’.  {name} will the be the name of the blob stored in the container.  The storage account connection is, as titled, the App Setting Name that contains the connection string for the storage account which contains or will contain the storage container added in the Path.  See Figure 4.  If you want this to be stored in a different container click on new and configure it.  Otherwise, press the ***Create*** button.

![Figure 4, how to create an Azure Function with an Blob trigger][FIGURE4]
###### Figure 4, how to create an Azure Function with an Blob trigger

Here is the default Run() method.

```
public static void Run(Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob Name:{name} Size: {myBlob.Length} Bytes");
}
```

As mentioned in this article here “Azure Blob storage bindings for Azure Functions” the metadata properties available to the Azure Function are those present in the Microsoft.WindowsAzure.Storage.Blob class described here.  In addition, there are ***BlobTrigger*** (same as ***URI***.***AbsolutePath***), Uri, Properties BlobProperties/CloudBlob(*/*), and user-defined ***Metadata*** properties which are also accessible.  Lastly, a System.IO.***Stream*** (see here) object is passed into the Run() which also provides some interesting information.

## Lab 1

Using those properties metadata, update the function code to resemble the following.

```
using System.Collections;
using System.Reflection;
 
public static void Run(Stream myBlob, string name, Uri uri,
                       IDictionary<string,string> metadata, ILogger log)
{
    log.LogInformation($"The name of the blob is: {name}");
    Type uriType = typeof(Uri);
    PropertyInfo[] properties = uriType.GetProperties();
    foreach (PropertyInfo uriProp in properties)
    {
        log.LogInformation($"Name: {uriProp.Name} Value: {uriProp.GetValue(uri, null)}");
    }
    foreach(KeyValuePair<string, string> data in metadata)
    {
        log.LogInformation($"User-Defined Metadata Key  = { data.Key  }");
        log.LogInformation($"User-Defined Metadata Value  = { data.Value  }");
    }
    if (metadata.Count == 0) 
    {
        log.LogInformation("There were no user-defined metadata");
    }
    log.LogInformation($"System.IO.Stream.CanRead is: {myBlob.CanRead}");
    log.LogInformation($"System.IO.Stream.CanSeek is: {myBlob.CanSeek}");
    log.LogInformation($"System.IO.Stream.CanTimeout is: {myBlob.CanTimeout}");
    log.LogInformation($"System.IO.Stream.CanWrite is: {myBlob.CanWrite}");
    log.LogInformation($"System.IO.Stream.Length is: {myBlob.Length}");
    log.LogInformation($"System.IO.Stream.Position is: {myBlob.Position}");
}
```

The items to note here are:

1. I included the reference to System.Reflection (Type and PropertyInfo classes) and System.Collections (KeyValuePair structure).
2. I used reflection to loop through the properties and values of the Uri class.

Run the AzureFunctionsConsumer and select the following to upload blobs which will trigger the Azure Function, Figure 5.  You will find the value for “Enter your blob Storage connection string:” as the VALUE item for “AzureWebJobsStorage” app setting used when creating the function. Note that the value for “Enter the Blob Container name:” should equal the Path you set previously (excluding the {name}), see Figure 4.

![Figure 5, how to create an Azure Function with an Blob trigger][FIGURE5]
###### Figure 5, how to create an Azure Function with an Blob trigger

Once executed, Navigate to the container that you configured to store the blobs, and you will see something similar to the following, Figure 6.

![Figure 6, how to create an Azure Function with an Blob trigger][FIGURE6]
###### Figure 6, how to create an Azure Function with an Blob trigger

The Log output, when triggered, resembles something like the following.  You can also view the output via the Monitor blade for the given Function.

![how to create an Azure Function with an Blob trigger][FIGURE7]

The Monitor blade for the given Function, as seen in Figure 7.

![Figure 7, how to create an Azure Function with an Blob trigger][FIGURE8]
###### Figure 7, how to create an Azure Function with an Blob trigger

NOTES:

Both queues and blobs can be stored in the same Azure Storage Account.  Within the account you can create a blob container, a queue, a file share and a table, for example.

From a blob perspective, the function is notified when one is added and the location and how to access is sent to the function allowing processing.  Unlike the Queue, where the message is deleted after processing, the blob is not deleted when the Azure Function is triggered.

The default contents of the function.json file.  Click on View files to see the function.json file in the portal.

```
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "csharpguitar/{name}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

You have now configured, created and consumed an inbound request to an Azure Function, next, configure an outbound action in response to the inbound request.

## Lab 1.1

In this lab you will configure an outbound action to delete the inserted blob that was inserted into the blob container configured in Lab 1.

To achieve this, select the Integrate link under the function created in Lab 1 and press the + New Output option, as seen in Figure 8.

![Figure 8, how to create an Azure Function with an Blob trigger][FIGURE9]
###### Figure 8, how to create an Azure Function with an Blob trigger

ress the select button and options similar to that in Figure 9, are rendered.  Note the Blob parameter name and update the Path as shown…leave the Storage account connection as the default, assuming you made no change in Lab 1.

![Figure 9, how to create an Azure Function with an Blob trigger][FIGURE10]
###### Figure 9, how to create an Azure Function with an Blob trigger

Once saved you will see the output binding in the interface, similar to that shown in Figure 10.

![Figure 10, how to create an Azure Function with an Blob trigger][FIGURE11]
###### Figure 10, how to create an Azure Function with an Blob trigger

The functions.json file gets updated to include the output binding, similar to that shown below.

```
{
  "bindings": [
  {
    "name": "myBlob",
    "type": "blobTrigger",
    "direction": "in",
    "path": "csharpguitar/{name}",
    "connection": "AzureWebJobsStorage"
  },
  {
    "type": "blob",
    "name": "outputBlob",
    "path": "csharpguitar/{name}",
    "connection": "AzureWebJobsStorage",
    "direction": "out"
  }
 ]
}
```

Update the Azure Function code so that it resemble the following.

```
#r "Microsoft.WindowsAzure.Storage"
 
using System.Collections;
using System.Reflection;
using Microsoft.WindowsAzure.Storage.Blob; 
public static void Run(Stream myBlob, string name, Uri uri, CloudBlockBlob outputBlob,
           IDictionary<string,string> metadata, ILogger log)
{
     log.LogInformation($"The name of the blob is: {name}");
     Type uriType = typeof(Uri);
     PropertyInfo[] properties = uriType.GetProperties();
     foreach (PropertyInfo uriProp in properties)
     {
       log.LogInformation($"Name: {uriProp.Name} Value: {uriProp.GetValue(uri, null)}");
     }
     foreach(KeyValuePair<string, string> data in metadata)
     {
       log.LogInformation($"User-Defined Metadata Key  = { data.Key  }");
       log.LogInformation($"User-Defined Metadata Value  = { data.Value  }");
     }
     if (metadata.Count == 0)
     {
       log.LogInformation("There were no user-defined metadata");
     }
     log.LogInformation($"System.IO.Stream.CanRead is: {myBlob.CanRead}");
     log.LogInformation($"System.IO.Stream.CanSeek is: {myBlob.CanSeek}");
     log.LogInformation($"System.IO.Stream.CanTimeout is: {myBlob.CanTimeout}");
     log.LogInformation($"System.IO.Stream.CanWrite is: {myBlob.CanWrite}");
     log.LogInformation($"System.IO.Stream.Length is: {myBlob.Length}");
     log.LogInformation($"System.IO.Stream.Position is: {myBlob.Position}");
      
     log.LogInformation($"About to delete: {name}");
     log.LogInformation($"Deleting: {name}");
     outputBlob.DeleteAsync();
     log.LogInformation($"Deleted: {name}");
}
```

Notice the inclusion of the Microsoft.WindowsAzure.Storage / .Blob binary and reference.  This is required for the utilization of the CloudBlockBlob passed to the Run() method, which is used at the end of the method to access the DeleteAsync() method which removes the blob from the storage container.  Notice that the variable ‘outputBlob” in the parameters for the Run() method, matches the name of the output parameter of name in the function.json file.

Execute AzureFunctionsConsumer, same as before as seen in Figure 5, and review the results in the Logs.

```
2018-10-12T14:43:51.723 [Information] About to delete: helloworld3.txt
2018-10-12T14:43:51.723 [Information] Deleting: helloworld3.txt
2018-10-12T14:43:51.729 [Information] Deleted: helloworld3.txt
```
And also, after some moments via the Monitor link for the function, see Figure 11

![Figure 11, how to create an Azure Function with an Blob trigger][FIGURE12]
###### Figure 11, how to create an Azure Function with an Blob trigger

[FIGURE1]: ../images/2019/azure-0007.png "Figure 1, how to create an Azure Function with an Blob trigger"
[FIGURE2]: ../images/2019/azure-0008.png "Figure 2, how to create an Azure Function with an Blob trigger"
[FIGURE3]: ../images/2019/azure-0009.png "Figure 3, how to create an Azure Function with an Blob trigger"
[FIGURE4]: ../images/2019/azure-0010.png "Figure 4, how to create an Azure Function with an Blob trigger"
[FIGURE5]: ../images/2019/azure-0011.png "Figure 5, how to create an Azure Function with an Blob trigger"
[FIGURE6]: ../images/2019/azure-0012.png "Figure 6, how to create an Azure Function with an Blob trigger"
[FIGURE7]: ../images/2019/azure-0013.png "Figure 7, how to create an Azure Function with an Blob trigger"
[FIGURE8]: ../images/2019/azure-0014.png "Figure 8, how to create an Azure Function with an Blob trigger"
[FIGURE9]: ../images/2019/azure-0015.png "Figure 9, how to create an Azure Function with an Blob trigger"
[FIGURE10]: ../images/2019/azure-0016.png "Figure 10, how to create an Azure Function with an Blob trigger"
[FIGURE11]: ../images/2019/azure-0017.png "Figure 11, how to create an Azure Function with an Blob trigger"
[FIGURE12]: ../images/2019/azure-0018.png "Figure 12, how to create an Azure Function with an Blob trigger"
