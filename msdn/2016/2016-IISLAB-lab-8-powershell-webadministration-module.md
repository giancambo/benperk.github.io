# Lab 8: PowerShell WebAdministration Module

## Prerequisites

+ Some understanding of PowerShell
+ Read about the Web Administration module
  + http://technet.microsoft.com/en-us/library/ee909471(v=WS.10).aspx
  + http://technet.microsoft.com/en-us/library/ee790599.aspx
+ There are some new PowerShell IIS cmdlets for IIS 10, see them [here][LINK1].  What’s new in IIS 10?

## Setup

+ Install and Configure CSharpGuitarBugs on an IIS server as described in [Lab 1][LINK2]

## Lab

Review the features available in the PowerShell WebAdmnistration module

1.  Open PowerShell as an Administrator

![PowerShell WebAdministration Module][FIGURE1]
###### Figure 1, PowerShell WebAdministration Module

2.  Execute: Get-ExecutionPolicy

![PowerShell WebAdministration Module][FIGURE2]
###### Figure 2, PowerShell WebAdministration Module

3.  Execute: Set-ExecutionPolicy Unrestricted

![PowerShell WebAdministration Module][FIGURE3]
###### Figure 3, PowerShell WebAdministration Module

4.  Execute: import-module WebAdministration

![PowerShell WebAdministration Module][FIGURE4]
###### Figure 4, PowerShell WebAdministration Module

5.  Execute: cd IIS:

![PowerShell WebAdministration Module][FIGURE5]
###### Figure 5, PowerShell WebAdministration Module

6.  Execute: cd AppPools -> dir

  a. Execute: Stop-WebAppPool -name "CSharpGuitarBugs" -> dir, notice that the State of the application pool shows as stopped. You can also see the same in IIS.

![PowerShell WebAdministration Module][FIGURE6]
###### Figure 6, PowerShell WebAdministration Module

![PowerShell WebAdministration Module][FIGURE7]
###### Figure 7, PowerShell WebAdministration Module

  b. Execute: Start-WebAppPool -name "CSharpGuitarBugs" -> dir, which starts the application pool again.

![PowerShell WebAdministration Module][FIGURE8]
###### Figure 8, PowerShell WebAdministration Module

7.  Execute: cd CSharpGuitarBugs -> dir, then access the CSharpGuitarBugs website and click the View By Manufacturer link. While the request is running, execute: Get-WebRequest

![PowerShell WebAdministration Module][FIGURE9]
###### Figure 9, PowerShell WebAdministration Module

8.  Change directory to the WorkerProcess directory and enter dir, what can you learn from the results presented?

![PowerShell WebAdministration Module][FIGURE10]
###### Figure 10, PowerShell WebAdministration Module

9.  Navigate back up to the root directory and enter dir -> cd Sites -> dir and review the information provided, where can you find the same in the IIS management console? Can you find the same in the IIS configuration file? TIP: in the applicationHost.config file?

![PowerShell WebAdministration Module][FIGURE11]
###### Figure 11, PowerShell WebAdministration Module

10. Navigate back to the root directory and execute: Stop-Website –name “CSharpGuitarBugs”

![PowerShell WebAdministration Module][FIGURE12]
###### Figure 12, PowerShell WebAdministration Module

![PowerShell WebAdministration Module][FIGURE13]
###### Figure 13, PowerShell WebAdministration Module

11. Restart the website by executing: Start-Website –name “CSharpGuitarBugs”

![PowerShell WebAdministration Module][FIGURE14]
###### Figure 14, PowerShell WebAdministration Module

12. Execute: Get-Command –pssnapin WebAdministration or for a specific cmdlet execute: get-help <CmdLetName> for a list of all available methods.

![PowerShell WebAdministration Module][FIGURE15]
###### Figure 15, PowerShell WebAdministration Module

![PowerShell WebAdministration Module][FIGURE16]
###### Figure 16, PowerShell WebAdministration Module




[FIGURE1]: ../images/2016/msdn-0751.png "Figure 1, PowerShell WebAdministration Module"
[FIGURE2]: ../images/2016/msdn-0752.png "Figure 2, PowerShell WebAdministration Module"
[FIGURE3]: ../images/2016/msdn-0753.png "Figure 3, PowerShell WebAdministration Module"
[FIGURE4]: ../images/2016/msdn-0754.png "Figure 4, PowerShell WebAdministration Module"
[FIGURE5]: ../images/2016/msdn-0755.png "Figure 5, PowerShell WebAdministration Module"
[FIGURE6]: ../images/2016/msdn-0756.png "Figure 6, PowerShell WebAdministration Module"
[FIGURE7]: ../images/2016/msdn-0757.png "Figure 7, PowerShell WebAdministration Module"
[FIGURE8]: ../images/2016/msdn-0758.png "Figure 8, PowerShell WebAdministration Module"
[FIGURE9]: ../images/2016/msdn-0759.png "Figure 9, PowerShell WebAdministration Module"
[FIGURE10]: ../images/2016/msdn-0760.png "Figure 10, PowerShell WebAdministration Module"
[FIGURE11]: ../images/2016/msdn-0761.png "Figure 11, PowerShell WebAdministration Module"
[FIGURE12]: ../images/2016/msdn-0762.png "Figure 12, PowerShell WebAdministration Module"
[FIGURE13]: ../images/2016/msdn-0763.png "Figure 13, PowerShell WebAdministration Module"
[FIGURE14]: ../images/2016/msdn-0764.png "Figure 14, PowerShell WebAdministration Module"
[FIGURE15]: ../images/2016/msdn-0764.png "Figure 15, PowerShell WebAdministration Module"
[FIGURE16]: ../images/2016/msdn-0766.png "Figure 16, PowerShell WebAdministration Module"

[LINK1]: http://blogs.iis.net/bariscaglar/iisadministration-powershell-cmdlets-new-feature-in-windows-10-server-2016
[LINK2]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
