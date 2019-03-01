# Why is my ASP.NET application recycling, restarting

When you are experiencing slowness with your ASP.NET application, one place to check is if the application is being recycled.  If the application is being recycled over and over again, then sessions and application settings are lost and it has a pretty big impact on your users and customers.

A place to begin your investigation is by enabling the “Generic Recycle Event Log Entry” Advanced Settings of your application pool.  I wrote an article about that [here][LINK1].

You can also place some code into your application_end event as described [here][LINK2].

You can also perform some ETW tracing as described [here][LINK3].

When all else fails, take a memory dump or a series of dumps, while the performance issue is happening and review it using Debug Diag or WinDbg.

I review a memory dump of such a case and I found many threads with a stack similar to this one:

```
…
clr!JITutil_MonContention+0xaf

System_Web!System.Web.Compilation.BuildManager.GetBuildResultFromCacheInternal(System.String, Boolean, System.Web.VirtualPath, Int64, Boolean)+0xb6

System_Web!System.Web.Compilation.BuildManager.GetVPathBuildResultFromCacheInternal(System.Web.VirtualPath, Boolean)+0x102               
…
```

What this showed me was that the code was being re-compiled when the memory dump was taken.  Notice at the top, the code path entered a MonConnection, which is a lock and was waiting on the compilation to complete.  This, IMO is common to see when the application is recycled, I.e. that the code is being recompiled.  As you can imagine, the fact that all the requests are now waiting on the compilation of the code, performance would not be so good.

I know that the application shutdown is triggered from the System.Web.HttpRuntime class.  You can review the source code for this class [here][LINK4].  Reading through the source code, at the end I find an enum named ApplicationShutDownReason that includes different types of reasons for the application to shutdown.

My goal was now to find out what the shutdown reason was and I used the W3WP memory dump to get it.  First I entered this command, as shown in Figure 1,  to get the address of all the System.Web.HttpRuntime objects:  ***!dumpheap -Type System.Web.HttpRuntime***

![dump the System.Web.HttpRuntime objects in WinDbg][FIGURE1]
###### Figure 1, dump the System.Web.HttpRuntime objects in WinDbg

There were six of them so I started at the bottom of the list and entered this command:  ***!do 000000040214fa28*** which resulted in the output shown in Figure 2.

![shutdownReason value within a W3WP memory dump][FIGURE2]
###### Figure 2, shutdownReason value within a W3WP memory dump

The output of dumping the properties of the System.Web.HttpRuntime object renders the _shutdownReason and is equal to 14.  Checking the source code, I can see that 14 means MaxRecompilationsReached.

I checked the !findebugtrue and found that in this situation debug=true and this should be set to false when in production.  I wrote an article about that [here][LINK5].  However, that was not the root cause of the issue.  The numRecompileBeforeAppRestart is set to 15 and when this counter is exceeded, then the application is shutdown.

This [article][LINK6] was helpful in explaining the issue and a valid action to increase the numRecompileBeforeAppRestart from its default.

I recently found worked on another similar issue, but this time the _shutdownReason was = 6 as seen in Figure 3.

![_shutdownReason value within a W3WP memory dump][FIGURE3]
###### Figure 3, _shutdownReason value within a W3WP memory dump

You can find a list of all the reason codes [here][LINK7].  6 = BinDirChangeOrDirectoryRename and would possibly show this exception stack:

```
shutDownStack=   at System.Environment.GetStackTrace(Exception e, Boolean needFileInfo)
at System.Environment.get_StackTrace()
at System.Web.Hosting.HostingEnvironment.InitiateShutdownInternal()
at System.Web.Hosting.HostingEnvironment.InitiateShutdownWithoutDemand()
at System.Web.HttpRuntime.ShutdownAppDomain(String stackTrace)
at System.Web.HttpRuntime.OnCriticalDirectoryChange(Object sender, FileChangeEvent e)
at System.Web.FileChangesMonitor.OnCriticaldirChange(Object sender, FileChangeEvent e)
at System.Web.DirectoryMonitor.FireNotifications()
at System.Web.Util.WorkItem.CallCallbackWithAssert(WorkItemCallback callback)
at System.Web.Util.WorkItem.OnQueueUserWorkItemCompletion(Object state)
at System.Threading.ExecutionContext.RunInternal(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)
at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)
at System.Threading.QueueUserWorkItemCallback.System.Threading.IThreadPoolWorkItem.ExecuteWorkItem()
at System.Threading.ThreadPoolWorkQueue.Dispatch()</Data></EventData></Event><Event><System><Provider Name=".NET Runtime"/><EventID>0</EventID><Level>0</Level><Task>0</Task><Keywords>Keywords</Keywords><TimeCreated SystemTime="2017-05-26T10:12:03Z"/><EventRecordID>549235078</EventRecordID><Channel>Application</Channel><Computer>RD0003FF1C6D3B</Computer><Security/></System><EventData><Data>

_shutDownMessage=Change Notification for critical directories.
bin dir change or directory rename
HostingEnvironment initiated shutdown
Change Notification for critical directories.
bin dir change or directory rename
```

[FIGURE1]: ../images/2015/msdn-0583.png "Figure 1, dump the System.Web.HttpRuntime objects in WinDbg"
[FIGURE2]: ../images/2015/msdn-0584.png "Figure 2, shutdownReason value within a W3WP memory dump"
[FIGURE3]: ../images/2015/msdn-0585.png "Figure 3, _shutdownReason value within a W3WP memory dump"

[LINK1]: ../2013/2013-07-troubleshooting-badly-behaving-iis-application-pools.md
[LINK2]: http://weblogs.asp.net/scottgu/433194
[LINK3]: http://blogs.msdn.com/b/tess/archive/2008/11/06/troubleshooting-appdomain-restarts-and-other-issues-with-etw-tracing.aspx
[LINK4]: http://referencesource.microsoft.com/#System.Web/HttpRuntime.cs
[LINK5]: ../2012/2012-09-debug-true.md
[LINK6]: https://support.microsoft.com/en-us/kb/319947
[LINK7]: http://referencesource.microsoft.com/#System.Web/HttpRuntime.cs,4d404925e0f99eb1
