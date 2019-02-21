# Configure Remote IIS Administration for Microsoft Azure App Service

***NOTE***: since the update to Windows Server 2016 and IIS 10 this capability is no longer supported and it likely never will be again.  In the short-term, you need to be looking for an alternative and learn to live without this one….IMO.  Consider Web Apps for Containers.

I wrote an article about configuring remote IIS administration [here][LINK1], but what I recently found out is that you can also add a Azure App Services like Web Apps, Mobile Apps, API Apps and Logic Apps to your IIS management console. This is super cool, because it makes the configuration of Failed Request Tracing, URL Rewrite, etc… much easier.

[Here][LINK2] is the article where I first discovered the capability, but I wanted to share my own experience.

Add an App Service to the IIS management console by selecting the Connect to a Site... menu item from within the IIS management console, as illustrated in Figure 1.

![Add an Azure App Service (Web App) for remote management][FIGURE1]
###### Figure 1, Add an Azure App Service (Web App) for remote management

Once clicked, enter the details of your App Service into the window, as shown in Figure 2.  In this example, I have a Web App named STANDARD and use the KUDU website URL for the administration.  I discuss KUDU in more detail [here][LINK3].

Once the information for your App Service is entered, click the Next button to enter the credentials, as shown in Figure 3.

![configure an  Azure App Service for remote management][FIGURE2]
###### Figure 2, configure an  Azure App Service for remote management

![Azure App Service publishing credentials for remote IIS management][FIGURE3]
###### Figure 3, Azure App Service publishing credentials for remote IIS management

The user name and password credentials required here are contained within your publish profile.  You can get this information by selecting on the web app you want to remote manage, as illustrated in Figure 4a.

![download publish profile from the Azure AUX portal][FIGURE4]
###### Figure 4a, download publish profile from the Azure AUX portal

In the new Azure portal, you can get the Publish Profile as shown in Figure 4b.

![download publish profile from the new/current Azure portal][FIGURE5]
###### Figure 4b, download publish profile from the new/current Azure portal

Use the userName and userPWD found within your publish profile file, similar to that shown in Figure 5.

![publishing profile example for an Azure App Service][FIGURE6]
###### Figure 5, publishing profile example for an Azure App Service

Enter the credentials, and click on the next button.

![adding credentials to IIS management console for remote Azure Web App management][FIGURE7]
###### Figure 6, adding credentials to IIS management console for remote Azure Web App management

Give the connection a name, and select the Finish button.

![name the remote administration Azure App Service connection][FIGURE8]
###### Figure 7, name the remote administration Azure App Service connection

After selecting the Finish button I received a message that there were some new and dependent features available, as illustrated in Figure 8.  I added all of them.

![new feature window when configuring Azure App Service for remote management][FIGURE9]
###### Figure 8, new feature window when configuring Azure App Service for remote management

Once added and connected, as shown in Figure 9, you can navigate through the configuration and make changed to the configuration as needed.  I like this feature a lot and will recommend it to any customer I work with.

![Remote management of an Azure App Service, ex: Web App, Mobile App, API App, Logic App in IIS][FIGURE10]
###### Figure 9, Remote management of an Azure App Service, ex: Web App, Mobile App, API App, Logic App in IIS

[FIGURE1]: ../images/2014/msdn-0425.png "Figure 1, Add an Azure App Service (Web App) for remote management"
[FIGURE2]: ../images/2014/msdn-0426.png "Figure 2, configure an  Azure App Service for remote management"
[FIGURE3]: ../images/2014/msdn-0427.png "Figure 3, Azure App Service publishing credentials for remote IIS management"
[FIGURE4]: ../images/2014/msdn-0428.png "Figure 4a, download publish profile from the Azure AUX portal"
[FIGURE5]: ../images/2014/msdn-0429.png "Figure 4b, download publish profile from the new/current Azure portal"
[FIGURE6]: ../images/2014/msdn-0430.png "Figure 5, publishing profile example for an Azure App Service"
[FIGURE7]: ../images/2014/msdn-0431.png "Figure 6, adding credentials to IIS management console for remote Azure Web App management"
[FIGURE8]: ../images/2014/msdn-0432.png "Figure 7, name the remote administration Azure App Service connection"
[FIGURE9]: ../images/2014/msdn-0433.png "Figure 8, new feature window when configuring Azure App Service for remote management"
[FIGURE10]: ../images/2014/msdn-0434.png "Figure 9, Remote management of an Azure App Service, ex: Web App, Mobile App, API App, Logic App in IIS"

[LINK1]: 2014-08-configure-remote-iis-administration-for-iis-web-sites.md
[LINK2]: http://blogs.msdn.com/b/windowsazure/archive/2014/02/28/remote-administration-of-windows-azure-websites-using-iis-manager.aspx
[LINK3]: 2014-03-using-kudu-with-windows-azure-web-sites.md
