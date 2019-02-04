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

You have now configured, created and consumed an inbound request to an Azure Function, next, configure an outbound action in response to the inbound request.

## Lab 2.1

In this lab you will update the Cosmos DB function trigger to select a document from the database, convert it to JSON and then display it to the Log console.

```
#r "Microsoft.Azure.DocumentDB.Core"
#r "Newtonsoft.Json"
using System;
using System.Collections.Generic;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
 
private static DocumentClient client;
 
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
        var endpointUrl = "<ADD URL>";
        var authorizationKey = "<PRIMARY KEY>";
        var databaseId = "ToDoList";
        var collectionId = "Items";
 
        client = new DocumentClient(new Uri(endpointUrl), authorizationKey);
        Database database = client.CreateDatabaseQuery()
            .Where(c => c.Id == databaseId).ToArray().FirstOrDefault();
        DocumentCollection collection = client.CreateDocumentCollectionQuery(database.SelfLink)
            .Where(c => c.Id == collectionId).ToArray().FirstOrDefault();
        Order order = client.CreateDocumentQuery<Order>(collection.SelfLink)
             .Where(o => o.Id == "0").AsEnumerable().FirstOrDefault();
        if (order != null)
        {
            log.LogInformation($"Order AccountNumber: {order.AccountNumber}");
            var output = JsonConvert.SerializeObject(order);
            log.LogInformation($"JSON: {output}");
        }        
    }
    catch (DocumentClientException de)
    {
        Exception baseException = de.GetBaseException();
        log.LogInformation($"{de.StatusCode} error: {de.Message}, Message: {baseException.Message}");
    }
    catch (Exception ex)
    {
        log.LogInformation($"Something Happened: {ex.Message}");
    }       
}
 
public class Order
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public DateTime CreateDate { get; set; }
    public string AccountNumber { get; set; }
    public decimal Freight { get; set; }
    public decimal TotalDue { get; set; }
    public OrderDetail[] Items { get; set; }
}
 
public class OrderDetail
{
    public int OrderQty { get; set; }
    public int ProductId { get; set; }
    public decimal UnitPrice { get; set; }
}
```

The items to note here are:

1. I included an assembly reference for both ***Microsoft.Azure.DocumentDB.Core*** and ***Newtonsoft.Json***.  Then I included them in my project via the using keyword.
2. Next using ***Microsoft.Azure.Documents.Client*** the client is instantiated, the query created and the result Serialized.

Run the AzureFunctionsConsumer and select the same as previously, Figure 7.  This time, upload only a single (1) document.

The output of the Logs console in the portal will look something similar to the following.

```
2018-10-29T10:42:23.801 [Information] 
   Executing 'Functions.CosmosTrigger' 
   (Reason='New changes on collection Items at 2018-10-29T10:42:23.8003475Z', Id=...d25b)
2018-10-29T10:42:24.364 [Information] Documents modified 1
2018-10-29T10:42:24.370 [Information] DocumentId 0
2018-10-29T10:42:24.370 [Information] AltLink: dbs/ToDoList/colls/Items/docs/0
2018-10-29T10:42:24.371 [Information] AttachmentsLink
2018-10-29T10:42:24.371 [Information] ETag "280038c4-0000-0000-0000-5bd6e40c0000"
2018-10-29T10:42:24.371 [Information] ResourceId BBcXAOfHQgBSAAAAAAAAAA==
2018-10-29T10:42:24.371 [Information] SelfLink dbs/BBcXAA==/colls/BB..gA=/docs/BBcXA...AAAAAAAA==/
2018-10-29T10:42:24.371 [Information] Timestamp 10/29/2018 10:42:20 AM
2018-10-29T10:42:24.371 [Information] TimeToLive
2018-10-29T10:42:25.241 [Information] Order AccountNumber: Account0
2018-10-29T10:42:25.243 [Information] 
JSON: {
        "id":"0",
        "CreateDate":"2018-10-29T10:42:20.3381465+00:00",
        "AccountNumber":"Account0",
        "Freight":472.3108,
        "TotalDue":985.018,
        "Items":[
        {
          "OrderQty":0,
          "ProductId":0,
          "UnitPrice":419.4589}
]}
2018-10-29T10:42:25.243 [Information] Executed 'Functions.CosmosTrigger' (Succeeded, Id=...d25b)
```

There are many actions take with a JSON file…try to come up with some ideas about how you can enhance this example or send the JSON body to another system, ask yourself also if this is the best architectural design for the solution.  Nice work, keep going.

## Lab 2.2

To complete this lab, you need to first have completed Lab 1 and Lab 1.1.  In this lab you will:

+ Retrieve the uploaded document from the Cosmos DB
+ Declaratively configure an outbound action to store that file into the blob storage container configured in Lab 1.

To achieve this, select the Integrate link under the function created in Lab 2 and press the + New Output option, as seen in Figure 10.

![Figure 10, how to create an Azure Function with a Cosmos DB trigger][FIGURE10]
###### Figure 10, how to create an Azure Function with a Cosmos DB trigger

Press the select button and options similar to that in Figure 11, are rendered.  Note the Blob parameter name and update the Path as shown…leave the Storage account connection as the default, assuming you made no change in Lab 1.

![Figure 11, how to create an Azure Function with a Cosmos DB trigger][FIGURE11]
###### Figure 11, how to create an Azure Function with a Cosmos DB trigger

Save the outbound trigger.  Once save completes you will see the output binding in the interface, similiar to that shown in Figure 12.

![Figure 12, how to create an Azure Function with a Cosmos DB trigger][FIGURE12]
###### Figure 12, how to create an Azure Function with a Cosmos DB trigger

The functions.json file gets updated to include the output binding, similiar to that shown below.

```
{
   "bindings": [
   {
     "type": "cosmosDBTrigger",
     "name": "input",
     "direction": "in",
     "connectionStringSetting": "csharpguitar_DOCUMENTDB",
     "databaseName": "ToDoList",
     "collectionName": "Items",
     "leaseCollectionName": "leases",
     "createLeaseCollectionIfNotExists": true
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
#r "Microsoft.Azure.DocumentDB.Core"
#r "Newtonsoft.Json"
 
using System;
using System.Collections.Generic;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Microsoft.WindowsAzure.Storage.Blob;
using Newtonsoft.Json;
 
private static DocumentClient client;
 
public static void Run(IReadOnlyList<Document> input, CloudBlockBlob outputBlob, ILogger log)
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
        var endpointUrl = "<URL>/";
        var authorizationKey = "<PRIMARY ACCESS KEY>";
        var databaseId = "ToDoList";
        var collectionId = "Items";
 
        client = new DocumentClient(new Uri(endpointUrl), authorizationKey);
        Database database = client.CreateDatabaseQuery()
            .Where(c => c.Id == databaseId).ToArray().FirstOrDefault();
        DocumentCollection collection = client.CreateDocumentCollectionQuery(database.SelfLink)
            .Where(c => c.Id == collectionId).ToArray().FirstOrDefault();
        Order order = client.CreateDocumentQuery<Order>(collection.SelfLink)
            .Where(o => o.Id == "0").AsEnumerable().FirstOrDefault();
        if (order != null)
        {
            log.LogInformation($"Order AccountNumber: {order.AccountNumber}");
            log.LogInformation($"Order: {order.ToString()}");
            log.LogInformation($"About to insert: {order.AccountNumber}");
            log.LogInformation($"Inserting: {order.AccountNumber}");
            outputBlob.UploadTextAsync(order.AccountNumber);
            log.LogInformation($"Inserted: {order.AccountNumber}");
        }        
    }
    catch (DocumentClientException de)
    {
        Exception baseException = de.GetBaseException();
        log.LogInformation($"{de.StatusCode} error: {de.Message}, Message: {baseException.Message}");
    }
    catch (Exception ex)
    {
        log.LogInformation($"Something Happened: {ex.Message}");
    }       
}
 
public class Order
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public DateTime CreateDate { get; set; }
    public string AccountNumber { get; set; }
    public decimal Freight { get; set; }
    public decimal TotalDue { get; set; }
    public OrderDetail[] Items { get; set; }
}
 
public class OrderDetail
{
    public int OrderQty { get; set; }
    public int ProductId { get; set; }
    public decimal UnitPrice { get; set; }
}
```

In Lab 1.1 you added a DeleteAsync() method to the Azure Function, so consider removing that temporarily so you can look at the blob in the container.

***Congratulations***, you have again declaratively configured, developed and consumed an Azure Function with an inbound and outbound trigger.

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-cosmosdb-v2
[LINK2]: https://github.com/benperk/AzureFunctionConsumer
[LINK3]: azure-functions-labs-information-and-setup-instructions.md
[LINK4]: https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-dotnet-samples
[LINK5]: https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs
[LINK6]: https://docs.microsoft.com/en-us/azure/cosmos-db/create-sql-api-dotnet

[FIGURE1]: ../images/2019/azure-0019.png "Figure 1, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE2]: ../images/2019/azure-0020.png "Figure 2, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE3]: ../images/2019/azure-0021.png "Figure 3, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE4]: ../images/2019/azure-0022.png "Figure 4, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE5]: ../images/2019/azure-0023.png "Figure 5, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE6]: ../images/2019/azure-0024.png "Figure 6, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE7]: ../images/2019/azure-0025.png "Figure 7, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE8]: ../images/2019/azure-0026.png "Figure 8, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE9]: ../images/2019/azure-0027.png "Figure 9, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE10]: ../images/2019/azure-0028.png "Figure 10, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE11]: ../images/2019/azure-0029.png "Figure 11, how to create an Azure Function with a Cosmos DB trigger"
[FIGURE12]: ../images/2019/azure-0030.png "Figure 12, how to create an Azure Function with a Cosmos DB trigger"
