# How to connect to a database from an Azure Function

I created a simple Azure Function and now I want to connect it to a database.  Check out these other articles as well.

+ [How to create an Azure Function in Visual Studio][LINK1]
+ [Deploy an Azure Function created from Visual Studio][LINK2]
+ Check out all my Azure Function articles [here][LINK3]

I found [this][LINK4] article also useful “Code and test Azure Functions locally”

I used ADO.NET to make this simple connection to a database.  The following is the code I used.

I included these namespaces:

```
using System;
using System.Data.SqlClient;
```

Keep in mind that this is only an example and there may be a few places to optimize the code pattern.  I am fine if you point them out, just for fun.

```
public static class Function1
{
  private static SqlConnection connection = new SqlConnection();

  [FunctionName("Function1")]
  public static async Task<HttpResponseMessage> 
      Run([HttpTrigger
        (AuthorizationLevel.Anonymous, 
          "get", "post", Route = null)]HttpRequestMessage req, TraceWriter log)
  {
      log.Info("C# HTTP trigger function processed a request.");
 
      try
      {
         connection.ConnectionString = 
             Environment.GetEnvironmentVariable("DatabaseConnectionString");
         await connection.OpenAsync();
         return req.CreateResponse(HttpStatusCode.OK, 
             $"The database connection is: {connection.State}");
      }
      catch (SqlException sqlex)
      {
         return req.CreateResponse(HttpStatusCode.BadRequest, 
             $"The following SqlException happened: {sqlex.Message}");
      }
      catch (Exception ex)
      {
         return req.CreateResponse(HttpStatusCode.BadRequest, 
             $"The following Exception happened: {ex.Message}");
      }
   }
}
```

While I was testing locally I wasn’t able to get the Environment.GetEnvironmentVariable() method to get the ConnectionStrings values contained in my local.settings.json file.  So I placed my connection string into the Values section and it worked fine.

When I executed the function locally using CURL I received the following successful result as seen in Figure 1.

![Azure Function App connecting to a database example][FIGURE1]
###### Figure 1, Azure Function App connecting to a database example

After I deployed my Function, see [here][LINK2] “Deploy an Azure Function created from Visual Studio” the code to read the information from the local.settings.json did not work any more.  I received this, Figure 2, when I attempted to execute the Azure Function.

``` "The following Exception happened: The ConnectionString property has not been initialized." ```

![Azure Function App connecting to a database example][FIGURE2]
###### Figure 2, Azure Function App connecting to a database example

To make this work, I simply added an Application Setting from my Azure Function App named “DatabaseConnectionString” with a value of the ADO.NET SQL connection string, Figure 3.

![Azure Function App connecting to a database example, cannot read from local.settings.json file][FIGURE3]
###### Figure 3, Azure Function App connecting to a database example, cannot read from local.settings.json file

While I was messing around with the Application Settings I got these errors, Figure 4.

```
"The following Exception happened: Keyword not supported: '\"server'."
"The following Exception happened: Format of the initialization string does 
  not conform to specification starting at index 0"
```

![Azure Function App connecting to a database example, cannot read from local.settings.json file][FIGURE4]
###### Figure 4, Azure Function App connecting to a database example, cannot read from local.settings.json file

The first one I resolved by Restarting the Azure App.  I didn’t get a chance to troubleshoot it more.  Or it was because I had the Application settings value wrapped in double quotes…?

The second one was because I fat fingered my connection string, once I got the correct value in there, all worked fine.

![Azure Function App connecting to a database example][FIGURE5]
###### Figure 5, Azure Function App connecting to a database example


[FIGURE1]: ../images/2018/msdn-0084.png "Figure 1, Azure Function App connecting to a database example"
[FIGURE2]: ../images/2018/msdn-0085.png "Figure 2, Azure Function App connecting to a database example"
[FIGURE3]: ../images/2018/msdn-0086.png "Figure 3, Azure Function App connecting to a database example, cannot read from local.settings.json file"
[FIGURE4]: ../images/2018/msdn-0087.png "Figure 4, Azure Function App connecting to a database example, cannot read from local.settings.json file"
[FIGURE5]: ../images/2018/msdn-0088.png "Figure 5, Azure Function App connecting to a database example"

[LINK1]: 2018-04-how-to-create-an-azure-function-in-visual-studio.md
[LINK2]: 2018-04-deploy-an-azure-function-created-from-visual-studio.md
[LINK3]: ../README.md#azure-functions
[LINK4]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local
