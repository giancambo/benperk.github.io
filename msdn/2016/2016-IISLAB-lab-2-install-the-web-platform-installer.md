# Lab 2: Install the Web Platform Installer

In many of my blog posts I have recommended using the Web Platform Installer (WPI) to install IIS modules and .NET Frameworks. In this lab you will:

+ Install the Web Platform Installer
+ Use the Web Platform Installer to install the .Net Framework and the ASP.NET MVC dependencies
+ ***NOTE*** In case you have a problem with the Web Platform Installer on ***IIS 10***, use the Server Manager, see Figure 7 at the bottom.

## Install the Web Platform Installer

When you open the IIS Management Console you might get prompted by the window shown in Figure 1. The windows asks if you would like to get started with WPI. Select ‚Yes‘ if you see this window.

![how to install Web Platform Installer on IIS, window][FIGURE1]
###### Figure 1, how to install Web Platform Installer on IIS, window

If the windows does not come up, check on the right-hand side of the IIS Management Console, on the action pane and click on ‚Get New Web Platform Components, as shown in Figure 2.

![get new web platform components][FIGURE2]
###### Figure 2, get new web platform components

Regardless of the method, either a popup or clicking the link on the Actions pane, you will be routed [here][LINK1]. Select the Free download button and run the installation to continue, as shown in Figure 3.

![installing Web Platform Installer][FIGURE3]
###### Figure 3, installing Web Platform Installer

Once installed, search for ‚.net 4.5‘ and install the following components, similar to that shown in Figure 4.

+ IIS: .NET Extensibility 4.5
+ IIS ASP.NET 4.5
+ Microsoft .NET Framework 4.5.2

![components for ASP.NET MVC CSharpGuitarBugs example][FIGURE4]
###### Figure 4, components for ASP.NET MVC CSharpGuitarBugs example

During the installation process, you will see a status similar to that shown in Figure 5.

![Web Platform Installer installation status][FIGURE5]
###### Figure 5, Web Platform Installer installation status

Once everything is installed, restart the server if required and run the web site. You will see the default CSharpGuitarBugs web site.

Lastly, notice that there is now a Web Platform Installer icon in the Feature pane of the IIS Management Console, Figure 6. If it is not present, you may need to to close and re-open the IIS Management Console. Access WPI from that link from now on.

![the web platform installer icon in the IIS management console][FIGURE6]
###### Figure 6, the web platform installer icon in the IIS management console

![if the web platform installer doesn't work, use Server Manager to install the ASP.NET role][FIGURE7]
###### Figure 7, if the web platform installer doesn't work, use Server Manager to install the ASP.NET role

[FIGURE1]: ../images/2016/msdn-0669.png "Figure 1, how to install Web Platform Installer on IIS, window"
[FIGURE2]: ../images/2016/msdn-0670.png "Figure 2, get new web platform components"
[FIGURE3]: ../images/2016/msdn-0671.png "Figure 3, installing Web Platform Installer"
[FIGURE4]: ../images/2016/msdn-0672.png "Figure 4, components for ASP.NET MVC CSharpGuitarBugs example"
[FIGURE5]: ../images/2016/msdn-0673.png "Figure 5, Web Platform Installer installation status"
[FIGURE6]: ../images/2016/msdn-0674.png "Figure 6, the web platform installer icon in the IIS management console"
[FIGURE7]: ../images/2016/msdn-0675.png "Figure 7, MSDN"

[LINK1]: http://www.microsoft.com/web/downloads/platform.aspx
