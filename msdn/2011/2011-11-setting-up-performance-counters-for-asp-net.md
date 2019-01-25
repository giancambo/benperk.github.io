# Setting up performance counters for ASP.NET

Troubleshooting a performance problem can be very complicated.  Unfortunately, in many cases you prepare and begin learning about how to fix a performance problem when it is happening or just after a performance problem.  The fact is, you need to monitor your system and gather statistics on what is considered “standard operating performance” before a performance problem occurs.  I mean the simple question is, “how do you know there is a performance problem if you have nothing to compare it against?”

Some good performance counters to begin setting a performance base line are shown in Table 1 and in Figure 1:

| Performance Object | Performance Counter | Short Description |
| ------------------ | ------------------- | ----------------- |
| ASP.NET | Application Restarts | Number of times the application has been restarted during the web server's lifetime. |
| ASP.NET | Requests Queued | The number of requests waiting to be processed. |
| ASP.NET | Worker Process Restarts | Number of times a worker process has restarted on the machine. |
| ASP.NET Applications | Errors Total | Total number of errors occurred. |
| ASP.NET Applications | Requests/Sec | The number of requests executed per second. |
| ASP.NET Applications | Pipeline Instance Count | Number of active pipeline instances. |
| Processor | % Processor Time | % Processor Time is the percentage of elapsed time that the processor  spends to execute a non-Idle thread |
| .NET CLR Exceptions | # of Exceptions Thrown | Displays the total number of exceptions thrown since the start of the application. |
| System | Context Switches/sec | Context Switches/sec is the combined rate at which all processors on the computer are switched from one thread to another. |
###### Table 1, sample performance monitor counters

![a possible performance counter setup for tracking performance][FIGURE1]
###### Figure 1, a possible performance counter setup for tracking performance

Significant capabilities can be found in the Performance Monitor tool.  This following instructions represent a manual implementation which may or may not fit into your requirements.  For detailed information about Data Collectors, System Diagnostics, System Performance and Reporting, please review the Technet document found [here][LINK1].

## Setting up a manual performance monitor

To set this up click on Start -> Run -> “Perfmon”, this will open the Performance Monitor tool as shown in Figure 2.

![Performance Monitor version 6.1.7601][FIGURE2]
###### Figure 2, Performance Monitor version 6.1.7601

Click on the Green + (plus icon) and you are presented with a modal window which enables you to add Performance Counters as shown in Figure 3.  Refer to Table 1 for some recommended performance monitor counters, their descriptions and associated objects.

![adding counters to the Performance Monitor][FIGURE3]
###### Figure 3, adding counters to the Performance Monitor

You can save the Performance Monitor by right-clicking the performance monitor graph -> Save Settings As…, as shown in Figure 4.  This will let you save the performance log as an HTML file.  When you open the file it lets you scroll through counters to view the Last, Average, Minimum, Maximum and Duration for each counter.  As well, you can click on different points of the graph to see what the values were at any given point.

![save the performance counters to an HTML file][FIGURE4]
###### Figure 4, save the performance counters to an HTML file

## Conslusion

System performance monitoring is a very important aspect, not only in regards to past and present performance metrics.  It is also possible to use the performance logs such as Requests/Sec or Sessions Total to show a possible increase in the usage of the system.  This graph could then be used for planning future growth of a system which may result in hardware upgrades or system optimization programs.

Within large scale systems where the complexity exceeds the capacity of a single mind, metrics are a very powerful tool which can bridge that gap, I recommend that you create and use them.

[LINK1]: http://technet.microsoft.com/en-us/library/cc749154.aspx

[FIGURE1]: ../images/2011/msdn-0033.png "Figure 1, a possible performance counter setup for tracking performance"
[FIGURE2]: ../images/2011/msdn-0034.png "Figure 2, Performance Monitor version 6.1.7601"
[FIGURE3]: ../images/2011/msdn-0035.png "Figure 3, adding counters to the Performance Monitor"
[FIGURE4]: ../images/2011/msdn-0036.png "Figure 4, save the performance counters to an HTML file"

