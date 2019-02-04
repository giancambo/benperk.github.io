# Using PowerShell 3.0 on Windows 8 to find the Process Id (PID) of an Application Pool

With the release of Windows 8 Server comes a new version of PowerShell.  You can check which version of PowerShell you are using by executing the command show in Figure 1.

![Which version of PowerShell am I using][FIGURE1]
###### Figure 1, Which version of PowerShell am I using

To get a list of application pools existing on the web server enter the following shown in Figure 2.

![How to list the application pools using PowerShell 3.0][FIGURE2]
###### Figure 2, How to list the application pools using PowerShell 3.0

To get the process id of the application pool, execute the command shown in Figure 3.

![how to get the process id (PID) of an application pool using PowerShell 3.0][FIGURE3]
###### Figure 1, how to get the process id (PID) of an application pool using PowerShell 3.0

Then, if required you can use that PID in combination with [ProcDump][LINK1] to create a memory dump for analysis.

[LINK1]: http://technet.microsoft.com/en-us/sysinternals/dd996900.aspx

[FIGURE1]: ../images/2012/msdn-0137.png "Figure 1, Which version of PowerShell am I using"
[FIGURE2]: ../images/2012/msdn-0138.png "Figure 2, How to list the application pools using PowerShell 3.0"
[FIGURE3]: ../images/2012/msdn-0139.png "Figure 3, how to get the process id (PID) of an application pool using PowerShell 3.0"
