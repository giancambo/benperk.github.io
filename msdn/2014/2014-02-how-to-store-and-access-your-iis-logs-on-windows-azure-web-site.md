# How to store and access your IIS Logs on Microsoft Azure App Service

Being part of the IIS support team I have memorized the structure of an IIS log file and can see oddities almost instantly.  Therefore, one of the things I want to look at when troubleshooting any Microsoft Azure Web App issue are the logs.  As Microsoft Azure App Service is relatively new when I work with a customer on an issue I ask for these files and when I ask for the files, they generally respond with, ‘How?’.  Well, here’s one way to do it.  (you can also use an FTP client to download them...)

As this article is about getting logs from a Microsoft Azure Web Site, the first step is to have created a Microsoft Azure Web App as shown in Figure 1.

![Microsoft Azure Web App – Create a new Web Site][FIGURE1]
###### Figure 1, Microsoft Azure Web App – Create a new Web Site

Once the Web App is created, navigate to the Web Site and select the CONFIGURE link and scroll down until you see ‘site diagnostics’.  Unless you already have a storage account in the same region, in this case ‘North Europe’ as the Web App, the STORAGE option is greyed out as shown in Figure 2.

![Microsoft Azure Web Apps – Storage option greyed out][FIGURE2]
###### Figure 2, Microsoft Azure Web Apps – Storage option greyed out

To create a storage account navigate to the storage feature and select the +NEW link on the bottom left of the web page.  You do not have to be on the storage feature, but if you are it navigates the menu for you by default.  Create a storage account as
shown in Figure 3.

Create storage account in same region of App Service Web App.

![Microsoft Azure Web Sites – Create a Storage account for IIS Logs][FIGURE3]
###### Figure 3, Microsoft Azure Web Sites – Create a Storage account for IIS Logs

Notice that I have selected ‘North Europe’ as the region.  Like I already mentioned, the storage account must be created in the region and in the same subscription as the Microsoft Azure Web App.  There is also a message mentioning this on the CONFIGURATION page as previously shown in Figure 2. 

Next, return to the CONFIGURATION page for the Web App and scroll down to the ‘site diagnostics’ section.  The STORAGE feature is no longer greyed out and can be selected.  Figure 4 illustrates this.  Once selected the ‘manage storage’ button is rendered to support the linking of the web server logging to the storage account just created.

![Microsoft Azure Web App – Enable Web Server Logging to a Storage account][FIGURE4]
###### Figure 4, Microsoft Azure Web App – Enable Web Server Logging to a Storage account

Clicking on the ‘manage storage’ button open a window similar to that shown in Figure 5.  Select the storage account that was created via Figure 3 and leave the other defaults as they are.  If you have other storage accounts in the same region for the same subscription and/or other blob containers in the storage account you can select the ones you wish to store the IIS Logs.

![Microsoft Azure Web Apps – Manage storage for IIS Logs][FIGURE5]
###### Figure 5, Microsoft Azure Web Apps – Manage storage for IIS Logs

Select the Save button at the bottom of the page and you are ready to go.  Figure 6 illustrates the saved configuration.

![Microsoft Azure Web Apps – Saved configuration][FIGURE6]
###### Figure 6, Microsoft Azure Web Apps – Saved configuration

From this point on the IIS logs are being stored into the configured container of the selected storage account.  Note that this configuration can result in additional costs, so setting a retention policy, via the Set Retention checkbox can possibly reduce the long-term costs. 

There are numerous ways to download the IIS logs.  My preference is using the Windows Azure SDK from within Visual Studio.  Once downloaded you can do many Windows Azure tasks from within the IDE.  Figure 7 shows how to navigate and find the container from within Visual Studio 2013.

![Viewing Microsoft Azure Web Apps IIS Logs in Visual Studio, Windows Azure SDK][FIGURE7]
###### Figure 7, Viewing Microsoft Azure Web Apps IIS Logs in Visual Studio, Windows Azure SDK

By double-clicking on one of the blobs or right-clicking -> Open, the IIS log file is opened in a text editor which can be saved and analyzed.  The status of the download is shown via the Microsoft Azure Activity log shown in Figure 8.  I use [LogParser][LINK1] and wrote an article about it [here][LINK2].

![Downloading Microsoft Azure Web Apps IIS Logs in Visual Studio, Windows Azure SDK][FIGURE8]
###### Figure 8, Downloading Microsoft Azure Web Apps IIS Logs in Visual Studio, Windows Azure SDK

I have also used a tool called CloudXplorer which works pretty well, it’s shown in Figure 9.  You will need the storage account name and the primary access key, which is called secret key in CloudXplorer.  In Microsoft Azure this information is retrieved from the Storage account DASHBOARD page and clicking on the Manage Access Keys option at the bottom of the page, as illustrated in Figure 10.

![CloudXplorer for downloading blobs from a Microsoft Azure Storage account container, IIS Logs][FIGURE9]
###### Figure 9, CloudXplorer for downloading blobs from a Microsoft Azure Storage account container, IIS Logs

![Get the access key for the storage container in Microsoft Azure, Manage Access Keys][FIGURE10]
###### Figure 10, Get the access key for the storage container in Microsoft Azure, Manage Access Keys



[FIGURE1]: ../images/2014/msdn-0328.png "Figure 1, Microsoft Azure Web App – Create a new Web Site"
[FIGURE2]: ../images/2014/msdn-0329.png "Figure 2, Microsoft Azure Web Apps – Storage option greyed out"
[FIGURE3]: ../images/2014/msdn-0330.png "Figure 3, Microsoft Azure Web Sites – Create a Storage account for IIS Logs"
[FIGURE4]: ../images/2014/msdn-0331.png "Figure 4, Microsoft Azure Web App – Enable Web Server Logging to a Storage account"
[FIGURE5]: ../images/2014/msdn-0332.png "Figure 5, Microsoft Azure Web Apps – Manage storage for IIS Logs"
[FIGURE6]: ../images/2014/msdn-0333.png "Figure 6, Microsoft Azure Web Apps – Saved configuration"
[FIGURE7]: ../images/2014/msdn-0334.png "Figure 7, Viewing Microsoft Azure Web Apps IIS Logs in Visual Studio, Windows Azure SDK"
[FIGURE8]: ../images/2014/msdn-0335.png "Figure 8, Downloading Microsoft Azure Web Apps IIS Logs in Visual Studio, Windows Azure SDK"
[FIGURE9]: ../images/2014/msdn-0336.png "Figure 9, CloudXplorer for downloading blobs from a Microsoft Azure Storage account container, IIS Logs"
[FIGURE10]: ../images/2014/msdn-0337.png "Figure 10, Get the access key for the storage container in Microsoft Azure, Manage Access Keys"

[LINK1]: http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser
[LINK2]: http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser
