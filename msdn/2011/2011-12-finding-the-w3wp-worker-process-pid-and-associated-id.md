# Finding the W3WP worker process PID and associated ID

When you have multiple websites running under different application pools on the same server, you may need to trouble shoot a specific worker process.
On Windows 2003 and Internet Information Services 6 (IIS6), I use the iisapp.vbs script as shown in following command, as shown in Figure 1:

![Find AppPoolId using IIS 6][FIGURE1]
###### Figure 1, Find AppPoolId using IIS 6

On Windows 2008 and Internet Information Services 7 (IIS 7 or IIS 7.5) or Windows 2012 (IIS 8 or IIS 8.5), I use the appcmd application passing it the wps parameter as displayed below, as shown in Figure 2:

![Find applicationPoolId using IIS 7, IIS 7.5, IIS 8 or IIS 8.5][FIGURE2]
###### Figure 2, Find applicationPoolId using IIS 7, IIS 7.5, IIS 8 or IIS 8.5

Then, if required I can use, for example, [Debug Diag][LINK1] or [ProcDump]{LINK2] to get a memory dump of the worker process which is experiencing the problem.

Then use the memory dump to analyze and find the root cause of the problem.

[FIGURE1]: ../images/2011/msdn-0037.png "Figure 1, Find AppPoolId using IIS 6"
[FIGURE2]: ../images/2011/msdn-0038.png "Figure 2, Find applicationPoolId using IIS 7, IIS 7.5, IIS 8 or IIS 8.5"

[LINK1]: http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=24370
[LINK2]: http://technet.microsoft.com/en-us/sysinternals/dd996900
