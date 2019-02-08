# Some tips for troubleshooting 503 HTTP Status codes

503’s are challenging HTTP status codes to resolve.  Sometimes they happen what appears to be random and other times they are shutting down your entire website.

When you see a 503 in your HTTP Error log or IIS Logs randomly I like to recommend that the default setting of the application pool recycle be changed.  The default setting is 1740 minutes which is 29 hours.  Over time, this recycle may happen during your peak business hour, minute or second.  Figure 1 illustrates the application pool advanced setting.

+ HTTP Error log – C:\windows\system32\logfiles\httperr
+ IIS Logs – C:\inetpub\logs\logfiles\w3svc*

![Application Pool default recycle setting = 1740 minutes][FIGURE1]
###### Figure 1, Application Pool default recycle setting = 1740 minutes

By setting the Regular Time Interval to 0, the automated recycle would be disabled.  You could then decide which time you may want to recycle the application pool, for example 03:00.  Or if you are in a web farm, you can stagger the recycling.

If the 503 is happening all the time and you cannot access the web page at all, check the state of the application pool and make sure it is running, see the Figure 2.  Notice that the status is stopped and there is a stop button on the name.  Restart it and attempt to access you site.  Sometimes this is the reason, but if it crashes again, you know there is something else.

Always check in the Application event logs for Errors, this will likely show you which module is causing the issue.  If the module is not needed then you can remove it from the configuration via the Modules feature.

![a stopped application pool][FIGURE2]
###### Figure 2, a stopped application pool

If the module is required, perhaps it is missing a dependency.  You can use a tool called Dependency Walker to see if there are some missing dependencies.

Lastly, check which .NET Framework version is associated to the application pool that is crashing with the 503 HTTP Status.  Then navigate to the path where that .NET version is installed.  For example:

C:\windows\Microsoft.NET\Framework64\v4.0.30319

Check for any files which have a recent Date Modified.  For example the Aspnet.config file, if it has been modified check the contents and compare it to an older version or a version from another server.

Do the same in the C:\windows\Microsoft.NET\Framework64\v4.0.30319\Config directory.

503’s and 500’s are some of the hardest to troubleshoot.  However, it is possible.  Good luck!

***UPDATE JUN-18-2013***

I was trying to get one of my test websites to run and I kept getting a Service Unavailable HTTP Error 503: The service is unavailable response from IIS.  I restarted the application pool, however, each time I accessed the website the application pool would stop.  Since the process would not start I could not get a memory dump because there was no PID.  This means I need to go 1 level up the ladder to the Windows Process Activation Service (WAS) to see what is happening there.  The best place to see what is going on with this service is in the System Event Log.  I looked and sure enough, I found the event and the reason.  I found event id 5059, 5057 and 5021.  Figure 3 shows the WAS event logs and the description of event id 5021.

![WAS event id 5021][FIGURE1]
###### Figure 3, WAS event id 5021

Looking at the application pool identity I saw that I was using a custom account and the password had expired.  I reset the password and the 503 issue was resolved.  You can often find useful information in the event logs, look at them when you are troubleshooting an issue, you might get lucky and the problem is logged and completely described as in this case.

[FIGURE1]: ../images/2013/msdn-0210.png "Figure 1, Application Pool default recycle setting = 1740 minutes"
[FIGURE2]: ../images/2013/msdn-0211.png "Figure 2, a stopped application pool"
[FIGURE3]: ../images/2013/msdn-0212.png "Figure 3, WAS event id 5021"
