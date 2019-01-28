# Publish failed

While I was writing these articles about creating, developing local and deploying an Azure Function App, I received this error, Figure 1.

+ [How to create an Azure Function in Visual Studio][LINK1]
+ [How to connect to a database from an Azure Function][LINK2]
+ [Deploy an Azure Function created from Visual Studio][LINK3]
+ Check out all my Azure Functions articles [here][LINK4]

```
1>------ Build started: Project: chsharpguitar-func-db, Configuration: Release Any CPU ------
1>Function1.cs(37,81,37,82): error CS1010: Newline in constant
1>Function1.cs(37,82,37,82): error CS1026: ) expected
1>Function1.cs(37,82,37,82): error CS1002: ; expected
1>Function1.cs(43,75,43,76): error CS1010: Newline in constant
1>Function1.cs(43,76,43,76): error CS1026: ) expected
1>Function1.cs(43,76,43,76): error CS1002: ; expected
1>Done building project "chsharpguitar-func-db.csproj" -- FAILED.
========== Build: 0 succeeded, 1 failed, 0 up-to-date, 0 skipped ==========
  Publish Started
Function1.cs(37,81): error CS1010: Newline in constant [C:\Users\benperk\source\repos\chsharpguitar-func-db\
                     chsharpguitar-func-db\chsharpguitar-func-db.csproj]
Function1.cs(37,82): error CS1026: ) expected [C:\Users\benperk\source\repos\chsharpguitar-func-db\
                     chsharpguitar-func-db\chsharpguitar-func-db.csproj]
Function1.cs(37,82): error CS1002: ; expected [C:\Users\benperk\source\repos\chsharpguitar-func-db\
                     chsharpguitar-func-db\chsharpguitar-func-db.csproj]
Function1.cs(43,75): error CS1010: Newline in constant [C:\Users\benperk\source\repos\chsharpguitar-func-db\
                     chsharpguitar-func-db\chsharpguitar-func-db.csproj]
Function1.cs(43,76): error CS1026: ) expected [C:\Users\benperk\source\repos\chsharpguitar-func-db\
                     chsharpguitar-func-db\chsharpguitar-func-db.csproj]
```

![publishing failed while deploying to and Azure Function App][FIGURE1]
###### Figure 1, publishing failed while deploying to and Azure Function App

It turned out to be a compile issue, I had added some code without testing or compiling first.  I saw the failure reasons in the Output window in Visual Studio, Figure 2.

![publishing failed while deploying to and Azure Function App][FIGURE2]
###### Figure 2, publishing failed while deploying to and Azure Function App

Once I fixed those compile error, all worked fine.

[FIGURE1]: ../images/2018/msdn-0098.png "Figure 1, publishing failed while deploying to and Azure Function App"
[FIGURE2]: ../images/2018/msdn-0099.png "Figure 2, publishing failed while deploying to and Azure Function App"

[LINK1]: 2018/2018-04-how-to-create-an-azure-function-in-visual-studio.md
[LINK2]: 2018/2018-04-how-to-connect-to-a-database-from-an-azure-function.md
[LINK3]: 2018/2018-04-deploy-an-azure-function-created-from-visual-studio.md
[LINK4]: ../README.md#azure-functions
