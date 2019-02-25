# The webpage cannot be found (404) when accessing a file on Azure App Services Web App

404 status codes can be a little misleading.  I wrote another article [here][LINK1] about 404’s here from an on premise IIS perspective, but in principle the information applies.  The fact about 404’s is that there are many sub status codes lined to the 404 status code.  Most people believe that the 404 means simply that the file doesn’t exist or cannot be found.  A closer look at the IIS log or a Failed Request Trace in search of the sub status number can help you find the solution.

Therefore, if you have deployed your code to an Azure Website, you try to access the file and you get a 404 similar to that shown in Figure 1, check the IIS sub status in the IIS log, Detailed Error Messages or via a Failed Request Trace.

I described how to enable Failed Request Tracing on an Azure Website [here][LINK2].  If you want the Detailed Error Messages then turn that feature on too.

![The webpage cannot be found – Azure App Service Web App][FIGURE1]
###### Figure 1, The webpage cannot be found – Azure App Service Web App

Once enabled the Failed Request trace can be found in the /LogFiles/W3SCV* directory of your Azure App Service Web App.  I used FileZilla and I find them in the directory illustrated in Figure 2.  Similarly, the IIS logs are in the /LogFiles/http/RawLogs directory and Detailed Error Messages in the /LogFiles/DetailedErrors.

![Failed Request Trace log on Azure App Services][FIGURE2]
###### Figure 2, Failed Request Trace log on Azure App Services

Once downloaded, check the Failed Request Trace for the sub status code, as shown in Figure 3.  Additionally, you can find the same information in the Detailed Error Messages, as shown in Figure 4.

![404.3 The webpage cannot be displayed on Azure App Services Web App / Mobile App 404, Failed Request Trace][FIGURE3]
###### Figure 3, 404.3 The webpage cannot be displayed on Azure App Services Web App / Mobile App 404, Failed Request Trace

![404.3 The webpage cannot be displayed on Azure Web App, Detailed Error Message][FIGURE4]
###### Figure 4, 404.3 The webpage cannot be displayed on Azure Web App, Detailed Error Message

Notice that the sub status code is 3, (404.3) and also notice that the description of what the sub status code means is described for you.  I.e. The page you are requesting cannot be served because of the extension configuration.  If the page is a script, add a handler.  If the file should be downloaded, add a MIME map.

To fix the issue and make *.BENPERK files available for download, add the mimeMap for .benperk, as shown in Figure 5, to your web.config file and publish it to your Azure App Service.

![add a mimeMap to your Azure Web App  to allow for downloads of custom file type][FIGURE5]
###### Figure 5, add a mimeMap to your Azure Web App  to allow for downloads of custom file type


[FIGURE1]: ../images/2014/msdn-0444.png "Figure 1, The webpage cannot be found – Azure App Service Web App"
[FIGURE2]: ../images/2014/msdn-0445.png "Figure 2, Failed Request Trace log on Azure App Services"
[FIGURE3]: ../images/2014/msdn-0446.png "Figure 3, 404.3 The webpage cannot be displayed on Azure App Services Web App / Mobile App 404, Failed Request Trace"
[FIGURE4]: ../images/2014/msdn-0447.png "Figure 4, 404.3 The webpage cannot be displayed on Azure Web App, Detailed Error Message"
[FIGURE5]: ../images/2014/msdn-0448.png "Figure 5, add a mimeMap to your Azure Web App  to allow for downloads of custom file type"

[LINK1]: ../2012/2012-06-displaying-detailed-error-messages-in-iis.md
[LINK2]: ../2013/2013-07-enabling-failed-request-logging-on-a-windows-azure-web-site.md
