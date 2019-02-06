# How to capture a memory dump of a W3WP process that consumes too much CPU

If you want to find out why a W3WP worker process is consuming so much CPU, you can use [ProcDump][LINK1].  You might also use [LogParser][LINK2] to look at the time-taken field, but to get to the root cause you will need to get deep into the source.  Before you can get a memory dump, you need the PID of the worker process.  You can get the PID in a number of ways.  Below I illustrate using either PowerShell or AppCmd, for example.

[Here][LINK3] is a previous article I wrote that covers how to get the PID of a worker process in PowerShell.  Figure 1 illustrated the specific PowerShell command.

![finding the W3WP PID using PowerShell][FIGURE1]
###### Figure 1, finding the W3WP PID using PowerShell

You can also find the PID using AppCmd, as shown in Figure 2.  I wrote a more detailed article about how to do this [here][LINK4].

![finding the W3WP PID using AppCmd][FIGURE2]
###### Figure 2, finding the W3WP PID using AppCmd

When you have found the PID (Process ID), you can use it as a parameter of the ProcDump command.  Enter this ProcDump command, shown in Figure 3, to capture a 64-bit full memory dump when the CPU consumption is greater than 90% for 5 seconds:

***C:\>procdump -64 -ma -c 90 -s 5 -o PID c:\PID.dmp***

![a ProcDump command to capture CPU consumption][FIGURE3]
###### Figure 3, a ProcDump command to capture CPU consumption

Then you can use tools like [Debug Diagnostic][LINK5] or [WinDbg][LINK6] to analyze the memory dump and find out what is consuming the CPU.

[FIGURE1]: ../images/2012/msdn-0194.png "Figure 1, finding the W3WP PID using PowerShell"
[FIGURE2]: ../images/2012/msdn-0195.png "Figure 2, finding the W3WP PID using AppCmd"
[FIGURE3]: ../images/2012/msdn-0196.png "Figure 3, a ProcDump command to capture CPU consumption"

[LINK1]: http://technet.microsoft.com/en-us/sysinternals/dd996900
[LINK2]: http://www.microsoft.com/en-us/download/details.aspx?id=24659
[LINK3]: ../2012/2012-06-using-powershell-3-0-on-windows-8-to-find-the-process-id-pid-of-an-application-pool.md
[LINK4]: ../2012/2012-02-creating-a-w3wp-memory-dump-on-windows-server-2008-r2.md
[LINK5]: http://www.microsoft.com/en-us/download/details.aspx?id=26798
[LINK6]: http://msdn.microsoft.com/en-us/windows/hardware/gg463009.aspx
