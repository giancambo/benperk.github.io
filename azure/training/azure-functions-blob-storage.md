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


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function


![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
###### Figure 1, how to modify host.json for an Azure Function



[FIGURE1]: images/2019/azure-0007.png "Figure 1, how to create an Azure Function with an Blob trigger"
[FIGURE2]: images/2019/azure-0008.png "Figure 2, how to create an Azure Function with an Blob trigger"
[FIGURE3]: images/2019/azure-0009.png "Figure 3, how to create an Azure Function with an Blob trigger"
[FIGURE4]: images/2019/azure-0010.png "Figure 4, how to create an Azure Function with an Blob trigger"
[FIGURE5]: images/2019/azure-0011.png "Figure 5, how to modify host.json for an Azure Function"
[FIGURE6]: images/2019/azure-0012.png "Figure 6, how to modify host.json for an Azure Function"
[FIGURE7]: images/2019/azure-0013.png "Figure 7, how to modify host.json for an Azure Function"
[FIGURE8]: images/2019/azure-0014.png "Figure 8, how to modify host.json for an Azure Function"
[FIGURE9]: images/2019/azure-0015.png "Figure 9, how to modify host.json for an Azure Function"
[FIGURE10]: images/2019/azure-0016.png "Figure 10, how to modify host.json for an Azure Function"
[FIGURE11]: images/2019/azure-0017.png "Figure 10, how to modify host.json for an Azure Function"
