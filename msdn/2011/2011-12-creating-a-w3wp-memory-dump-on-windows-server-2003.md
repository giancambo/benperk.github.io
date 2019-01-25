# Creating a W3WP Memory dump on Windows Server 2003

On a web server with multiple application pools, there most likely will be multiple W3WP processes.  Therefore, the first action to take when creating a memory dump of a W3WP worker process is to find which one you need to get the dump of. 

I wrote a short article on how to get a worker process memory dump for IIS 6, IIS 7 and IIS 7.5 [here][LINK1].

Specifically for IIS 6, open IIS 6 Management Console and select the web site which is having the problem, in this example, let’s assume the “Default Web Site”.  Right-click the “Default Web Site” and then select Properties as shown in Figure 1.

![popup menu for Default Web Site][FIGURE1]
###### Figure 1, popup menu for Default Web Site

Selecting Properties will open the Default Web Site Properties window as shown in Figure 2.  Select the Home Directory tab and you will find the application pool which is being used for the web site selected in the Application pool dropdown, in this case DefaultAppPool.

![Default Web Site Properties window][FIGURE2]
###### Figure 2, Default Web Site Properties window

Next, use the iisapp.vbs script to show a list of all running application pools with associated PID, as shown in Figure 3:

![using the iisapp.vbs script to find PID and AppPoolId][FIGURE3]
###### Figure 3, using the iisapp.vbs script to find PID and AppPoolId

You can find the same in Task Manager, but unfortunately, it will not show you the name of the application pool.  See the following Figure 4.

![Task Manager w3wp.exe process and PID without associated application pool name][FIGURE4]
###### Figure 4, Task Manager w3wp.exe process and PID without associated application pool name

Once you have the PID of the w3wp.exe worker process, download ProcDump from [here][LINK2], and execute the "example" command line shown in Figure 5.

![ProcDump command example][FIGURE5]
###### Figure 5, ProcDump command example

Note: The command shown in figure 5 will create a 32bit memory dump, ProcDump creates a 32bit dump by default.  If your worker process and OS are 64bit you need to add the -64 parameter to capture the dump correctly, simply place -64 before the –ma, as the following command shows:

```C:\>procdump –64 –ma –o 2028 w3wp-2028.dmp```

Another example:

If you are experiencing high CPU utilization and want to create the memory dump once the CPU has exceed 90 percent for 15 seconds, your command would be similar to the following:

```C:\>procdump –ma –c 90 –s 15 –o 2028 w3wp-2028.dmp```

Closing:

There are numerous parameters which can be passed to ProcDump and are discussed in detail on the ProcDump download [page][LINK3].

Once the dump is created you can analyze it using [WinDbg][LINK4] and find the reason for its incorrect behavior.

[LINK1]: 2011-12-finding-the-w3wp-worker-process-pid-and-associated-id.md
[LINK2]: http://technet.microsoft.com/en-us/sysinternals/dd996900
[LINK3]: http://technet.microsoft.com/en-us/sysinternals/dd996900
[LINK4]: http://msdn.microsoft.com/en-us/windows/hardware/gg463009

[FIGURE1]: ../images/2011/msdn-0039.png "Figure 1, popup menu for Default Web Site"
[FIGURE2]: ../images/2011/msdn-0040.png "Figure 2, Default Web Site Properties window"
[FIGURE3]: ../images/2011/msdn-0041.png "Figure 3, using the iisapp.vbs script to find PID and AppPoolId"
[FIGURE4]: ../images/2011/msdn-0042.png "Figure 4, Task Manager w3wp.exe process and PID without associated application pool name"
[FIGURE5]: ../images/2011/msdn-0043.png "Figure 5, ProcDump command example"
