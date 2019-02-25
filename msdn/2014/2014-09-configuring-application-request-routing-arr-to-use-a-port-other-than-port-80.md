# Configuring Application Request Routing (ARR) to use a port other than port 80

[Application Request Routing][LINK1] is a software based load balancing system, plus more.  When you configure your web farm you can choose from a number of different load balancing algorithm as shown in Figure 1.  I needed to add that in because I like the load balancing features in ARR, it is so simple compared to what I/we used to do before ARR.

![Application Request Routing Load Balance algorithms][FIGURE1]
###### Figure 1, Application Request Routing Load Balance algorithms

You can configure ARR to use a different port during the creation of the Server Farm.  By right-clicking on the Server Farms folder in the Connections tab of the IIS management console and clicking on Create Server Farm…, the windows shown in Figure 2 is rendered.

![Application Request Routing – create server farm ][FIGURE2]
###### Figure 2, Application Request Routing – create server farm 

Add the name of the server farm and click the Next button.  Figure 3 illustrates where you add the server names of the servers in your web farm with the Advanced Settings link expanded, this is where you can add a port which is different than port 80.  Port 80 is the default and if it is not changed then this is where all HTTP traffic is sent.  HTTPS traffic is sent to port 443 be default as well.

![Application Request Routing – Advanced Options  ][FIGURE3]
###### Figure 3, Application Request Routing – Advanced Options  

If you would like to change the port, enter the port you want HTTP traffic to be sent.  For example, port 3302.  There is nothing special about this port, it’s just a number I came up with fast, I better BING the number just in case it does have a meaning, but there is no implied meaning.  Enough…

You can modify this port as shown in Figure 4, then press Add to add another server to the farm or Finish to complete the installation.  Once you have added all the servers to your web farm click on the finish button.

![Application Request Routing – change port number to something other than port 80  ][FIGURE4]
###### Figure 4, Application Request Routing – change port number to something other than port 80  

Once added you can look at the configuration in the applicationHost.config file as shown in Figure 5.  This file contains all the IIS and some web site configurations and is located in c:\windows\system32\inetsrv\config by default.

![Application Request Routing – entry in applicationHost.config  ][FIGURE5]
###### Figure 5, Application Request Routing – entry in applicationHost.config  

Although you can open this file in Notepad or some other editor, be careful if you do that because the file must be in proper XML format and if you fat finger something, you will likely cause some problems.  [Back it up][LINK2] before you make any manual changes.

Once very useful tool that I think doesn’t get enough press is the Configuration Editor.  You can view and modify the webfarm element and most other IIS elements from here.  To do this, open the IIS management console, click on the Server name and in the Feature panel, double-click the Configuration Editor icon, expand the drop-down list and select webFarms, as shown in Figure 6.

![Application Request Routing – configuration editor - webFarms ][FIGURE6]
###### Figure 6, Application Request Routing – configuration editor - webFarms 

Click the (Collections) ellipses and the windows shown in Figure 7 is rendered.  The ellipses seems to not be present when the window is open, but I know you get the point.

![Application Request Routing – configuration editor - webFarms detail][FIGURE7]
###### Figure 7, Application Request Routing – configuration editor - webFarms detail

Click on the webFarm element and then again the ellipses for the given collection.  When clicked you will then have the ability to edit the port number, as illustrated on Figure 8.

![Application Request Routing – configuration editor – modify the port][FIGURE8]
###### Figure 8, Application Request Routing – configuration editor – modify the port

Make your changes and close the windows.  Before you click on Apply, check out the Generate Script link.  An even more, less mentioned feature is the script generator.  Click on the link in the Actions pane you will see the windows shown in Figure 9 that displays many different ways to make this change, for example, C#, JavaScript, AppCmd and in IIS 8+, PowerShell.

![Application Request Routing – configuration editor – generate script][FIGURE9]
###### Figure 9, Application Request Routing – configuration editor – generate script

[FIGURE1]: ../images/2014/msdn-0435.png "Figure 1, Application Request Routing Load Balance algorithms"
[FIGURE2]: ../images/2014/msdn-0436.png "Figure 2, Application Request Routing – create server farm "
[FIGURE3]: ../images/2014/msdn-0437.png "Figure 3, Application Request Routing – Advanced Options  "
[FIGURE4]: ../images/2014/msdn-0438.png "Figure 4, Application Request Routing – change port number to something other than port 80  "
[FIGURE5]: ../images/2014/msdn-0439.png "Figure 5,  Application Request Routing – entry in applicationHost.config  "
[FIGURE6]: ../images/2014/msdn-0440.png "Figure 6, Application Request Routing – configuration editor - webFarms "
[FIGURE7]: ../images/2014/msdn-0441.png "Figure 7, Application Request Routing – configuration editor - webFarms detail"
[FIGURE8]: ../images/2014/msdn-0442.png "Figure 8, Application Request Routing – configuration editor – modify the port"
[FIGURE9]: ../images/2014/msdn-0443.png "Figure 9, Application Request Routing – configuration editor – generate script"

[LINK1]: http://www.iis.net/downloads/microsoft/application-request-routing
[LINK2]: ../2011/2011-08-create-an-iis-configuration-backup.md
