# How to create an Azure Function in Visual Studio

[These][LINK1] are some good instructions as well “Create your first function using Visual Studio”.  But I like to try it out myself.  Check out these other articles as well.

+ [How to connect to a database from an Azure Function][LINK2]
+ [Deploy an Azure Function created from Visual Studio][LINK3]
+ Check out all my Azure Function articles [here][LINK4]

First, open Visual Studio and create the Azure Function, similar to that shown in Figure 1.

![how to create an Azure Function using Visual Studio][FIGURE1]
###### Figure 1, how to create an Azure Function using Visual Studio

Select the trigger type, I chose the below configuration, I.e. Http Trigger as seen in Figure 2.

![how to create an Azure Function using Visual Studio][FIGURE2]
###### Figure 2, how to create an Azure Function using Visual Studio

Without making any changes, I pressed F5 and was then asked to install the Azure Functions CLI tools, Figure 3 and I did that.

![how to create an Azure Function using Visual Studio][FIGURE3]
###### Figure 3, how to create an Azure Function using Visual Studio

The simulator started up, as seen in Figure 4.

![how to create an Azure Function using Visual Studio][FIGURE4]
###### Figure 4, how to create an Azure Function using Visual Studio

Once the CLI tools were installed, the CMD window, seen in Figure 5 opened up and provided me the URL to test the Azure Function.

![how to test local an Azure Function using Visual Stu][FIGURE5]
###### Figure 5, how to test local an Azure Function using Visual Stu

Then I used CURL from another CMD window to call and test the Azure Function, see Figure 6.

![how to test local an Azure Function using Visual Studio][FIGURE6]
###### Figure 6, how to test local an Azure Function using Visual Studio

That was too easy, Azure Functions are pretty cool.
Be sure to check out my other Azure Function articles [here][LINK4].

After I published my Function, I had some problems:

+ [An error occurred][LINK5]
+ [Publish failed][LINK6]

I resolved them, but the thing is that when I access the Function in the portal I have limited means of updating there in the portal itself.  This if fine, since I decided to create from Visual Studio, I will need to develop, test and publish in Visual Studio from this point on.  Do not think that you can go back and forth between developing in the portal and in Visual Studio.  It is either or and you need to decide how you want to develop.  When created from this direction, I see this, Figure 7 in the portal when I navigate to it.

![how to test local an Azure Function using Visual Studio, azure portal][FIGURE7]
###### Figure 7, how to test local an Azure Function using Visual Studio, azure portal

You can consider creating the Azure Function App and Function, then downloading the project and doing some developing and testing in Visual Studio, then cut & paste the changes from Visual Studio into the Azure Function IDE in the portal.

[FIGURE1]: ../images/2018/msdn-0077.png "Figure 1, how to create an Azure Function using Visual Studio"
[FIGURE2]: ../images/2018/msdn-0078.png "Figure 2, how to create an Azure Function using Visual Studio"
[FIGURE3]: ../images/2018/msdn-0079.png "Figure 3, how to create an Azure Function using Visual Studio"
[FIGURE4]: ../images/2018/msdn-0080.png "Figure 4, how to create an Azure Function using Visual Studio"
[FIGURE5]: ../images/2018/msdn-0081.png "Figure 5, how to test local an Azure Function using Visual Studio"
[FIGURE6]: ../images/2018/msdn-0082.png "Figure 6, how to test local an Azure Function using Visual Studio"
[FIGURE7]: ../images/2018/msdn-0083.png "Figure 7, how to test local an Azure Function using Visual Studio, azure portal"

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-your-first-function-visual-studio
[LINK2]: ../README.md#azure-functions
[LINK3]: ../README.md#azure-functions
[LINK4]: ../README.md#azure-functions
[LINK5]: ../README.md#azure-functions
[LINK6]: ../README.md#azure-functions
