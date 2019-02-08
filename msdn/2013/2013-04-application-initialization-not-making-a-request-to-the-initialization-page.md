# Application Initialization not making a request to the initialization page

Setting up Application Initialization is pretty straight forward.  Instructions can be found [here][LINK1] or [here][LINK2].

After setting it up the Application Initialization module with these parameters:

+ autoStart = True
+ startMode = AlwaysRunning

when IIS was restarted the W3WP worker process was started up and running just as expected.  Figure 1 shows a Process Explorer Properties window.

![Process Explorer showing running W3WP worker process][FIGURE1]
###### Figure 1, Process Explorer showing running W3WP worker process

The problem was that a ‘simulated’ request was not sent to the worker process after a restart so my ASPX file was not compiled.  Precompiling the file prior to the first request is a good idea and will certainly improve user experience for the first person to request that page after a restart.

I found that by setting the parameter preloadEnabled, shown in Figure 2, resulted in a pre-compilation of the file identified in the initializationPage parameter of my web.config.  This web.config file exists in the root directory for the web site which I configure Application Initialization for.

![configure the preloadEnabled attribute][FIGURE2]
###### Figure 2, configure the preloadEnabled attribute

Before I made the change I stopped the application pool which I had configured for Application Initialization.  When I started the application pool again the W3WP worker process was indeed started as previous, but also the .NET Assemblies tab showed up in my Process Explorer as illustrated in Figure 3.  As well, there was a compiled ASP.NET file created in my C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Temporary ASP.NET Files\root directory.

![Process Explorer with .NET Assemblies tab present][FIGURE2]
###### Figure 2, Process Explorer with .NET Assemblies tab present

I write this article because I missed that very important piece of the original instructions and felt it was worth mentioning again.  HTH.

[FIGURE1]: ../images/2013/msdn-0229.png "Figure 1, Process Explorer showing running W3WP worker process"
[FIGURE2]: ../images/2013/msdn-0230.png "Figure 2, configure the preloadEnabled attribute"
[FIGURE3]: ../images/2013/msdn-0231.png "Figure 3, IIS"

[LINK1]: https://blogs.msdn.microsoft.com/benjaminperkins/2014/01/07/configure-the-iis-application-initialization-module/
[LINK2]: http://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-application-initialization
