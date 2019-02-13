# Download Azure App Services Web App log files from Visual Studio

I wrote an article about the ability to enable Web Server Logging, Detailed Error Message, Failed Request Tracing and Application Logging [here][LINK1].  In that article I discussed that you can download the logs using an FTP tool like FIlezilla. You can also download the log files from within Visual Studio by performing the following.  I currently have the Windows Azure SDK 2.1 installed, expand the Azure App Service Web Apps menu item within Server Explorer, then expand the subscription and right-click on the web site you want to down load the logs for.  Lastly, click on the View Settings… menu item shown in Figure 1.

![View Azure App Services Web Apps details in Visual Studio][FIGURE1]
###### Figure 1, View Azure App Services Web Apps details in Visual Studio

By default this opens a window showing the current configurations applied to the selected web site.  Figure 2 illustrates this.

![Azure App Service Web App configuration page in Visual Studio][FIGURE2]
###### Figure 2, Azure App Service Web App configuration page in Visual Studio

Notice that there is also a Logs item feature available for use.  Clicking the item renders the page similar to that shown in Figure 3.

![Download Logs from a an Azure App Service Web App using Visual Studio][FIGURE3]
###### Figure 3, Download Logs from a an Azure App Service Web App using Visual Studio

Notice that there are 3 available options.

+ ***Stream Logs*** – By placing System.Diagnostics.Tracing code into your application, once deployed to the Microsoft Azure platform, you can watch, in real time, when that piece of code is executed.  More information about that can be found [here][LINK2] and in my [book][LINK3].
+ ***Download Logs*** – Clicking on this link will download the data stored into the LogFiles directory of the Web Site as a ZIP file.
+ ***Configure Logging*** – Opens a browser and navigates to the CONFIGURE feature for the selected Windows Azure Web Site.

There is also an option to enable the Diagnostics on the Web Site.  This supports the viewing of events logged into the event viewer or custom events.  This feature requires the linking of a storage resource to the web site and the configuration of the Site Diagnostics to be configured to store the log into a Blog container.  An article on this is in the works.

[FIGURE1]: ../images/2013/msdn-0279.png "Figure 1, View Azure App Services Web Apps details in Visual Studio"
[FIGURE2]: ../images/2013/msdn-0280.png "Figure 2, Azure App Service Web App configuration page in Visual Studio"
[FIGURE3]: ../images/2013/msdn-0281.png "Figure 3, Download Logs from a an Azure App Service Web App using Visual Studio"

[LINK1]: ../2013/2013-09-enable-logging-on-your-windows-azure-web-site-from-visual-studio.md
[LINK2]: http://weblogs.asp.net/scottgu/archive/2013/04/30/announcing-the-release-of-windows-azure-sdk-2-0-for-net.aspx
[LINK3]: http://www.wrox.com/WileyCDA/WroxTitle/Windows-Azure-and-ASP-NET-MVC-Migration.productCd-1118678583.html
