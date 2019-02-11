# What’s new in IIS 8.5 – Microsoft Internet Information Services 8.5 New Features

With the release of the new version of Windows Server 2012 R2 comes a new version of IIS, IIS 8.5.  The new features can be broken into 2 categories, Scalability and Manageability:

+ Manageability - Logging Enhancements
+ Manageability - ETW Events
+ Scalability - Dynamic site activation
+ Scalability - Idle Worker Process Page-out
+ Certificate Rebind - see [here][LINK1]
+ dynamicRegistrationThreshold (system.applicationHost/webLimits) - see [here][LINK2]

The following article contains a brief explanation of the new features.  For a very nice and more in-depth walk through of these new features, access the information by watching this Channel9 video “Internet Information Services: What’s New in Window Server 2012 R2 Internet Information Services: What’s New in Window Server 2012 R2” [here][LINK3].

## ETW Events

Event Tracing for Windows has been around for many years.  Now it is built right into IIS.  Select the Logging feature from within the IIS 8.5 management console and you will notice this new group box of additional features, shown in Figure 1.

![new ETW logging features in IIS 8.5][FIGURE1]
###### Figure 1, new ETW logging features in IIS 8.5

If you enable ETW logging which is useful for getting a deeper look into the “behind the scenes” of what’s going on with your request, consider using a tool called Microsoft Message Analyzer to analyze the ETW log.  Details of which can be found [here][LINK4].

Notice that you can log both ETW and standard IIS logs simultaneously.

Additional information about ETW can be found [here][LINK5].

## Logging Enhancements

Click on the Logging feature from within the IIS management console followed by clicking on the Select Fields… button within the Log File group box.  Doing this opens a window as shown in Figure 2.

![IIS 8.5 Logging Fields windows][FIGURE2]
###### Figure 2, IIS 8.5 Logging Fields windows

Notice the new section called Custom Fields.  Selecting the Add Field… button opens a window as illustrated in Figure 3.

![IIS 8.5 Add Custom Field windows][FIGURE3]
###### Figure 3, IIS 8.5 Add Custom Field windows

This feature allows for the logging of fields within Request Header, Response Header and Server Variables.  The following bulleted list represents a small example of the selectable fields.

+ Request Header – If-Match
+ Request Header – Proxy-Authorization
+ Request Header – Max-Forwards
+ Response Header – Last-Modified
+ Response Header - ETag
+ Server Variable – ALL_RAW
+ Server Variable – REMOTE_HOST
+ Server Variable – SCRIPT_NAME
+ Etc...

Also note that the Source text box is editable so you can capture custom request, response and server variables.

## Dynamic site activation

When you are hosting a lot of web sites (100s) in IIS version 6/7/7.5, you may have experienced some noticable time when loading the IIS configuration file.  When WAS runs it will load the configuration for all web sites hosted on the server.  In IIS 8.5 a new more optimized process has been implemented to improve the performance of this activity.  Note: There were also many improvements in this area found in IIS 8.0.

There is no user interface for this feature.  However, the feature is configurable via the Configuration Editor as shown in Figure 4.

![dynamicRegistrationThreshold attribute in IIS 8.5][FIGURE4]
###### Figure 4, dynamicRegistrationThreshold attribute in IIS 8.5

By default, if you have less than 100 web sites / bindings on your web server, then the standard process for loading the configuration is used.  The benefit of this feature is primarily realized when a large number of web sites are hosted on the web server.

## Idle Worker Process Page-out

When a worker process is not used for 20 minutes, IIS will terminate the process.  This is done to conserve memory and is very important on server where memory utilization is a concern.  In IIS 8.5 instead of terminating the worker process when the Idle Time-out threshold is breached, you can Suspend it instead.  Figure 5 illustrates the new Idle Time-out Action (idleTimeoutAction) parameter which supports either to Terminate or Suspend the worker process.

![idleTimeoutAction in IIS 8.5, Terminate or Suspend][FIGURE5]
###### Figure 5, idleTimeoutAction in IIS 8.5, Terminate or Suspend


[FIGURE1]: ../images/2013/msdn-0243.png "Figure 1, new ETW logging features in IIS 8.5"
[FIGURE2]: ../images/2013/msdn-0244.png "Figure 2, IIS 8.5 Logging Fields windows"
[FIGURE3]: ../images/2013/msdn-0245.png "Figure 3, IIS 8.5 Add Custom Field windows"
[FIGURE4]: ../images/2013/msdn-0246.png "Figure 4, dynamicRegistrationThreshold attribute in IIS 8.5"
[FIGURE5]: ../images/2013/msdn-0247.png "Figure 5, idleTimeoutAction in IIS 8.5, Terminate or Suspend"

[LINK1]: https://www.iis.net/learn/get-started/whats-new-in-iis-85/certificate-rebind-in-iis85
[LINK2]: https://www.iis.net/configreference/system.applicationhost/weblimits
[LINK3]: https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/MDC-B303#fbid=2zy5Qvl5j2L
[LINK4]: http://technet.microsoft.com/en-us/library/jj649776.aspx
[LINK5]: http://msdn.microsoft.com/en-us/library/ms751538.aspx
