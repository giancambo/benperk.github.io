# Install Failed Request Tracing on IIS 8.5 Server Core

To setup Failed Request Tracing on an IIS 8.5 Server Core server, follow these steps.

+ Start PowerShell
+ Install the Web-Http-Tracing Role, using Install-WindowsFeature
+ Configure the rule via a Remote Management IIS connection
 + How to configure an IIS Server Core server for remote management

## Start PowerShell

When you login to a Service Core installation of Windows Server 2012 R2 you will see only a command prompt as illustrated in Figure 1.  Enter ‘powershell’ and press the enter key.

![start powershell on Server Core 2012 R2][FIGURE1]
###### Figure 1, start powershell on Server Core 2012 R2

Notice the PS is added to the command line after starting PowerShell.  This confirms that you are in PowerShell mode and that you can now run PowerShell cmdlets.

## Install the Web-Http-Tracing role
I wrote some articles about installing Failed Request Tracing from within the IIS Management Console and on Azure here:

+ [Enabling Failed Request Logging on for Web App on Azure][LINK1]
+ [Enable and Activate Failed Request Tracing Rules][LINK2]

To install it on Server Core, execute the following command.  As shown in Figure 2, the status is provided during the installation.

```Install-WindowsFeature Web-Http-Tracing```

![Server Core Web-Http-Tracing installation status][FIGURE2]
###### Figure 2, Server Core Web-Http-Tracing installation status

Once the role has been successfully installed, you should receive a response similar to that shown in Figure 3.

![Server Core Web-Http-Tracing installed successfully][FIGURE3]
###### Figure 3, Server Core Web-Http-Tracing installed successfully

## Configure the Failed Request Tracing rule via Remote Management

You can setup a Failed Request Tracing rule using AppCmd, shown [here][LINK3], or PowerShell commands.  If you do this on the IIS Server Core server itself, then you do not need to authorize or configure Remote Management on the IIS Server Core IIS server as described here.

I have decided to configure the Failed Request Tracing rule from the IIS Management Console and therefore, the IIS Server Core server needs to be configured to support Remote Management prior to continuing on with the remaining steps.

Additionally, I wrote a previous article, [here][LINK4], which discusses how to configure remote management of an IIS server within the IIS Management Console.  That article walks you through adding the IIS Server Core server to an IIS Management Console existing on a different server.  If, for example, I have 5 IIS Server Core servers, I could manage them all from a single IIS server that has the management console installed.

Once you have completed the steps in both of the articles mentioned above, 1. Configured the Server Core IIS server to support Remote Administration and 2. Add the Server Core IIS server into the IIS Management Console on another server for administration, open the IIS manager and expand the IIS Server Core server you want to create the Failed Request Tracing rule onto, as shown in Figure 4.

![Configure Failed Request Tracing on a Server Core IIS server via Remote Administration][FIGURE4]
###### Figure 4, Configure Failed Request Tracing on a Server Core IIS server via Remote Administration

Now, follow the steps [here][LINK5] to create the rule.

[FIGURE1]: ../images/2015/msdn-0500.png "Figure 1, start powershell on Server Core 2012 R2"
[FIGURE2]: ../images/2015/msdn-0500.png "Figure 2, Server Core Web-Http-Tracing installation status"
[FIGURE3]: ../images/2015/msdn-0500.png "Figure 3, Server Core Web-Http-Tracing installed successfully"
[FIGURE4]: ../images/2015/msdn-0500.png "Figure 4, Configure Failed Request Tracing on a Server Core IIS server via Remote Administration"

[LINK1]: ../2013/2013-07-enabling-failed-request-logging-on-a-windows-azure-web-site.md
[LINK2]: ../2012/2012-01-enable-and-activate-failed-request-tracing-rules.md
[LINK3]: https://technet.microsoft.com/en-us/library/cc725948(v=ws.10).aspx
[LINK4]: ../2014/2014-08-configure-remote-iis-administration-for-iis-web-sites.md
[LINK5]: ../2012/2012-01-enable-and-activate-failed-request-tracing-rules.md
