# Create a W3WP memory dump, quick and easy

I have written a number of articles on how to create memory dumps for troubleshooting W3WP process hangs, crashes or memory consumption issues.

+ [Create a memory dump when the W3WP process terminates using Procdump][LINK1]
+ [Creating a W3WP Memory dump on Windows Server 2003][LINK2]
+ [Creating a W3WP Memory dump on Windows Server 2008 R2][LINK3]
+ [Configure Debug Diagnostic 2.0 to create a memory dump when a specific exception is thrown][LINK4]
+ [Using Procdump and Failed Request Tracing to capture a memory dump][LINK5]
+ [How to capture a memory dump of a W3WP process that consumes too much CPU][LINK6]
+ [New ProcDump version 5.0 released, using the –f parameter][LINK7]
+ [Finding the W3WP worker process PID and associated ID][LINK8]
+ [Debugging a hung application with WinDbg][LINK9]
+ [Why does my !runaway command not work][LINK10]
+ [Create a memory dump for your slow performing Web App][LINK11]

Those are all good techniques, IMO, but sometimes, I or you just need a quick and easy memory dump to either give to support or to analyze yourself.  Again, IMO, this method is best for when CPU is 100% (or a high amount) or a request is hanging or taking some time to respond.  In the first example, simply open the Task Manager, and when the CPU consumption for the W3WP process is higher than you would expect, right-click on the process, and select Create dump file, as shown in Figure 1.

![get a memory dump quick, fast and easy][FIGURE1]
###### Figure 1, get a memory dump quick, fast and easy

You should then see the following, while the memory dump is being created, Figure 2.

![the process memory dump is being created][FIGURE2]
###### Figure 2, the process memory dump is being created

Then, as shown in Figure 3, once created the location of the DMP file is provided, so you can analyze it or send it on to support.

![where the memory dump is written][FIGURE3]
###### Figure 3, where the memory dump is written

The only difference between the capture of a memory dump for a process consuming too much CPU and a hang is that you need to make sure the hung request is running within the process when the memory dump is taken.

***VERY IMPORTANT***:  The hanging request must be running in the process when the dump is taken

***I’ll mention that point again because it is very important.  When you have a hung situation, ***make sure the request which is hanging is running within the process when the memory dump is taken.***  If this does not happen, then the memory dump would not contain the problem and would either waste some time or lead the person analyzing the dump to wrong conclusions and suggest non-applicable resolution paths/options.

[FIGURE1]: ../images/2015/msdn-0562.png "Figure 1, get a memory dump quick, fast and easy"
[FIGURE2]: ../images/2015/msdn-0563.png "Figure 2, the process memory dump is being created"
[FIGURE3]: ../images/2015/msdn-0564.png "Figure 3, where the memory dump is written"

[LINK1]: ../2013/2013-06-create-a-memory-dump-when-the-w3wp-process-terminates-using-procdump.md
[LINK2]: ../2011/2011-12-creating-a-w3wp-memory-dump-on-windows-server-2003.md
[LINK3]: ../2012/2012-02-creating-a-w3wp-memory-dump-on-windows-server-2008-r2.md
[LINK4]: ../2014/2014-04-configure-debug-diagnostic-2-0-to-create-a-memory-dump-when-a-specific-exception-is-thrown.md
[LINK5]: ../2013/2013-12-using-procdump-and-failed-request-tracing-to-capture-a-memory-dump.md
[LINK6]: ../2012/2012-12-how-to-capture-a-memory-dump-of-a-w3wp-process-that-consumes-too-much-cpu.md
[LINK7]: ../2012/2012-10-new-procdump-version-5-0-released-using-the-f-parameter.md
[LINK8]: ../2011/2011-12-finding-the-w3wp-worker-process-pid-and-associated-id.md
[LINK9]: ../2013/2013-01-debugging-a-hung-application-with-windbg.md
[LINK10]: ../2012/2012-07-why-does-my-runaway-command-not-work.md
[LINK11]: ../../waws/2015/2015-07-create-a-memory-dump-for-your-slow-performing-web-app.md
