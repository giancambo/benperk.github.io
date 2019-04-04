# How (I) connected a console application to a Web API protected by an Azure Active Directory

The code can be downloaded from [here][LINK1] or on [GitHub][LINK2].

I wrote this post [here][LINK3] where I discussed how I configured Azure Active Directory into one of my Azure App Service Web Apps.  One of the main differences between that post and what I am trying to do here was realized when I received this exception:

AADSTS90014: The request body must contain the following parameter: 'client_secret or client_assertion'.

What this ultimately lead is my understanding that when I configure/create my Azure Active Directory application during the configuration the, shown in Figure 4, is that is creates a ‘confidential’ application.  I am not part of the Active Directory team so I can only speculate that it means that only certain types of applications can access this application, possibly only applications running on the Azure Platform.  To get the access to work using this client application I needed to create a ‘public’ application.  Check out the section called ‘Configure a native client application’ [here][LINK4] which IMO is the way in which you create a ‘public’ Azure Active Directory application.

+ WEB APPLICATION AND/OR WEB API –> CONFIDENTIAL
+ NATIVE CLIENT APPLICATION –> PUBLIC

Here are the steps I took to get my console application using an HttpClient to call a Web API hosted on the Azure platform as an App Service Web App:

+ Create the Web API and publish it to the Web App
+ Create the Console application and test the Web API is working – without authentication
+ Configure Authentication/Authorization for the Web App
+ Call the protected Web API, test and fail miserably…
+ Create a Native Client Application in my Azure Active Directory, more information [here][LINK4]
+ Troubleshoot some more

Sweet success!  This wasn’t easy, but should it be?
This was a real challenge for me, it took a lot of time and trail to get it to work and I wasn’t able to find an end-to-end set of instructions for getting it to work, maybe because this is still in PREVIEW so it has and likely will change…let’s get started.

Check out my other post on a similar subject:  [How (I) configured Azure Active Directory into my ASP.NET MVC OWIN web application][LINK3]

## Create the Web API and publish it to the Web App

I created a simple Web API using Visual Studio 2015 as seen in Figure 1.

![authenticate a Web API using Azure Active Directory][FIGURE1]
###### Figure 1, authenticate a Web API using Azure Active Directory

I added a controller named MgmtController.cs with a method called GetMgmt(string id, string value) that simply returns the DateTime and the parameters that are sent to it.  See code snippet 1 that shows the code.

```
namespace mgmt.Controllers
{
    public class MgmtController : ApiController
    {
        [HttpGet]
        [Route("api/GetMgmt/{id}/{value}")]
        public IHttpActionResult GetMgmt(string id, string value)
        {
            try
            {
                return Ok(DateTime.Now.ToString() + $" id is '{id}' and the value is '{value}'");
            }
            catch (Exception ex)
            {
                return Ok(ex.Message.ToString());
            }
        }
    }
}
```

###### Code snippet 1, an example Web API used to authenticate against an Azure Active Directory Application running on a Azure Web App

## Create the Console application and test the Web API is working – without authentication

Next I created a console application, as shown in Figure 2.

![C# console application for consuming Web API protected by Azure Active Directory running on an Azure App Service Web App][FIGURE2]
###### Figure 2, C# console application for consuming Web API protected by Azure Active Directory running on an Azure App Service Web App

The code that called the Web API is shown in code snippet 2.

```
static async Task CallWebApiUnprotectedAsync(string webApiUrl)
{
 try
 {
   HttpClient client = new HttpClient();
   Uri requestURI = new Uri(webApiUrl);
   WriteLine($"Reading values from '{requestURI}'.");
   HttpResponseMessage httpResponse = await client.GetAsync(requestURI);
   WriteLine($"HTTP Status Code: '{httpResponse.StatusCode.ToString()}'");
   WriteLine($"HTTP Response: '{httpResponse.ToString()}'");
   string responseString = await httpResponse.Content.ReadAsStringAsync();
   var json = JsonConvert.DeserializeObject(responseString);
   WriteLine($"JSON Response: {json}");
 }
 catch(Exception ex)
 {
   WriteLine("Exception in CallWebApiUnprotectedAsync(): " + ex.Message);
 }            
}
```

###### Code snippet 2, an example Console application calling a Web API running on an Azure Web App

Running the code I get the output shown in Figure 3.

![calling a Web API hosted on an Azure Web App from a console application][FIGURE3]
###### Figure 3, calling a Web API hosted on an Azure Web App from a console application

Configure Authentication/Authorization for the Web App
Now that I was certain my Web API worked and can be called, unprotected from my console application I moved onto the enablement of Azure Active Directory.  I had already created an Azure Active Directory as you can see in Figure 4 in the Current Active Directory value of ‘Benjamin Perkins’ on the Azure Active Directory Settings blade.  The actual Azure Active Directory is benjaminPerkins.onmicrosoft.com,

So, from the beginning, I published the App Service Web App to a the platform, it is called ‘mgmt’.  I logged into the Azure Management portal here and and navigated to the Web App.  Then I clicked on the Authentication / Authorization settings link which opened the Authentication / Authorization blade.  By default it is Off, so click the On button, since I wanted to enable Azure Active Directory, I selected it from the drop-down, then I click on Azure Active Directory from the list of Authentication Providers which opened the blade I referred to initially ‘Azure Active Directory Settings.  Click on Express, unless you want to do some Advanced stuff, leave the Create New Ad App selected and give the App an name, in my case ‘mgmtADApp’.  Like mentioned originally, this creates a CONFIDENTIAL application.

![enabled / configure Azure Active Directory authentication for an Azure App Service Web App][FIGURE4]
###### Figure 4, enabled / configure Azure Active Directory authentication for an Azure App Service Web App

Click OK, then Save the configuration.  You can then navigate to the Azure Active Directory feature in the Azure Management Portal here and see that the App is registered and has an application type equal to Web App / API.  As seen in Figure 5.

![register a CONFIDENTIAL Azure Active Directory Application][FIGURE5]
###### Figure 5, register a CONFIDENTIAL Azure Active Directory Application

What I did next was access the Web App using a browser, just to make sure that I could, indeed login and it was successful.  As I was already logged into the Azure Management portal with the identity that is allowed AAD access to my Web App, I was not prompted to provide my credentials.  I noticed during the login that I get redirected, authenticated and redirected back to the my site.  I copied the URL while it was waiting on the authentication and found some valuable information within it.

+ AUTHORITY = “login.microsoftonline.com/b29343ba-***/oauth2/authorize”, not, it does need to have HTTPS:// in front of it, but didn’t want it to show up as a link
+ REDIRECT URI = “mgmt.azurewebsites.net/.auth/login/aad/callback”
+ CLIENT ID = “4f2e934d-***”
+ RESOURCE = ??? this will come later, keep reading…

The above 4 bullet points are the values you need to create an instance of Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext, and those were not easy nor simple to find….I found out…

## Call the protected Web API, test and fail miserably…

Before we get to the solution, I want to share some exceptions I worked through.

+ AADSTS50001: The application named https://benjaminPerkins.onmicrosoft.com/mgmtADApp was not found in the tenant named b29343ba… .This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant. You might have sent your authentication request to the wrong tenant.  SOLVED Added the correct name into the resource value, what value should go there?  I bet you wish you could find that, I will tell you later.
+ AADSTS50011: The reply address 'https://mgmt.azurewebsites.net' does not match the reply addresses configured for the application: '4f2e934d-***”’. More details: not specified  SOLVED Had HTTP configured as the REDIRECT URIS and I used HTTPS in my code, made both HTTPS
+ AADSTS65001: The user or administrator has not consented to use the application with ID '…'. Send an interactive authorization request for this user and resource - I was not able to resolve this one, it just went away, maybe because I accessed the resource from my browser and consented manually, or I set the PromptBehavior.Always which prompted me for credentials from the client with every access request.
+ AADSTS90014: The request body must contain the following parameter: 'client_secret or client_assertion'.  SOLVED  This is where I learned about CONFIDENTIAL and PUBLIC Azure Active Directory Application types
+ The page cannot be displayed because an internal error has occurred.  SOLVED when the AAD application is set to NATIVE CLIENT APPLICATION, then I got that exception when I tried accessing the Web App via a browser.  If you get this one, Figure 6, you are way going in the wrong direction…I enabled Failed Request Tracing, shown later in Figure 8 and saw the error was a 500.79 IIS status code 500 sub status code 79

![The page cannot be displayed because an internal server error has occurred][FIGURE6]
###### Figure 6, The page cannot be displayed because an internal server error has occurred

After finally figuring out that I needed a PUBLIC NATIVE CLIENT APPLICATION I got a 401.71, IIS status 401 sub status 71, HTTP Status Code: 'Unauthorized', HTTP Response: 'StatusCode: 401, ReasonPhrase: 'Unauthorized'.  I saw the 401 in the output of my console application, as shown in Figure 7, but I, like so often, the real detail comes with the sub status code which I found be enabling Failed Request Tracing, Figure 8 and reviewing the XML file, Figure 9.

![received a 401.71 IIS unauthorized HTTP response from my Azure Active Directory Application running in an App Service from a client][FIGURE7]
###### Figure 7, received a 401.71 IIS unauthorized HTTP response from my Azure Active Directory Application running in an App Service from a client

![enable FREB logs to troubleshoot 401.71 Azure Active Directory Application authentication issue][FIGURE8]
###### Figure 8, enable FREB logs to troubleshoot 401.71 Azure Active Directory Application authentication issue

Finding the sub status code took me further as there were some other articles with tips and thoughts about why it was happening and how to resolve.  None provided the complete solution, but they moved me forward.

![FREB trace with the 401.71 issue when calling an AAD application from a console application][FIGURE9]
###### Figure 9, FREB trace with the 401.71 issue when calling an AAD application from a console application

So, finally, here are the steps I took to code the console application that calls a Web API hosted on Azure as an App Service Web App protected by Azure Active Directory:

+ Add the Microsoft.IdentityModel.Clients.ActiveDirectory NuGet Package
+ Write the code find out the values for AUTHORITY, RESOURCE, CLIENTID, REDIRECTURI and PARAMETERS
+ It still will not work because we have not created nor configured the NATIVE CLIENT APPLICATION, next section

## Add the Microsoft.IdentityModel.Clients.ActiveDirectory NuGet Package

Right-click the console application project and select Manage NuGet Packages… as shown in Figure 10.

![add a NuGet Package library to a console application in Visual Studio][FIGURE10]
###### Figure 10, add a NuGet Package library to a console application in Visual Studio

Search for Microsoft.IdentityModel.Clients.ActiveDirectory and install it, you should find something similar to Figure 11.

![install the NuGet package Microsoft.IdentityModel.Clients.ActiveDirectory  401.71 Azure Active Directory Web API][FIGURE11]
###### Figure 11, install the NuGet package Microsoft.IdentityModel.Clients.ActiveDirectory  401.71 Azure Active Directory Web API

## Write the code find out the values for AUTHORITY, RESOURCE, CLIENTID, REDIRECTURI and PARAMETERS

The code in Code Snippet 3 is how I called the API.

Points:

+ The AUTHORITY for my example is the OAUTH 2.0 TOKEN ENDPOINT, accessible as illustrated in Figure 12.  I used the OAUTH 2.0 AUTHORIZATION ENDPOINT for a long time, no idea why, but after trying everything else, I switched it and retired everything again…
+ The RESOURCE was the one that took me the longest to find, it turns out that it is the CLIENT ID of the CONFIDENTIAL Azure Active Directory Application I created in Figure 4.  It is also references to as the Application Id in the portal, you can see it also in Figure 12.  Notice that it is the one called mgmtADApp, which matches the one created in Figure 4 and has an application type of Web App / API.
+ The CLIENT ID is what you create in the next section where you create the PUBLIC NATIVE CLIENT APPLICATION, but you can already see it in Figure 12 too, it is called mgmtADAppClient and has an Application type of Native.  The Application Id is what is commonly referred to as CLIENT ID.
+ The REDIRECT URL, they just need to match what you place into the configuration of the Azure Active Directory Application, otherwise you get AADSTS50011 which I described earlier
+ The parameters is of type IPlatformParameters I instantiate it to Always prompt for credentials as I was using numerous ones, it just reduced the chance of the client using an identity which was not configured as a user

```
static async Task CallWebApiProtectedAsync(string webApiUrl)
{
 parameters = new PlatformParameters(PromptBehavior.Always);
 //string authority = "https://login.microsoftonline.com/b29343ba-***/oauth2/authorize"; // Authorization endpoint
 string authority = "https://login.windows.net/b29343ba-***/oauth2/token"; //token endpoint
 string resource = "4f2e934d***";  //Web API Client Id (mgmtADApp)
 string clientId = "a2b01fd9-***"; //Client Id (mgmgADAppClient)
 string redirectUri = "https://mgmt.azurewebsites.net/.auth/login/done";
 try
 {
  AuthenticationContext authContext = new AuthenticationContext(authority);
  var token = await authContext.AcquireTokenAsync(resource, clientId, new Uri(redirectUri), parameters);
  var authHeader = token.CreateAuthorizationHeader();
  HttpClient client = new HttpClient();
  client.DefaultRequestHeaders.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue(token.AccessTokenType, token.AccessToken);
  Uri requestURI = new Uri(webApiUrl);
  WriteLine($"Reading values from '{requestURI}'.");
  HttpResponseMessage httpResponse = await client.GetAsync(requestURI);
  WriteLine($"HTTP Status Code: '{httpResponse.StatusCode.ToString()}'");
  WriteLine($"HTTP Response: '{httpResponse.ToString()}'");
  string responseString = await httpResponse.Content.ReadAsStringAsync();
  var json = JsonConvert.DeserializeObject(responseString);
  WriteLine($"JSON Response: {json}");
 }
 catch (Exception ex)
 {
  WriteLine("Exception in CallWebApiUnprotectedAsync(): " + ex.Message);
 }
}
```

###### Code Snippet 3, calling a Web API on Azure Web App protected by Azure Active Directory using a console

Navigate to Figure 12 by selecting the Azure Active Directory feature then click the App Registrations then Endpoints.

![finding the right endpoint for Azure Active Directory authentication][FIGURE12]
###### Figure 12, finding the right endpoint for Azure Active Directory authentication

NOTE to self –> find out what these other endpoints do and are and how to use them and why to use them versus the others…

I read through this book "Modern authentication with Azure Active Directory for Web applications" by Vittorio Bertocci which does a super job at describing the Endpoint types:

+ WS-Federation is used primarily with WFC, it is heavy
+ SAML is an old school SSO protocol
+ Microsoft Graph is for exposing O365 resources to your users
+ OAUTH is for granting authentication to access Website B content from Website A on behalf of a customer

## Create a Native Client Application in my Azure Active Directory

The instructions are pretty straight forward [here][LINK5], so won’t go too deep.  Focus on the section “(Optional) Configure a native client application”.

Much of the configuration of the Azure Active Directory is still in the AUX portal [here][LINK6], no problem with that.  Open the Active Directory link and you’ll see something similar to that shown in Figure 13.

![adding an native client application to azure active directory][FIGURE13]
###### Figure 13, adding an native client application to azure active directory

Notice also after clicking on the APPLICATIONS link that the mgmtADApp which was created in Figure 4 is present.

Click the ADD+ button at the bottom of the page similar to that in Figure 14.

![add an Azure Active Directory Application][FIGURE14]
###### Figure 14, add an Azure Active Directory Application

Then follow the wizard that which consists of 3 pages, Figure 15, Figure 16 and Figure 17.

![Add an application my organization is developing][FIGURE15]
###### Figure 15, Add an application my organization is developing

![Create a NATIVE CLIENT APPLICATION, Web API, Web App, Active Directory][FIGURE16]
###### Figure 16, Create a NATIVE CLIENT APPLICATION, Web API, Web App, Active Directory

![REDIRECT URI for an Azure Active Directory protected Azure App Services Web App][FIGURE17]
###### Figure 17, REDIRECT URI for an Azure Active Directory protected Azure App Services Web App

This is the setting I reference to earlier in point 4 of the Write the code section, this value and the one in the code need to match 100%.

Once created, you can look in the new and AUX portal to see the mgmtADAppClient, like in Figure 12.

Set the permissions, this one kind of got me at first.  I thought I needed to add the application I just created to the app I just created, which means I wanted to add the app to itself.  It turns out I needed to add the mgmgADApp, CONFIDENTIAL application to the NATIVE PUBLIC applciation I just created, I saw Figure 18 and 19.

![X when adding a permission to an Azure Active Directory Application][FIGURE18]
###### Figure 18, X when adding a permission to an Azure Active Directory Application

I would expect the X shown in Figure 18 because I wouldn’t want, need to grant a permission to itself.

![the X when adding a permission to an Azure Active Directory Application is gone][FIGURE19]
###### Figure 19, the X when adding a permission to an Azure Active Directory Application is gone

Then, back on the CONFIGURE page for my mgmgADAppClient application I need to grant the Access mgmtAdApp to this NATIVE PUBLIC Azure Active Directory application, shown in Figure 20.  Save the configuration, notice the CLIENT ID use it in the code.

![add permission to access Web API, Web App, Client / Console App, Azure Active Directory][FIGURE20]
###### Figure 20, add permission to access Web API, Web App, Client / Console App, Azure Active Directory

You can also see the permissions in the new portal, as shown in Figure 21.

![check the Azure Active Directory permissions][FIGURE21]
###### Figure 21, check the Azure Active Directory permissions

Then i run the code and, holy jeebers, 200!!!

![Success][FIGURE22]
###### Figure 22, Success

I love this stuff, the satisfaction I get when it works is …..

[FIGURE1]: ../images/2016/msdn-0979.png "Figure 1, authenticate a Web API using Azure Active Directory"
[FIGURE2]: ../images/2016/msdn-0980.png "Figure 2, C# console application for consuming Web API protected by Azure Active Directory running on an Azure App Service Web App"
[FIGURE3]: ../images/2016/msdn-0981.png "Figure 3, calling a Web API hosted on an Azure Web App from a console application"
[FIGURE4]: ../images/2016/msdn-0982.png "Figure 4, enabled / configure Azure Active Directory authentication for an Azure App Service Web App"
[FIGURE5]: ../images/2016/msdn-0983.png "Figure 5, register a CONFIDENTIAL Azure Active Directory Application"
[FIGURE6]: ../images/2016/msdn-0984.png "Figure 6, The page cannot be displayed because an internal server error has occurred"
[FIGURE7]: ../images/2016/msdn-0985.png "Figure 7, received a 401.71 IIS unauthorized HTTP response from my Azure Active Directory Application running in an App Service from a client"
[FIGURE8]: ../images/2016/msdn-0986.png "Figure 8, enable FREB logs to troubleshoot 401.71 Azure Active Directory Application authentication issue"
[FIGURE9]: ../images/2016/msdn-0987.png "Figure 9, FREB trace with the 401.71 issue when calling an AAD application from a console application"
[FIGURE10]: ../images/2016/msdn-0988.png "Figure 10, add a NuGet Package library to a console application in Visual Studio"
[FIGURE11]: ../images/2016/msdn-0989.png "Figure 11, install the NuGet package Microsoft.IdentityModel.Clients.ActiveDirectory  401.71 Azure Active Directory Web API"
[FIGURE12]: ../images/2016/msdn-0990.png "Figure 12, finding the right endpoint for Azure Active Directory authentication"
[FIGURE13]: ../images/2016/msdn-0991.png "Figure 13, adding an native client application to azure active directory"
[FIGURE14]: ../images/2016/msdn-0992.png "Figure 14, add an Azure Active Directory Application"
[FIGURE15]: ../images/2016/msdn-0993.png "Figure 15, Add an application my organization is developing"
[FIGURE16]: ../images/2016/msdn-0994.png "Figure 16, Create a NATIVE CLIENT APPLICATION, Web API, Web App, Active Directory"
[FIGURE17]: ../images/2016/msdn-0995.png "Figure 17, REDIRECT URI for an Azure Active Directory protected Azure App Services Web App"
[FIGURE18]: ../images/2016/msdn-0996.png "Figure 18, X when adding a permission to an Azure Active Directory Application"
[FIGURE19]: ../images/2016/msdn-0997.png "Figure 19, the X when adding a permission to an Azure Active Directory Application is gone"
[FIGURE20]: ../images/2016/msdn-0998.png "Figure 20, add permission to access Web API, Web App, Client / Console App, Azure Active Directory"
[FIGURE21]: ../images/2016/msdn-0999.png "Figure 21, check the Azure Active Directory permissions"
[FIGURE22]: ../images/2016/msdn-1000.png "Figure 22, Success"

[LINK1]: https://code.msdn.microsoft.com/Azure-App-Web-App-Web-API-75672c9e
[LINK2]: https://github.com/benperk/mgmt
[LINK3]: 2016-09-how-i-configured-azure-active-directory-into-my-asp-net-mvc-owin-web-application.md
[LINK4]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-how-to-configure-active-directory-authentication/
[LINK5]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-how-to-configure-active-directory-authentication/
[LINK6]: https://manage.windowsazure.com/
