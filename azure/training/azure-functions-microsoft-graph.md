
# Azure Functions – Microsoft Graph

The labs contained in this article show how to create, configure, code and monitor an Azure Function with a Microsoft Graph.  There is a detailed document [here][LINK1] “Microsoft Graph bindings for Azure Functions” which discusses the Microsoft Graph in detail so I will not readdress that content.

## Azure Active Directory Tenant

Every Azure subscription will have an associated Azure Active Directory (AAD) tenant, it has the pattern similar to *.onmicrosoft.com.  Where * is the unique identifier created with your subscription.  There are other ways of getting an AAD tenant, like via O365, but that is outside the scope of this lab.

In order to complete this lab you will need to be authorized to Grant consent to permissions.  I.e. the first time you are authenticated, after configuring the Azure Function, you get prompted with a pop-up similar to that shown here, Figure 1.

![Figure 1, how to create an Azure Function with Microsoft Graph][FIGURE1]
###### Figure 1, how to create an Azure Function with Microsoft Graph

If you do not have this permission, after clicking the ‘Consent on behalf of your organization’ and pressing the Accept button, you will get the following error.

```
AADSTS90094: An administrator of has set a policy that prevents you from granting "app name" the permissions 
it is requesting. Contact an administrator.
```

You can check if your identity has the permission in the portal looking at the API permissions of the Application which you will create later.  If the ‘Grant admin consent for *****’ button is enabled, it means you have the permission, if not, well, you don’t, Figure 2, and you will need to be granted permission.

![Figure 2, how to create an Azure Function with Microsoft Graph][FIGURE2]
###### Figure 2, how to create an Azure Function with Microsoft Graph

## Create an Azure Function for access to Microsoft Graph on behalf of

Add a new Function to the Function App by pressing the + sign next the the Functions menu item, as seen in Figure 3.

![Figure 3, how to create an Azure Function with Microsoft Graph][FIGURE3]
###### Figure 3, how to create an Azure Function with Microsoft Graph

Then find and select the Microsoft Graph template, as seen in Figure 4.

![Figure 4, how to create an Azure Function with Microsoft Graph][FIGURE4]
###### Figure 4, how to create an Azure Function with Microsoft Graph

Install the ***Microsoft.Azure.WebJobs.Extension.AuthTokens*** extension, if prompted.  Once installed, check that the following extension has been added to your extensions.csproj file.

```
<ItemGroup>
  <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.AuthTokens" Version="3.0.0" />
</ItemGroup>
```

Next, select the **Configure AAD** now link (see Figure 5) to setup EasyAuth and set the permissions for this application in the tenant.

![Figure 5, how to create an Azure Function with Microsoft Graph][FIGURE5]
###### Figure 5, how to create an Azure Function with Microsoft Graph

Once the automated configuration is complete, press the Create button to create the Function, Figure 6.

![Figure 6, how to create an Azure Function with Microsoft Graph][FIGURE6]
###### Figure 6, how to create an Azure Function with Microsoft Graph

The configuration create an app registration into the AAD with a name similar to the Azure Function App name, Figure 7.

![Figure 7, how to create an Azure Function with Microsoft Graph][FIGURE7]
###### Figure 7, how to create an Azure Function with Microsoft Graph

Here is the default Run() method.

```
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers;
 
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, ILogger log)
{
   log.LogInformation("C# HTTP trigger function processed a request."); 
   HttpClient client = new HttpClient();
   client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
   return await client.GetAsync("https://graph.microsoft.com/v1.0/me/photo/$value");
}

Make sure that you require AAD authentication “Log In with Azure Active Directory” for the function via the Authentication / Authorization link found on the Platform tab for the Azure Function App, Figure 8.  By default, it is set to allow anonymous.

![Figure 8, how to create an Azure Function with Microsoft Graph][FIGURE8]
###### Figure 8, how to create an Azure Function with Microsoft Graph

I received a 500 when the authentication allowed anonymous.

![Figure 9, how to create an Azure Function with Microsoft Graph][FIGURE9]
###### Figure 9, how to create an Azure Function with Microsoft Graph

If you receive this error when you access the Function from a browser:

```
{
   "error": {
     "code": "OrganizationFromTenantGuidNotFound",
     "message": "The tenant for tenant guid '####' does not exist.",
     "innerError": {
        "request-id": "####",
        "date": "2018-10-30T10:55:55"
     }
   }
}
```

Then you need to manually update the additionalLoginParams using resource explorer here.  The Application Id is the one shown previously in Figure 7, I marked it out with a black line with orange dots.  It is unique per registration.

```
https://resources.azure.com/subscriptions/<>/resourceGroups/<>/providers/
      Microsoft.Web/sites/<>/config/authsettings/list
      "additionalLoginParams": ["response_type=code id_token",
      "resource=<AAD application ID>"],
```

Again, the application ID is the GUID for the application permission configured into AAD when creating the Function, see Figure 5, 6.

If you access the Azure Function now from a ***browser*** you will get the following error.

```
{
     "error": {
     "code": "UnknownError",
     "message": "",
     "innerError": {
        "request-id": "10445a2f-860e-467d-824f-215ed653b067",
        "date": "2018-10-29T13:18:28"
     }
   }
}
```

This is because the $value being returned is a binary data for the signed in user and not the actual image.  The client requesting the image will need to know how to work with that.

## Lab 8
U
pdate the line of code starting with “return await…” to the following.

```return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");```

Then you can enter the URL to the Microsoft Graph configured Azure Function into a ***browser*** and you will get the results of your profile as stored in your AAD tenant.  View/Confirm the details in the portal by clicking on Azure Active Directory –> Users –> Your Profile.  The output of the Function with be something similar to the following.

```
{"@odata.context":"https://graph.microsoft.com/v1.0/$metadata#users/$entity",
"id":"...0821d4f","businessPhones":],"displayName":"Benjamin Perkins)","givenName":
"Benjamin","jobTitle":"Cool","mail":null,"mobilePhone":null,"officeLocation":null,
"preferredLanguage":null,"surname":"Perkins",
"userPrincipalName":"benjamin_...#EXT#@<>.onmicrosoft.com"}
```

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
