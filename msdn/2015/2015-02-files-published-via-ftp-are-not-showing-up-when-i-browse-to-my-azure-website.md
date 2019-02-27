# Files published via FTP are not showing up when I browse to my App Service

Have you ever FTPed some files to your Azure App Service and when you went to browse the site you still saw the old files or the ones you published do not exist?

When you have multiple Azure App Services hosted in the same region (I.e. West Europe, East US, etc…), exist in the same App Service Plan ([ASP][LINK1]) and the sites are running in Basic or Standard mode it means that all your Azure App Services are running on the same Virtual Machine (VM).  If you check the FTP HOST NAME value on the DASHBOARD for one of your sites, when compared to your other sites in the same region, the FTP HOST NAME value is the same.

So you might ask, if my FTP USER is the same for all my Azure Websites in the same region, how does Azure App Services know which site I want to publish to?  The answer is, that you provide the Azure App Service name before the UserID, similar to the way you provide your domain credential when logging into a corporate domain.

For example, if you have three Azure App Services one named CONTOSO1, one CONTOSO2 and another named CONTOSO3, where each are in the same region, are running in Basic or Standard mode and exist in the same App Service Plan ([ASP][LINK1]), you would access them like:

+ CONTOSO***1***\UserID
+ CONTOSO***2***\UserID
+ CONTOSO***3***\UserID

Where UserID is the FTP USER you created your deployment credentials.

Be sure you let the Azure App Service platform know which Website you want the files to be published to, by providing that piece of information. I.e. the website name.  Then you can be sure the files will show as expected when browsed.

There are numerous other reasons why this might happen, but this one is an example of the one I see often.

This can also happen if you have enabled LOCAL CACHE and are publishing directly to production. If you have LOCAL CACHE enabled, then you need to publish and test on a deployment slot. Once all the testing is complete, then swap the slot with production and those files will be the production ones. As of now, there is no logic that keeps the LOCAL CACHE (D:\local\…) synchronized with the D:\home\…

[LINK1]: http://azure.microsoft.com/en-us/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
