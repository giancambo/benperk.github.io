# Lab 3: Install and configure Application Initialization

I have written a few articles about this [here][LINK1], but wanted to expand on it with this specific lab. In this lab you will:

+ Install the Application Initialization feature
+ Utilize ASP.NET MVC website created in [Lab 1][LINK2] and [Lab 2][LINK3] hosted [here][LINK4]
+ Configure Application Initialization for the CSharpGuitarBugs website
+ Learn about the ASP.NET warm-up request
+ Learn about ASP.NET temporary files in [Lab 28][LINK5]

I got lots of the contents of this lab from [here][LINK6].

## Install the Application Initialization feature

Q: What issue does this module attempt to resolve?  A: When changes are made to the ASP.NET application or certain configuration changes are made, the temporary ASP.NET files need to be recompiled. The first request to the ASP.NET file is what triggers the compilation. Depending on the complexity of the web application, this request can take some time. This module makes the first request to the web site after a recycle or restart and warms up the site, this avoids having a customer or visitor doing it.

Install the Application Initialization using the WPI as per these instructions, [here][LINK7] up to Figure 2. The website in the that article is referring to another site, the source for that site can be downloaded [here][LINK8].

If you have not already accessed the CSharpGuitarBugs website, then open the task manager and you will notice that there is no W3WP process running for the CSharpGuitarBugs website. If you have already accessed the site, there will be a W3WP process, see Figure 3, execute an IISReset from a command prompt, shown in Figure 1, and notice that the W3WP process is removed from task manager.

![execute an IISReset to stop W3WP processes][FIGURE1]
###### Figure 1, execute an IISReset to stop W3WP processes

Using the configuration editor, at the server level, as shown in Figure 2, set the startMode for the CSharpGuitarBugs application pool to AlwaysRunning.

![setting startMode as AlwaysRunning for IIS, AlwaysOn][FIGURE2]
###### Figure 2, setting startMode as AlwaysRunning for IIS, AlwaysOn

Close the Collection Editor window and press the Apply button in the Action pane to apply the settings. NOTE: before you press the Apply button, click on the Generate Script link. This is a cool feature to view the PowerShell, C#, AppCmd, etc. for making the same modification. You will then see the W3WP process running for this web site in Task Manager, as illustrated by Figure 3.

![AlwaysRunning W3WP process][FIGURE3]
###### Figure 3, AlwaysRunning W3WP process

Q: What are the advantages of having the W3WP process always running? A: By default, a worker process will terminate if there is no access to it within 20 minutes. That means that the process and the memory which it consumes is de-allocated and can then be used by other processes. If the W3WP process is not running, when a request is made to the website, WAS (discussed in [Lab 6][LINK9]) will have to spin up the process before the request can be responded to. Having your process AlwaysRunning will keep the W3WP process running without shutting it down after an time period. In IIS 8.5, instead of terminating the W3WP process after 20 minutes, you can now suspend it. Read [here][LINK10] for more information about that.

Next, use Process Explorer, downloadable from [here][LINK11] to look at the properties of the W3WP process. Figure 4 illustrates that there is no .NET Assemblies tab, which signifies that there is no managed code compiled or contained within the process yet.

![Process Explorer showing no .NET Assemblies][FIGURE4]
###### Figure 4, Process Explorer showing no .NET Assemblies

Take a quick look at the compiled ASP.NET files in the C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Temporary ASP.NET Files\root directory. You should not see any compiled ASP.NET files for this web site because it has not been accessed.

Access the CSharpGuitarBugs web site and then refresh the W3WP process in Process Explorer. Notice, Figure 5, that there is now a .NET Assemblies tab and that you can see the modules that were compiled.

![temporary ASP.NET files created with first access, warm-up][FIGURE5]
###### Figure 5, temporary ASP.NET files created with first access, warm-up

Take another look at the compiled ASP.NET files in the C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Temporary ASP.NET Files\root directory. You should now see some compiled ASP.NET files, similar to Figure 6. Notice that the assemblies reported via Process Explorer have a matching .dll in the Temporary ASP.NET Files folder.

![Temporary ASP.NET files folder][FIGURE6]
###### Figure 6, Temporary ASP.NET files folder

Now, open Task Manager again and find the W3WP process for the CSharpGuitarBugs website and note the process id (PID). Execute and IISReset, as shown previously in Figure 1, and notice the W3WP process is recycled and a new W3WP process, with a different PID, is started automatically.

Q: If nothing changes, will the ASP.NET pages be recompiled after an IISReset? A: No, if nothing changes, after a recycle, then IIS is smart enough to know that the files do not need to be recompiled and the request is responded to using the already compiled ASP.NET file, for example the App_Web_ezxbpqls.dll file. You can confirm this by looking at the Date Modified on the temporary file. Does it change after a recycle?

So the real value of Application Initialization is to make the first request after changes are made to the application that require the ASP.NET page to be recycled, like a deployment of a new release or a configuration that requires a recompile, like setting debug=false/true, for example, discussed here.

To complete the configuration of Application Initialization, again open the Configuration Editor at the server level and modify the ***applicationPool***, ***path*** and ***preloadEnabled*** values as shown in Figure 7.

![Application Initialization required configurations][FIGURE7]
###### Figure 7, Application Initialization required configurations

Close the Collection Editor and apply the changes by clicking the Apply link in the Action pane.

Manually stop the CSharpGuitarBugs application pool, as show in Figure 8 and delete the existing Temporary ASP.NET Files. This is done to make sure there is no W3WP process running and that there are no existing temporary ASP.NET files insuring that when we start the process again, we are certain Application Initialization triggered the compilation.

![stop the website and application pool][FIGURE8]
###### Figure 8, stop the website and application pool

The expectation is that now that Application Initialization is configured, as soon as I start the application pool again, that a warm-up request is sent and the ASP.NET temporary files are compiled and ready for the first real request from a user or customer.

You might consider executing an IISReset as discussed previously, Figure 1. Additionally, if the warm up request does not happen, make sure you have installed the Application Initialization role, as per [these][LINK7] instructions and mentioned at the beginning, as it is possible to make the preceding confirmations without having the module installed. See Figure 9.

![installing the Application Initialization role][FIGURE9]
###### Figure 9, installing the Application Initialization role



[FIGURE1]: ../images/2016/msdn-0676.png "Figure 1, execute an IISReset to stop W3WP processes"
[FIGURE2]: ../images/2016/msdn-0677.png "Figure 2, setting startMode as AlwaysRunning for IIS, AlwaysOn"
[FIGURE3]: ../images/2016/msdn-0678.png "Figure 3, AlwaysRunning W3WP process"
[FIGURE4]: ../images/2016/msdn-0679.png "Figure 4, Process Explorer showing no .NET Assemblies"
[FIGURE5]: ../images/2016/msdn-0680.png "Figure 5, temporary ASP.NET files created with first access, warm-up"
[FIGURE6]: ../images/2016/msdn-0681.png "Figure 6, Temporary ASP.NET files folder"
[FIGURE7]: ../images/2016/msdn-0682.png "Figure 7, Application Initialization required configurations"
[FIGURE8]: ../images/2016/msdn-0683.png "Figure 8, stop the website and application pool"
[FIGURE9]: ../images/2016/msdn-0684.png "Figure 9, installing the Application Initialization role"

[LINK1]: ../../README.MD
[LINK2]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK3]: 2016-IISLAB-lab-2-install-the-web-platform-installer.md
[LINK4]: https://github.com/benperk/CSharpGuitarBugs
[LINK5]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/06/20/lab-28-the-impact-of-debugtrue/
[LINK6]: http://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-application-initialization
[LINK7]: ../2014/2014-01-configure-the-iis-application-initialization-module.md
[LINK8]: https://github.com/benperk/AppInit
[LINK9]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/06/16/lab-6-using-http-w3svc-was-and-w3wp/
[LINK10]: http://blogs.msdn.com/b/benjaminperkins/archive/2013/06/25/what-s-new-in-iis-8-5.aspx
[LINK11]: https://technet.microsoft.com/en-us/sysinternals/bb896653.aspx
[LINK12]: ../2012/2012-09-debug-true.md
