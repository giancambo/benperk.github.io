# Making changes to the applicationHost.config on Azure App Service

There are a few other resources about this [here][LINK1] and [here][LINK2], but as experiences and changes happen, documenting and sharing different perspectives help move projects and people forward.  Additionally, there is no better way to learn than by doing and no better way to remember than by documenting or writing it down. [Here][LINK3] is an extension that will make this process eaiser.

As you likely know, not all IIS configuration elements can be modified on the Azure App Services platform.  [Here][LINK4] is the most current list of the elements which you can modify that I am aware of.

An example is the [weblimits][LINK5] element that can control connectTimeout, headerWaitTime, maxGlobalBandwidth, minBytesPerSecond, etc.. behavior.  As shown in Figure 1, using the IIS Management console on a stand-alone instance of IIS, changing these values is achieved via the Configuration Editor.

![changing applicationHost variables, Azure Web App][FIGURE1]
###### Figure 1, changing applicationHost variables, Azure Web App

There is no interface for doing this to an Azure App Service Web App, nor will there ever likely be one, because access to system configuration settings would cause some issues when automated scaling, or manual for that matter, happened.  I.e. system configurations are not replicated to other instances when scaled.  To get around this, the Azure Web App owner, can create an XDT file that includes these system configurations and publish them to the website.  Then, when the website is scaled, the settings can be propagated as required, because theses settings are part of the website configuration and not the system.

I did write a post about configuring the IIS Management Console for use with an App Service here, regardless of that you cannot change all the options, but at least you can look at them and see what the setting are.

For this example, want to decrease the connectionTimeout to 30 seconds.  To achieve this, I performed the following.

1. Create an applicationHost.xdt file
2. Create the content for the XDT file
3. Publish the XDT file
4. Confirm the change was applied

## Create an applicationHost.xdt file

I created a file called applicationHost.xdt and placed it in my Azure App Service Web App project using Visual Studio 2013/15, similar to that show in Figure 2.

![adding an applicationHost.xdt file to my project][FIGURE2]
###### Figure 2, adding an applicationHost.xdt file to my project

## Create the content for the XDT file

Once created I added the following content, shown in Listing 1, to the applicationHost.xdt file.

###### Listing 1, reducing the connectionTimeout limit on Azure App Service Web App

```
<?xml version="1.0"?>
<configuration  xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <webLimits xdt:Transform="SetAttributes(connectionTimeout)" 
               connectionTimeout="00:00:30" />
  </system.applicationHost>
</configuration>
```

## Publish the XDT file

I then published the XDT file to my Azure App Service Web App to the /site directory, similar to that shown in Figure 3 using FileZilla.

![where to publish the XDT for an Azure Web App][FIGURE3]
###### Figure 3, where to publish the XDT for an Azure Web App

## Confirm the change was applied

Before the configuration is visible, a restart of the Azure Web App is required.  Additionally, make an initial request to the website to make sure the worker process is running.  There are certainly numerous options to get the Azure Web App configuration, I used the CMD Debug Console from KUDU, which I have discussed [here][LINK7], shown in Figure 4.  As you can see, after navigating to D:\home\site, I executed the following command: 

```
type c:\dwasfiles\sites\#1standard\config\applicationHost.config > myAppHostConfig.config
type D:\local\config\applicationHost.config > myAppHostConfig.config
```

You can find your App Service applicationHost.config file by following [these][LINK8] instructions.

![how to get the applicationHost.config for an Azure App Service Web App][FIGURE4]
###### Figure 4, how to get the applicationHost.config for an Azure App Service Web App

Once created, the file can be downloaded using an FTP tool or by clicking the download link, illustrated by Figure 5, within KUDU. 

![download the applicationHost.config for you Azure App Service Web App][FIGURE5]
###### Figure 5, download the applicationHost.config for you Azure App Service Web App

When I clicked on the download link the configuration file was opened in my browser.  Searching for weblimits resulted in finding the desired element configuration, shown in Figure 6.

![finding the XDT element in the extended Azure App Service Web App applicationHost.config file][FIGURE6]
###### Figure 6, finding the XDT element in the extended Azure App Service Web App applicationHost.config file

***NOTE***:  Not all variables are extendable.  When I find a list of which ones are or are not, I will share.  For example, I do not think the sites\siteDefaults\limits element can be extended.  This was what I initially tried to get to work, without luck, so there is obviously some Azure App Service Web App logic preventing its change and likely protecting me from changing something that would cause problems with my website or for others.


[FIGURE1]: ../images/2015/msdn-0492.png "Figure 1, changing applicationHost variables, Azure Web App"
[FIGURE2]: ../images/2015/msdn-0493.png "Figure 2, adding an applicationHost.xdt file to my project"
[FIGURE3]: ../images/2015/msdn-0494.png "Figure 3, where to publish the XDT for an Azure Web App"
[FIGURE4]: ../images/2015/msdn-0495.png "Figure 4, how to get the applicationHost.config for an Azure App Service Web App"
[FIGURE5]: ../images/2015/msdn-0496.png "Figure 5, download the applicationHost.config for you Azure App Service Web App"
[FIGURE6]: ../images/2015/msdn-0497.png "Figure 6, Feature"

[LINK1]: http://azure.microsoft.com/en-us/documentation/articles/web-sites-transform-extend/
[LINK2]: http://blogs.msdn.com/b/waws/archive/2014/06/17/transform-your-microsoft-azure-web-site.aspx
[LINK3]: https://github.com/shibayan/IISManager
[LINK4]: http://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/
[LINK5]: http://www.iis.net/configreference/system.applicationhost/weblimits
[LINK6]: https://blogs.msdn.microsoft.com/benjaminperkins/2014/08/05/configure-remote-iis-administration-for-iis-web-sites/
[LINK7]: http://blogs.msdn.com/b/benjaminperkins/archive/2014/03/24/using-kudu-with-windows-azure-web-sites.aspx
[LINK8]: https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions#finding-your-applicationhostconfig
