# Using LogParser to analyze the EVENTLOG.XML, Azure App Service Web App

If you have an active Azure Web App and you experience problems or want to do some checks on what might the problem, there is a file called EVENTLOG.XML which contain exceptions and information that may be helpful.  The file is located in the /LogFiles directory on your Web App.  Figure 1 illustrates how that might look when using an FTP client tool like FileZilla.

![EVENTLOG.XML file for an Azure App Service Web App ][FIGURE1]
###### Figure 1, EVENTLOG.XML file for an Azure App Service Web App 

As this file can be large, you might consider using a tool like LOGPARSER to perform the analysis as manual analysis might cause your eyes to hurt.  I wrote a troubleshooting article about this tool here, it focuses on IIS logs, but you can use it for checking XML files as well.

The selectable columns are:

+ Name
+ EventID
+ Level
+ Task
+ Keywords
+ ServerTime
+ EventRecordID
+ Channel
+ Computer
+ Data

An example LogParser query you can use is like the following, with the result shown in Figure 2.

```LOGPARSER “SELECT Name, EventId, Count(*) FROM EVENTLOG.XML GROUP BY Name, EventId” i:xml```

![Analyzing Azure App Service Web App EVENTLOG.XML file using LogParser][FIGURE2]
###### Figure 2, Analyzing Azure App Service Web App EVENTLOG.XML file using LogParser

If you see an abundant number of exceptions in the file, you can then write another query to look at the data for these entries.  For example, a query to see what the Data is for all the ASP.NET entries where EventId = 1309, as shown below and in Figure 3.

```LOGPARSER “SELECT Name, Data FROM EVENTLOG.XML WHERE EventId=’1309’” i:xml```

![Logparser query for Azure App Service Web App EVENTLOG.XML file analysis][FIGURE3]
###### Figure 3, Logparser query for Azure App Service Web App EVENTLOG.XML file analysis

This can help isolate and ultimately find the reason for poor performance or unexpected performance on your Web App.

I use LogParser Lizard often to perform queries, which you can use in this context too.  Simply select XML Input Format as shown in Figure 4, and execute the same queries.

![using LogParser Lizard to view Azure App Service Web App EVENTLOG.XML logs][FIGURE4]
###### Figure 4, using LogParser Lizard to view Azure App Service Web App EVENTLOG.XML logs

You can also manually view the event logs within the KUDU console, as I describe [here][LINK1].  You need to add /Support to the end of your KUDU URL or select the Support option from the Tools menu, as shown in Figure 5.

![using KUDU to view Azure App Service Web App event logs logs][FIGURE5]
###### Figure 5, using KUDU to view Azure App Service Web App event logs logs

Then select the Analyze feature then Event Viewer, as shown in Figure 6.  The contents displayed here are the same that are in the EVENTLOG.XML file.

![using KUDU to view Azure App Service Web App event logs logs][FIGURE6]
###### Figure 6, using KUDU to view Azure App Service Web App event logs logs

Then you can scroll through the entries manually.

[FIGURE1]: ../images/2014/msdn-0452.png "Figure 1, EVENTLOG.XML file for an Azure App Service Web App "
[FIGURE2]: ../images/2014/msdn-0453.png "Figure 2, Analyzing Azure App Service Web App EVENTLOG.XML file using LogParser"
[FIGURE3]: ../images/2014/msdn-0454.png "Figure 3, Logparser query for Azure App Service Web App EVENTLOG.XML file analysis"
[FIGURE4]: ../images/2014/msdn-0455.png "Figure 4, using LogParser Lizard to view Azure App Service Web App EVENTLOG.XML logs"
[FIGURE5]: ../images/2014/msdn-0456.png "Figure 5, using KUDU to view Azure App Service Web App event logs logs"
[FIGURE6]: ../images/2014/msdn-0457.png "Figure 6, using KUDU to view Azure App Service Web App event logs logs"

[LINK1]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
