# Configure the IIS Application Initialization module

During my many years of active IIS administration which hosted ASP.NET application, we had many discussions about why the first request to the web site took longer than those coming after.  In short, it’s because the first request, after a restart of the process results in a recompilation of the temporary ASP.NET files.  In complicated, large scale enterprise applications, the compilation of the temporary ASP.NET files can take seconds.  On one of the system I supported, we actually implemented a process that rotated the responsibility for making the first call to the web site each morning at 6AM to make sure none of our users experienced this.  Sure, you can pre-compile the code but I am talking about long ago, in IT years…

Now that this feature, Application Initialization exists, there is no longer a need for that or anything like it.  Once you recycle or restart the IIS process, a request is sent to the website and the binaries are compiled and ready for fast usages.

To install the feature you can use the Web Platform Installer on IIS 7.5, as shown in Figure 1 or install it as a feature in IIS 8/8.5+.

![Install Application Initialization using the Install Feature][FIGURE1]
###### Figure 1, Install Application Initialization using the Install Feature

There is a good set of instruction on how to setup Application Initialization [here][LINK1].  I will cover similar topics, but expand, simplify a little.

After the feature has been installed and you have configured an ASP.NET web site, set the ***startMode*** of the application pool associated to the web site in which you want to enable Application Initialization on to ***AlwaysRunning***.  Figure 2 illustrates a method for doing this using the Configuration Editor.

![Set application pool to AlwaysRunning][FIGURE2]
###### Figure 2, Set application pool to AlwaysRunning

By setting the startMode to AlwaysRunning, when you recycle the process or perform a reboot, the IIS process (W3WP.exe) is started up automatically.  This is not the case by default, where after a restart or reboot, the IIS worker process is not started until a request is sent to it.  Apply these changes and open the Task Manager, you will notice that there is a W3WP process running for the associated web site, even though there has been no request made to it. 

To test this feature further, download and install Process Explorer [here][LINK2], find the W3WP process for the web site, open the details window and notice that there is no .Net Assemblies tab.   Figure 3 illustrates this.

![Use Process Explorer to view W3WP compiled assemblies, not present][FIGURE3]
###### Figure 3, Use Process Explorer to view W3WP compiled assemblies, not present

You can also take a quick look at the compiled ASP.NET files which already exist in the C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Temporary ASP.NET Files\root directory to set a baseline.  Note: this assume you are using .NET 4 on a 64bit machine.

You should not see any compiled ASP.NET files for this web site because it has not yet been accessed.

Access the ASP.NET web site and reopen the details windows for the worker process.  You will then see the compiled .NET assemblies, as shown in Figure 4.  This will happen only once and future requests to those ASPX files will be handled from the compiled binary which results in much faster response.

![Use Process Explorer to view W3WP compiled assemblies, present][FIGURE4]
###### Figure 4, Use Process Explorer to view W3WP compiled assemblies, present

In Windows Explorer, navigate to C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Temporary ASP.NET Files\root and see if the ASP.NET file has been compiled…  You can also try matching the names in Figure 5 to the .DLL file.

Next, before executing an IISReset, open Task Manager and locate the PID for the W3WP process of the web site worker process.  When the existing worker process is shut down and a new one created.  Have both a command prompt to enter the command shown in Figure 5 plus the Task Manager and watch the removal of the existing W3WP process and the creation of a new one automatically, it is automatic because startMode=AlwaysRunning.

![Execute an IISReset][FIGURE5]
###### Figure 5, Execute an IISReset

Up to now, IIS is only configured to start the W3WP worker process for the web site, but the initial request that compiles the ASP.NET files still needs to be configured.  To perform this, make the following modifications and as shown in Figure 6.

+ applicationPool = The name of the web sites Application Pool
+ path = the path to the web site, for example a Virtual Directory
+ preloadEnabled = true

![Configure Application Initialization using Configuration Editor][FIGURE6]
###### Figure 6, Configure Application Initialization using Configuration Editor

Apply your changes.  To test that everything is working as expected, you can stop the web site configured for Application Initialization and delete the existing ASP.NET temporary files.  Check the Task Manager for a worker process, Process Explorer for any .Net assemblies and restart the web site.  You should see the worker process automatically started, a request being sent to the web site and the .Net assemblies being compiled.

[FIGURE1]: ../images/2014/msdn-0319.png "Figure 1, Install Application Initialization using the Install Feature"
[FIGURE2]: ../images/2014/msdn-0320.png "Figure 2, Set application pool to AlwaysRunning"
[FIGURE3]: ../images/2014/msdn-0321.png "Figure 3, Use Process Explorer to view W3WP compiled assemblies, not present"
[FIGURE4]: ../images/2014/msdn-0322.png "Figure 4, Use Process Explorer to view W3WP compiled assemblies, present"
[FIGURE5]: ../images/2014/msdn-0323.png "Figure 5, Execute an IISReset"
[FIGURE6]: ../images/2014/msdn-0324.png "Figure 6, Configure Application Initialization using Configuration Editor"

[LINK1]: http://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-application-initialization
[LINK2]: http://technet.microsoft.com/en-us/sysinternals/bb896653.aspx
