# An error occurred

While I was writing these articles about creating, developing local and deploying an Azure Function App, I received this error, Figure 1.

+ [How to create an Azure Function in Visual Studio][LINK1]
+ [How to connect to a database from an Azure Function][LINK2]
+ [Deploy an Azure Function created from Visual Studio][LINK3]
+ Check out all my Azure Functions articles [here][LINK4]

```
Template deployment failed. Deployment operation statuses:
Failed: /subscriptions/*****/resourceGroups/PRODUCTION-AM2-001/providers/Microsoft.Storage/
storageAccounts/myazurefunctionsstorage ()   error (ResourceNotFound): The Resource 
'Microsoft.Storage/storageAccounts/****' under resource group 'PRODUCTION-AM2-001'
 was not found.  Succeeded: /subscriptions/####/resourceGroups/PRODUCTION-AM2-001
/providers/Microsoft.Web /sites/csharpguitarfuncdb ()
```

![An error occurred while deploying an Azure Function App][FIGURE1]
###### Figure 1, An error occurred while deploying an Azure Function App

This happened when I first attempted to deploy and simultaneously create the Azure Function App and the Azure Function.  I did not resolve it rather I worked around it.  Regardless of this error the Azure Function App was created, so I published to an existing Function App instead of creating new and it worked.

[LINK1]: 2018/2018-04-how-to-create-an-azure-function-in-visual-studio.md
[LINK2]: 2018/2018-04-how-to-connect-to-a-database-from-an-azure-function.md
[LINK3]: 2018/2018-04-deploy-an-azure-function-created-from-visual-studio.md
[LINK4]: ../README.md#azure-functions

[FIGURE1]: ../images/2018/msdn-0097.png "Figure 1, An error occurred while deploying an Azure Function App"
