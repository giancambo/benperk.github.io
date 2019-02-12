# 404 File or directory not found when mapping custom domain to an Azure App Service, Server Error

If you are receiving a 404 error while accessing your custom domain, as shown in Figure 1, then you are most likely missing the link between your Azure App Service Web App configuration and your custom domain.

![404 File or directory not found when mapping custom domain][FIGURE1]
###### Figure 1, 404 File or directory not found when mapping custom domain

Before we move to the solution, you might want to also check these descriptions of how to map a custom domain to a Azure App Service Web App found here and here.  Make sure you have followed all those instruction carefully.

+ http://blogs.msdn.com/b/kaushal/archive/2013/07/06/windows-azure-web-sites-how-to-configure-a-custom-domain.aspx
+ http://www.windowsazure.com/en-us/develop/net/common-tasks/custom-dns-web-site/
 
To resolve this issue, access the Azure management console https://manage.windowsazure.com/ and click on the App Service you are trying to link the custom domain to.  Once there, click the CONFIGURE link as illustrated in Figure 2.

![the CONFIGURE link for a given Azure App Service Web App][FIGURE2]
###### Figure 2, the CONFIGURE link for a given Azure App Service Web App

Scroll down until you find the Manage Domains button as shown in Figure 3.

![Manage Custom domains for an Azure App Service Web App][FIGURE3]
###### Figure 3, Manage Custom domains for an Azure App Service Web App

Clicking on the Manage Domains button will render a pop-up box that resembles Figure 4.

![Manage custom domains for a Windows Azure Web Site][FIGURE4]
###### Figure 4, Manage custom domains for a Windows Azure Web Site

Enter in your custom domain and the 404 issue should be resolved in a number of seconds.  If the issue remains or you get a pop-up error stating “The DNS record for ‘’ that points to ‘’ could not be located”, go back and review the entire configuration using the 2 links provided at the beginning of this article.

You might also want to check the Azure App Services forums for additional help location [here][LINK1].

***UPDATE 12-MAY-2014***:
When this happens suddenly, check the mode of your Web App, I.e. Free, Shared, Basic or Standard.  If your subscription gets suspended, your sites are set to Free mode and your custom domain setting is not valid in that mode.

***UPDATE 29-JAN-2014***:
Another reason I have found that this issue can exist is that the mapping of custom domains is only supported (when writing this) when the web site is running in either Shared, Basic or Standard mode.  I had an Azure App Services Web App in Standard mode and mapped a custom domain to it.  At a later point I scaled the mode down to Free for some reason and my custom domain stopped working and I got the 404 file or directory not found.

I scaled the App Service Web App back to Standard mode and the custom domain worked seconds later.

Setting up a custom domain requires changes to your DNS configuration, for example CNAME an A Record settings.  Once you make all these configurations, if it still doesn't work (and you are getting a 404) make sure your App Service is at the supported scale mode.

[FIGURE1]: ../images/2013/msdn-0275.png "Figure 1, 404 File or directory not found when mapping custom domain"
[FIGURE2]: ../images/2013/msdn-0276.png "Figure 2, the CONFIGURE link for a given Azure App Service Web App"
[FIGURE3]: ../images/2013/msdn-0277.png "Figure 3, Manage custom domains for a Windows Azure Web Site"
[FIGURE4]: ../images/2013/msdn-0278.png "Figure 4, IIS"

[LINK1]: http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=windowsazurewebsitespreview
