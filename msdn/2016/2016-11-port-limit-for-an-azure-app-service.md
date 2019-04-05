# Port limit for an Azure App Service

Ever wondered how to check the number of open connections on a App Service Web App or Mobile App?  Have you ever received this error while running your App Service?

System.Net.Sockets.SocketException: "A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond XXX.XXX.XXX.XXX:80"

If yes, you might be hitting a connection limit.  These limits are based on the size of your App Service Plan, for example SMALL (1920), MEDIUM (3968) and LARGE (8064), the details can be found [here][LINK1].

Note that these limits are placed on the App Service Plan and not a specific App Service, like a Web App or Mobile App for example.  You can check the historical number of TCP connections in the portal by navigating to the App Service and selecting Metrics per instance (Apps).  See Figure 1 as an illustration of the output.

![how to check the number of open TCP connection for an App Service Web App or Mobile App][FIGURE1]
###### Figure 1, how to check the number of open TCP connection for an App Service Web App or Mobile App

If you have multiple App Services running on an ASP, you will need to look at them all, add them together to determine if you are breaching the limit.

At the moment running netstat -a is not allowed through KUDU which I discussed [here][LINK2].

[FIGURE1]: ../images/2016/msdn-1022.png "Figure 1, how to check the number of open TCP connection for an App Service Web App or Mobile App"

[LINK1]: https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits
[LINK2]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
