# Configure an IIS Server Core server for remote management

To configure an IIS 8.5 server running on Server Core, perform the following:

+ Install the Web-Server role
+ Install the Web-Mgmt-Service
+ Enable Remote Management
+ Set the WMSVC service to start automatically
+ Create connection from Remote IIS Management Console

In a situation where you need to closely manage physical resources, you might consider using the Server Core version of Windows Server to run your IIS server.  It is obvious, with my experiences that Server Core requires much less CPU and memory to run.  At the same time, I do prefer to use the IIS Management Console to do much of my administration, so I thought, hey, I can 1 server running the GUI version of Windows Server where I could then remotely manage all my working IIS servers from.  I simply needed to configure the worker IIS servers to support the remote management.  And this article describes how I did it.

When you login to a Server Core version of Windows Server you are greeted by a command prompt.  Enter ‘powershell’ as shown in Figure 1 to start up PowerShell.  Notice the PS is added in front of the C:\> prompt.

![start powershell from a Server Core command prompt][FIGURE1]
###### Figure 1, start powershell from a Server Core command prompt

## Install the Web-Server role

Next, install the web service, by entering ‘Install-WindowsFeature Web-Server’.  After pressing the enter key, you are shown the status, as in Figure 2.

![installing IIS on Server Core, 7, 7.5, 8, 8.5 and IIS 10][FIGURE2]
###### Figure 2, installing IIS on Server Core, 7, 7.5, 8, 8.5 and IIS 10

Once complete, you receive confirmation, as illustrated in Figure 3.

![How to install IIS on server core][FIGURE3]
###### Figure 3, How to install IIS on server core

## Install the Web-Mgmt-Service

The Management Service feature is required for remote management to work.  Enter ‘Install-WindowsFeature Web-Mgmt-Service’ and again, you get the status of the installation shown in Figure 4.

![How to install remote management on server core][FIGURE4]
###### Figure 4, How to install remote management on server core

Once installed, you receive confirmation and the status of the installation, Figure 5.  In all cases so far Exit Code has equaled Success.

![install web-management-service on server core IIS][FIGURE5]
###### Figure 5, install web-management-service on server core IIS

## Enable Remote Management

A registry setting is required to allow remote management.  Set the Data value of EnableRemoteManagement to 1, as seen in Figure 6.  This attribute is found in HKLM\SOFTWARE\Microsoft\WebManagement\Server.  You can open the registry editor by entering ‘regedit’ from the command prompt.  NOTE: if you make any mistakes and deleted or change something unintentionally, you may disable the server to a point where the only solution is to rebuild.  So take care when making changes.

![set EnableRemoteManagement to 1 to enable IIS remote management][FIGURE6]
###### Figure 6, set EnableRemoteManagement to 1 to enable IIS remote management

## Set the WMSVC service to start automatically

The WMSVC service is not running by default, so you need to manually start it.  It goes without writing, that it’s not optimal to login to a server and start a service after a reboot, so setting the service to start automatically is a preferred and logical step, unless you have a reason not to.

Figure 7 illustrates the ‘sc config WMSVC start=auto’ command that sets the Web Management service to start automatically.  After the installation and after running the ‘sc’ command for the first time, I need to start the service, also shown in Figure 7.

![how to set a windows service to start automatically, server core][FIGURE7]
###### Figure 7, how to set a windows service to start automatically, server core

***NOTE***:  You might need to create a Firewall rule to allow the connection on port 8172, for example.  In my environment I did not need to create firewall rule.  If you get an error while connecting to the remote IIS server core server, then check if the connection is being blocked by the firewall.  For example a PowerShell command like this ‘***netsh advfirewall firewall add rule name=”IIS Remote Management” dir=in action=allow service=WMSVC***’

## Create connection from Remote IIS Management Console

I wrote an article [here][LINK1] that explains how to add an IIS server to the IIS Management Console for remote management, see starting from Figure 7.  I setup 2 server core Windows Server 2012R2 servers running IIS 8.5 and added them to an Application Request Routing Server farm, just for fun, Figure 8.

![Server Core servers in a ARR server farm remotely managed, super coolness][FIGURE8]
###### Figure 8, Server Core servers in a ARR server farm remotely managed, super coolness

***NOTE***: This also worked with IIS 10 on Windows Server 2016, as seen in Figure 9.

![Windows Server 2016 and IIS 10 on Server Core][FIGURE9]
###### Figure 9, Windows Server 2016 and IIS 10 on Server Core

[FIGURE1]: ../images/2015/msdn-0573.png "Figure 1, start powershell from a Server Core command prompt"
[FIGURE2]: ../images/2015/msdn-0574.png "Figure 2, installing IIS on Server Core, 7, 7.5, 8, 8.5 and IIS 10"
[FIGURE3]: ../images/2015/msdn-0575.png "Figure 3, How to install IIS on server core"
[FIGURE4]: ../images/2015/msdn-0576.png "Figure 4, How to install remote management on server core"
[FIGURE5]: ../images/2015/msdn-0577.png "Figure 5, install web-management-service on server core IIS"
[FIGURE6]: ../images/2015/msdn-0578.png "Figure 6, set EnableRemoteManagement to 1 to enable IIS remote management"
[FIGURE7]: ../images/2015/msdn-0579.png "Figure 7, how to set a windows service to start automatically, server core"
[FIGURE8]: ../images/2015/msdn-0580.png "Figure 8, Server Core servers in a ARR server farm remotely managed, super coolness"
[FIGURE9]: ../images/2015/msdn-0581.png "Figure 9, Windows Server 2016 and IIS 10 on Server Core"

[LINK1]: ../2014/2014-08-configure-remote-iis-administration-for-iis-web-sites.md
