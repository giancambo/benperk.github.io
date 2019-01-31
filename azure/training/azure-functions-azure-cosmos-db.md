# Azure Functions – Azure Cosmos DB

The labs contained in this article show how to create, configure, code and monitor an Azure Function with an Azure Cosmos DB.  There is a detailed document [here][LINK1] “Azure Cosmos DB bindings for Azure Functions 2.x” which discusses the Cosmos DB trigger in detail so I will not readdress that content.

I have written an AzureFunctionConsumer program which I host on GitHub [here][LINK2].  You can use this code to consume the Azure Functions discussed in [this][LINK3] series of articles.

When you want an Azure Function to execute when a document is added to a specified Cosmos DB this lab will get you started.  Additionally, here are 2 articles which are useful to get some more insight into this feature:

+ [Azure Cosmos DB: .NET examples for the SQL API][LINK4]
+ [azure-cosmosdb-dotnet][LINK5]

## Create a Cosmos Database

***NOTE*** – to complete this lab you will need an Azure Cosmos DB account, a Cosmos DB and a Collection.  Follow [these][LINK6] instructions to create the account, database and collection.  For this lab, I have created something similar to that shown in Figure 1.

![Figure 1, how to create an Azure Function with a Cosmos DB trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with a Cosmos DB trigger

Where ***csharpguitar*** is the Cosmos DB account, ***ToDoList*** is the database and ***Items*** is the collection.

## Create an Azure Cosmos DB triggered Azure Function

Add a new Function to the Function App by pressing the + sign next the the Functions menu item, as seen in Figure 2.

![Figure 2, how to create an Azure Function with a Cosmos DB trigger][FIGURE2]
###### Figure 2, how to create an Azure Function with a Cosmos DB trigger

Then find and select the Cosmos DB trigger template, as seen in Figure 3.

![Figure 3, how to create an Azure Function with a Cosmos DB trigger][FIGURE3]
###### Figure 3, how to create an Azure Function with a Cosmos DB trigger

Install the Microsoft.Azure.WebJobs.Extensions.CosmosDB extension, if prompted.  Once installed, check that the following extension has been added to your extensions.csproj file.

```
<ItemGroup>
  <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.CosmosDB" Version="3.0.0" />
</ItemGroup>
```

Next, select the new link (see Figure 5) to create an Azure Cosmos DB connection.  When the new link is selected, you will be prompted by a windows similar to that seen in Figure 4.  You can view the Cosmos DB connection string on the Cosmos DB / Keys blade.  Once the select button is press the Cosmos DB connection string is stored in an Application Setting named for ex: csharpguitar_DOCUMENTDB.

![Figure 4, how to create an Azure Function with a Cosmos DB trigger][FIGURE4]
###### Figure 4, how to create an Azure Function with a Cosmos DB trigger

Finally, similar as that seen in Figure 5, give the function a ***Name***, add the ***Collection name*** as created in Figure 1, add the ***Database*** name also as created in Figure 1, then press the Create button to complete the function creation.

![Figure 5, how to create an Azure Function with a Cosmos DB trigger][FIGURE5]
###### Figure 5, how to create an Azure Function with a Cosmos DB trigger

Once created you get the default Run() method shown here.

```
#r "Microsoft.Azure.DocumentDB.Core"
using System;
using System.Collections.Generic;
using Microsoft.Azure.Documents;
 
public static void Run(IReadOnlyList<Document> input, ILogger log)
{
    if (input != null && input.Count > 0)
    {
        log.LogInformation("Documents modified " + input.Count);
        log.LogInformation("First document Id " + input[0].Id);
    }
}
```

Notice that there is a variable of IReadOnlyList<Microsoft.Azure.Documents.Document> passed into the Run().  Let's find out what properties that Document object exposes.

## Lab 2

Update the function code to resemble the following.

```
#r "Microsoft.Azure.DocumentDB.Core"
using System;
using System.Collections.Generic;
using Microsoft.Azure.Documents;
 
public static void Run(IReadOnlyList<Document> input, ILogger log)
{
    try
    {
        if (input != null && input.Count > 0)
        {
            var result = input.ToList();
            log.LogInformation($"Documents modified {input.Count}");
            foreach (var document in result)
            {
                log.LogInformation($"DocumentId {document.Id}");
                log.LogInformation($"AltLink: {document.AltLink}");
                log.LogInformation($"AttachmentsLink {document.AttachmentsLink}");
                log.LogInformation($"ETag {document.ETag}");
                log.LogInformation($"ResourceId {document.ResourceId}");
                log.LogInformation($"SelfLink {document.SelfLink}");
                log.LogInformation($"Timestamp {document.Timestamp}");
                log.LogInformation($"TimeToLive {document.TimeToLive}");
            }
        } 
    }
    catch (Exception ex)
    {
        log.LogInformation($"Something Happened: {ex.Message}");
    }       
}
```

The items to note here are:

1. I originally included the reference to System.Reflection (Type and PropertyInfo classes), however the Document class has set the GetProperty() to private…  However, see Figure 6 where the Document class does expose GetMetaObject() method which will return return the properties rendered in the above code example.
2. The IReadOnlyList<> interface does not include an IEnummerable interface, so it gets casted to a List which can then be iterated through within a foreach loop.

![Figure 6, how to create an Azure Function with a Cosmos DB trigger][FIGURE6]
###### Figure 6, how to create an Azure Function with a Cosmos DB trigger

Run the AzureFunctionsConsumer and select the following to upload documents which will trigger the Azure Function, Figure 7.  You will find the value for ***database*** name and ***collection*** name via the values you entered when creating your Cosmos DB, see Figure 1.  The ***endpoint url*** name is found on the Overview blade for the Cosmos DB account (URI).  The ***account key*** is found on the Keys blade for the given Cosmos DB account.

![Figure 7, how to create an Azure Function with a Cosmos DB trigger][FIGURE7]
###### Figure 7, how to create an Azure Function with a Cosmos DB trigger

Check the Documents item on the Cosmos DB blade to see that the documents where indeed uploaded and view the contents of them, Figure 8.

![Figure 8, how to create an Azure Function with a Cosmos DB trigger][FIGURE8]
###### Figure 8, how to create an Azure Function with a Cosmos DB trigger

The Log output, when triggered, resembles something like the following.  You can also view the output via the Monitor blade for the given Function.

```
2018-10-26T09:11:04.684 [Information] Executing 'Functions.CosmosTrigger' 
   (Reason='New changes on collection Items at 2018-10-26T09:11:04.6842733Z', Id=...78aa)
2018-10-26T09:11:05.015 [Information] Documents modified 4
2018-10-26T09:11:05.016 [Information] DocumentId 0
2018-10-26T09:11:05.016 [Information] AltLink: dbs/ToDoList/colls/Items/docs/0
2018-10-26T09:11:05.043 [Information] 
   AttachmentsLink dbs/BBcXAA==/colls/BBcXAOfHQgA=/docs/BBcXAOfHQgAlAAAAAAAAAA==/attachments/
2018-10-26T09:11:05.044 [Information] ETag "0000c30a-0000-0000-0000-5bd2da240000"
2018-10-26T09:11:05.044 [Information] ResourceId BBcXAOfHQgAlAAAAAAAAAA==
2018-10-26T09:11:05.044 [Information] 
   SelfLink dbs/BBcXAA==/colls/BBcXAOfHQgA=/docs/BBcXAOfHQgAlAAAAAAAAAA==/
2018-10-26T09:11:05.044 [Information] Timestamp 10/26/2018 9:11:00 AM
2018-10-26T09:11:05.044 [Information] TimeToLive
2018-10-26T09:11:05.044 [Information] DocumentId 1
2018-10-26T09:11:05.044 [Information] AltLink: dbs/ToDoList/colls/Items/docs/1
...
2018-10-26T09:11:05.045 [Information] DocumentId 2
2018-10-26T09:11:05.045 [Information] AltLink: dbs/ToDoList/colls/Items/docs/2
...
2018-10-26T09:11:05.045 [Information] DocumentId 3
2018-10-26T09:11:05.045 [Information] AltLink: dbs/ToDoList/colls/Items/docs/3
...
2018-10-26T09:11:05.050 [Information] Executed 'Functions.CosmosTrigger' 
   (Succeeded, Id=...78aa)
```

The Monitor blade for the given Function, as seen in Figure 9, notice that there is a single log for all the documents uploaded during the connection.

![Figure 9, how to create an Azure Function with a Cosmos DB trigger][FIGURE9]
###### Figure 9, how to create an Azure Function with a Cosmos DB trigger


![Figure 1, how to create an Azure Function with a Cosmos DB trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with a Cosmos DB trigger


![Figure 1, how to create an Azure Function with a Cosmos DB trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with a Cosmos DB trigger


![Figure 1, how to create an Azure Function with a Cosmos DB trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with a Cosmos DB trigger


[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb-v2
[LINK2]: https://github.com/benperk/AzureFunctionConsumer
[LINK3]: https://benperk.github.io/azure/training/azure-functions-labs-information-and-setup-instructions.html
[LINK4]: https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-dotnet-samples
[LINK5]: https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs
[LINK6]: https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet

[FIGURE1]: ../images/2019/azure-0019.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0020.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0021.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0022.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0023.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0024.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0025.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0026.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0027.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0028.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0029.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE1]: ../images/2019/azure-0030.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
