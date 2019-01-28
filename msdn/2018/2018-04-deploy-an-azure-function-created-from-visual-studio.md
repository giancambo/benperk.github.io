# Deploy an Azure Function created from Visual Studio

I created an Azure Function and added some some code to connect to a database and now I want to deploy it to the Azure platform.

+ [How to create an Azure Function in Visual Studio][LINK1]
+ [How to connect to a database from an Azure Function][LINK2]
+ Check out all my Azure Functions articles [here][LINK3]

Right-click on the Project and select Publish, as seen in Figure 1.

![how to publish an Azure Function from Visual Studio, develop local][FIGURE1]
###### Figure 1, how to publish an Azure Function from Visual Studio, develop local

I chose to create a new Azure Function App, as you can see in Figure 2.

![how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio][FIGURE2]
###### Figure 2, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio

Then I filled out the details as seen in Figure 3.

![how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio][FIGURE3]
###### Figure 3, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio

And then selected the Create button.  This actually failed, see [here][LINK4]  “An error occurred”.   The Function App was created, but not the Function.  So I attempted to redeploy again by selecting the newly created Function App, see Figure 4 and all worked out fine.

![how to publish an Azure Function from Visual Studio, develop local, select existing, how to create Function App from Visual Studio][FIGURE4]
###### Figure 4, how to publish an Azure Function from Visual Studio, develop local, select existing, how to create Function App from Visual Studio

I selected the Azure Function App from Figure 5.

![how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio][FIGURE5]
###### Figure 5, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio

And all worked out just fine.  When I viewed the Azure Function App within the portal it looked like that seen in Figure 6.

![how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio][FIGURE6]
###### Figure 6, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio

As I mentioned [here][LINK1] “How to create an Azure Function in Visual Studio”, “This if fine, since I decided to create from Visual Studio, I will need to develop, test and publish in Visual Studio from this point on.  Do not think that you can go back and forth between developing in the portal and in Visual Studio.  It is either or and you need to decide how you want to develop.  When created from this direction, I see this, Figure 7 in the portal when I navigate to it.”

When I tested originally, I received the exception shown in Figure 7.

![how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio][FIGURE7]
###### Figure 7, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio

I resolved it by adding “DatabaseConnectionString” application setting as I discuss [here][LINK2] “How to connect to a database from an Azure Function”.  Issue was that I was reading from the local.settings.json file locally and the value didn’t exist when I published.
Once I added that, all was well, see Figure 8.

![how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio][FIGURE8]
###### Figure 8, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio

[LINK1]: 2018/2018-04-how-to-create-an-azure-function-in-visual-studio.md
[LINK2]: 2018/2018-04-how-to-connect-to-a-database-from-an-azure-function.md
[LINK3]: ../README.md#azure-functions
[LINK4]: 2018-04-an-error-occurred.md

[FIGURE1]: ../images/2018/msdn-0089.png "Figure 1, how to publish an Azure Function from Visual Studio, develop local"
[FIGURE2]: ../images/2018/msdn-0090.png "Figure 2, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio"
[FIGURE3]: ../images/2018/msdn-0091.png "Figure 3, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio"
[FIGURE4]: ../images/2018/msdn-0092.png "Figure 4, how to publish an Azure Function from Visual Studio, develop local, select existing, how to create Function App from Visual Studio"
[FIGURE5]: ../images/2018/msdn-0093.png "Figure 5, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio"
[FIGURE6]: ../images/2018/msdn-0094.png "Figure 6, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio"
[FIGURE7]: ../images/2018/msdn-0095.png "Figure 7, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio"
[FIGURE8]: ../images/2018/msdn-0096.png "Figure 8, how to publish an Azure Function from Visual Studio, develop local, how to create Function App from Visual Studio"
