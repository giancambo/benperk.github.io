
# Azure Functions – Microsoft Graph

The labs contained in this article show how to create, configure, code and monitor an Azure Function with a Microsoft Graph.  There is a detailed document [here][LINK1] “Microsoft Graph bindings for Azure Functions” which discusses the Microsoft Graph in detail so I will not readdress that content.

## Azure Active Directory Tenant

Every Azure subscription will have an associated Azure Active Directory (AAD) tenant, it has the pattern similar to x.onmicrosoft.com.  Where 'x' is the unique identifier created with your subscription.  There are other ways of getting an AAD tenant, like via O365, but that is outside the scope of this lab.

In order to complete this lab you will need to be authorized to Grant consent to permissions.  I.e. the first time you are authenticated, after configuring the Azure Function, you get prompted with a pop-up similar to that shown here, Figure 1.

![Figure 1, how to create an Azure Function with Microsoft Graph][FIGURE1]
###### Figure 1, how to create an Azure Function with Microsoft Graph

If you do not have this permission, after clicking the ‘Consent on behalf of your organization’ and pressing the Accept button, you will get the following error.

```
AADSTS90094: An administrator of has set a policy that prevents you from granting "app name" the permissions 
it is requesting. Contact an administrator.
```

You can check if your identity has the permission in the portal looking at the API permissions of the Application which you will create later.  If the ‘Grant admin consent for xxxx’ button is enabled, it means you have the permission, if not, well, you don’t, Figure 2, and you will need to be granted permission.

![Figure 2, how to create an Azure Function with Microsoft Graph][FIGURE2]
###### Figure 2, how to create an Azure Function with Microsoft Graph

## Create an Azure Function for access to Microsoft Graph on behalf of

[FIGURE1]: ../images/2019/azure-0054.png "Figure 1, how to create an Azure Function with Microsoft Graph"
[FIGURE2]: ../images/2019/azure-0055.png "Figure 2, how to create an Azure Function with Microsoft Graph"
[FIGURE3]: ../images/2019/azure-0056.png "Figure 3, how to create an Azure Function with Microsoft Graph"
[FIGURE4]: ../images/2019/azure-0057.png "Figure 4, how to create an Azure Function with Microsoft Graph"
[FIGURE5]: ../images/2019/azure-0058.png "Figure 5, how to create an Azure Function with Microsoft Graph"
[FIGURE6]: ../images/2019/azure-0059.png "Figure 6, how to create an Azure Function with Microsoft Graph"
[FIGURE7]: ../images/2019/azure-0060.png "Figure 7, how to create an Azure Function with Microsoft Graph"
[FIGURE8]: ../images/2019/azure-0061.png "Figure 8, how to create an Azure Function with Microsoft Graph"
[FIGURE9]: ../images/2019/azure-0061.png "Figure 9, how to create an Azure Function with Microsoft Graph"

[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-microsoft-graph
