# Create a memory dump when the W3WP process terminates using Procdump

There are times when you need to capture a W3WP memory dump when the process stops responding.  However, the process might stop responding in the middle of the night or early morning which is not the best time to manually take a memory dump.

You can configure Procdump to execute and capture a memory dump when the W3WP process stops responding or terminates.

To do this, first download Procdump from [here][LINK1].

Copy Procdump to the server where you want to capture the memory dump and place it in, for example, the c:\temp directory.
Then open Task Manager to find the PID of the W3WP worker process that is behaving badly, as shown in Figure 1.

![Use Task Manager to find PID of W3WP process][FIGURE1]
###### Figure 1, Use Task Manager to find PID of W3WP process

Next open a command window as an Administrator and execute the command shown in Figure 2.

![Procdump command to capture a memory dump when process terminates][FIGURE2]
###### Figure 2, Procdump command to capture a memory dump when process terminates

The command will remain open until an Unhandled exception happens within the process being monitored.  Figure 3 shows that there were a number of exceptions, but they were handled ok, however as soon as an exception happened that terminated to the process, a memory dump of that process was created.

![Procdump automatically capturing a memory dump when process unexpected termination][FIGURE3]
###### Figure 3, Procdump automatically capturing a memory dump when process unexpected termination

You may want to run the command a number of times, like 3-5 just to make sure.  Then you can compare the dumps and see if the Unhandled exceptions causing the process to crash was the same all the time or if there are multiple issues.

***NOTE***: Standard or scheduled recycling of the worker process will likely trigger this memory dump process.  Therefore, when performing this activity, consider disabling any scheduled recycle of the worker process / application pool.

[FIGURE1]: ../images/2013/msdn-0237.png "Figure 1, Use Task Manager to find PID of W3WP process"
[FIGURE2]: ../images/2013/msdn-0238.png "Figure 2, Procdump command to capture a memory dump when process terminates"
[FIGURE3]: ../images/2013/msdn-0239.png "Figure 3, Procdump automatically capturing a memory dump when process unexpected termination"

[LINK1]: http://technet.microsoft.com/en-us/sysinternals/dd996900.aspx
