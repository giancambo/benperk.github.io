# Enable logging on your Windows Azure Web App – App Service from Visual Studio

With the release of Windows Azure SDK 2.0, you can enable the logging from within Visual Studio.  I wrote another article which discussed how to enable Failed Request Tracing while logged into the Microsoft Azure Management console.  You can review that article [here][LINK1].  After installing the SDK, open Visual Studio and example the Server Explorer, you may see something similar to that illustrated by Figure 1.

![Windows Azure SDK 2.0 Server Manager in Visual Studio 2012][FIGURE1]
###### Figure 1, Windows Azure SDK 2.0 Server Manager in Visual Studio 2012

Notice that you have access to the following Microsoft Azure services:

+ Microsoft Azure Compute, which are your Cloud Services, Web Roles, etc…
+ Microsoft Azure Service Bus
+ Microsoft Azure Storage
+ Microsoft Azure Virtual Machines
+ Microsoft Azure App Service

Next, right-click on the Microsoft Azure Web App that you want to view the setting for and select the View Settings… link as shown in Figure 2.  Notice that there is also an option to View Streaming Logs in Output Window.  This is a very nice feature that is used in combination with the System.Diagnostics.Trace capabilities.  I covered this in detail in my book called “Windows Azure and ASP.NET MVC Migration” as well as a deeper discussion of what I am discussing in this article, check it out [here][LINK2].

![View Settings of Windows Azure Web App from Visual Studio 2012][FIGURE2]
###### Figure 2, View Settings of Windows Azure Web App from Visual Studio 2012

A window, similar to that shown in Figure 3 is rendered.  There exists the ability to stop and start the Windows Azure Web App, changing the .NET version and configuring the following logging:

+ Web Server Logging
+ Detailed Error Messages
+ Failed Request Tracing
+ Application Logging

![Windows Azure Web App setting within Visual Studio 2012][FIGURE3]
###### Figure 3, Windows Azure Web App setting within Visual Studio 2012

Enable the Failed Request Tracing and select the Save link, shown in Figure 3.  Once enabled, the simpelest way to generate a file it to request a file which is not present.  This generates a 404 and gets logged.  For example, http://standard.azurewebsites.net/index.html results in a 404 and generate a log, thanks for clicking on it, you just generated a Failed Request Tracing log on my Microsoft Azure Web App.

Next, I use Filezilla to FTP to my Microsoft Azure Web App and download the traces.  The location of the Failed Request Trace can be seen in Figure 4.

![Failed Request Trace location on Windows Azure Web App][FIGURE4]
###### Figure 4, Failed Request Trace location on Windows Azure Web App

Download both the fr00***.xml and the freb.xsl file and open the fr00***.xml in Internet Explorer.  An example of the Failed Request Trace is illustrated with Figure 5.

![An example of a Failed Request Trace generated on Windows Azure Web App][FIGURE5]
###### Figure 5, An example of a Failed Request Trace generated on Windows Azure Web App


[FIGURE1]: ../images/2013/msdn-0268.png "Figure 1, Windows Azure SDK 2.0 Server Manager in Visual Studio 2012"
[FIGURE2]: ../images/2013/msdn-0269.png "Figure 2, View Settings of Windows Azure Web App from Visual Studio 2012"
[FIGURE3]: ../images/2013/msdn-0270.png "Figure 3, Windows Azure Web App setting within Visual Studio 2012"
[FIGURE4]: ../images/2013/msdn-0271.png "Figure 4, Failed Request Trace location on Windows Azure Web App"
[FIGURE5]: ../images/2013/msdn-0272.png "Figure 5, An example of a Failed Request Trace generated on Windows Azure Web App"

[LINK1]: ../2013/2013-07-enabling-failed-request-logging-on-a-windows-azure-web-site.md
[LINK2]: http://www.wrox.com/WileyCDA/WroxTitle/Windows-Azure-and-ASP-NET-MVC-Migration.productCd-1118678583.html
