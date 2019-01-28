# Using KUDU with Microsoft Azure Web Apps

There is a nice set of troubleshooting and analysis tools for use with Microsoft Azure Web Apps (formerlly Web Sites) called KUDU.  Information about that tool set can be found [here][LINK1].

It is useful for capturing a memory dump, as explained [here][LINK2], looking at deployment logs, viewing configuration parameters and much more.

To access your KUDU console, using your DEPLOYMENT credentials, navigate to https://*****.scm.azurewebsites.net where ***** is the name of your Web App.  Once authenticated, you should see a page similar to the one illustrated with Figure 1.

![KUDU Microsoft Azure App Service Web App troubleshooting console][FIGURE1]
###### Figure 1, KUDU Microsoft Azure App Service Web App troubleshooting console

Clicking on the Debug Console, then PowerShell open a console similar to that shown in Figure 2.

![Microsoft Azure App Service Web App PowerShell console][FIGURE2]
![Microsoft Azure App Service Web App PowerShell console][FIGURE3]
###### Figure 2, Microsoft Azure App Service Web App PowerShell console

Enter the following command and you can see the processes running on your web site, results are shown in Figure 3.

``` powershell –command get-process ```

![View Microsoft Azure App Service Web App processes using PowerShell][FIGURE4]
![View Microsoft Azure App Service Web App processes using PowerShell][FIGURE5]
###### Figure 1, View Microsoft Azure App Service Web App processes using PowerShell

[FIGURE1]: ../images/2014/msdn-0072.png "Figure 1, KUDU Microsoft Azure App Service Web App troubleshooting console"
[FIGURE2]: ../images/2014/msdn-0073.png "Figure 2, Microsoft Azure App Service Web App PowerShell console"
[FIGURE3]: ../images/2014/msdn-0074.png "Figure 3, Microsoft Azure App Service Web App PowerShell console"
[FIGURE4]: ../images/2014/msdn-0075.png "Figure 4, View Microsoft Azure App Service Web App processes using PowerShell"
[FIGURE5]: ../images/2014/msdn-0076.png "Figure 5, View Microsoft Azure App Service Web App processes using PowerShell"

[LINK1]: https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession
[LINK2]: http://blogs.msdn.com/b/asiatech/archive/2013/07/18/using-kudu-to-capture-hang-dumps-for-windows-azure-web-sites.aspx
