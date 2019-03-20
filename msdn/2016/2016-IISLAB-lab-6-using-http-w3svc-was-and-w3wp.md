# Lab 6: Using HTTP, W3SVC, WAS and W3WP

There is a lot going on when a client makes an HTTP request to IIS. There are numerous kernel mode and native mode components that are used to route the request to the W3WP process which ultimately responds to the request.

![Using HTTP, W3SVC, WAS and W3WP][FIGURE1]
###### Figure 1, Using HTTP, W3SVC, WAS and W3WP

See Table 1 for a list and definition of those modules.

| Component | Description |
| --------- | ----------- |
| HTTP.sys | Kernel level HTTP/HTTPS protocol listener |
| W3SVC | World Wide Web Publishing Service – User level listener adapter for HTTP.sys |
| WAS | Windows Process Activation Service – Worker Process (W3WP) manager |
| W3WP |	IIS Worker Process -> Application Pool  |

###### Table 1, modules required to respond to an HTTP request

Table 2 shows the location where each of the modules write logs to.

| Component | Logs are written to |
| --------- | ------------------- |
| HTTP.sys | C:\Windows\System32\LogFiles\HTTPERR |
| W3SVC | System Event Viewer |
| WAS | System Event Viewer |
| W3WP | System Event Viewer and IIS Logs in C:\intepub\logs\LogFiles\W3SVC* |

##### Table 2, where are the logs written

Please review the following for more details about the IIS architecture.

## What is HTTP.sys

There is a great description of what this is and does [here][LINK1], so no need to cover it all. However, in brief, the http.sys listens for HTTP requests from the network, passes them to IIS and returns the response. It is a service that can be stopped and started from the command line. See Figure 2.

![stop and start the http service][FIGURE2]
###### Figure 2, stop and start the http service

You can modify the http.sys settings using NETSH HTTP, described [here][LINK3]. The NETSH HTTP command updates the http.sys registry settings found [here][LINK3]. When you modify http.sys setting, you need to recycle the HTTP listener by running, NET STOP HTTP and then NET START HTTP, as shown previously. *NOTE: don’t recycle the HTTP listener on a live production machine unless some downtime is acceptable.

## What is the World Wide Web Publishing Service (W3SVC)

The World Wide Web Publishing service, described in detail [here][LINK4], is a listener adapter for http.sys. The W3SVC service can be stopped and started from the Services management console, as shown in Figure 3.

![stop and start the world wide web publishing service][FIGURE3]
###### Figure 3, stop and start the world wide web publishing service

W3SVC is responsible for the configuration of http.sys and WWW performance counters.

## What is the Windows Process Activation Process (WAS)

WAS, as described in great detail [here][LINK5]. In short, it manages the application pool configuration and the worker processes (W3WP). I wrote an article that touches on the ‘effective configuration’ [here][LINK6], that covers some of what WAS would do when spinning up a worker process. Basically, WAS gathers all the configuration files, shown in Figure 4 and creates an effective configuration and stores it in the C:\inetpub\temp\appPools\ directory.

![IIS effective configuration example][FIGURE4]
###### Figure 4, IIS effective configuration example

The WAS service can be stopped and started from the Services management console, as shown in Figure 5.

![stop and start the windows process activation service][FIGURE5]
###### Figure 5, stop and start the windows process activation service

Errors that happen at the WAS level are logged into the Event Viewer System Event log.

## The IIS worker process, W3WP

What is the difference or relation between an Application Pool and the W3WP process? As eloquently stated [here][LINK7], an app pool is a grouping of URLs that is routed to one or more worker processes. You can have a single W3WP process that runs multiple websites, but this makes it a little harder to troubleshoot. So instead, have a 1-to-1 mapping between websites and applications pools.

You can view the W3WP process using Task Manager, for example, illustrated in Figure 6.

![viewing the W3WP process][FIGURE6]
###### Figure 6, viewing the W3WP process

Notice in Figure 5 that the User Name is the same as the Application Pool name you find in the IIS management console. Shown in Figure 7.

![the application pool identity name][FIGURE7]
###### Figure 7, the application pool identity name

You learned about the basic and advanced features of an application pool in the previous lab here.

I wrote numerous articles about how to troubleshoot a badly behaving worker process [here][LINK8]. There are also some very nice new features for the application pool that write event logs when specific events happen, as described [here][LINK9].

## Lab – Start, stop and experience the effects

In this lab you will review the different error messages and behavior when each of the components are shutdown.

1. Execute the NET STOP HTTP, as shown previously in Figure 1.
2. Open the IIS Management console and you will see a black square on the wed sites, as seen in Figure 8.

![http listener is stopped, black squares on web sites][FIGURE8]
###### Figure 8, http listener is stopped, black squares on web sites

3. Attempt to browse to the localhost on the server or from a client and you will get a „This page can’t be displayed“ response. You will not find any IIS Log or HTTP error log, because the request does not make it far enough along for anything to log it’s failure.
  a. Where would you look to see if the HTTP listener is stopped? (Services)
  b. Why would you need to stop the HTTP listener? (Recycle)
4. Take some time now to open a command prompt and read through the NETSH HTTP commands and see what exists.
5. Start the HTTP listener by entering NET START HTTP
6. Access the Default Web Site (localhost) now, what happens? You still get the „This page can’t be displayed“ message. Why? When you look back into the IIS management console, as shown in Figure 9, you see the black square is still there and that the website is not started. Click on the Start action for the Default Web Site.

![W3SVC, world wide publishing service is stopped, black squares on web sites][FIGURE9]
###### Figure 9, W3SVC, world wide publishing service is stopped, black squares on web sites

Turns out that a recycle of the HTTP listener shuts down the World Wide Web Publishing service as well.

7. Restart the service as shown in Figure 10 and then access the the Default Web Site successfully.

![start the W3SVC service][FIGURE10]
###### Figure 10, start the W3SVC service

Also notice, that if you refresh the list of websites in the IIS managements console, after restarting the W3SVC service, that the black squares are gone.

You can also stop and start the World Wide Web Publishing service from the console using NET STOP W3SVC and NET START W3SVC, as shown in Figure 11.

![stop and start the W3SVC service from a command prompt][FIGURE11]
###### Figure 11, stop and start the W3SVC service from a command prompt

8. Stop the W3SVC service by entering NET STOP W3SVC as shown in Figure 11. Attempt to access the Default Web Site or CSharpGuitarBugs web site and again it fails. Execute an IISRESET, then access the a web site. It works, an IISRESET restarts the W3SVC service.

9. Click on the Application Pools folder, as shown in Figure 12 and stop the CSharpGuitarBugs application pool. Again, notice the black square next in the icon next to the application pool name.

![a stopped application pool][FIGURE12]
###### Figure 12, a stopped application pool

10. Access the CSharpGuitarBugs web site and you get a 503 – The service is unavailable, as shown in Figure 13.

![503 service is unavailable][FIGURE13]
###### Figure 13, 503 service is unavailable

11. Run an IISRESET, and recognized that it does not restart the application pool
12. Q:   Where can you look for information about why the Application Pool is shutdown?  A:  The System Log of the Event Viewers
  a. [This][LINK8] post, troubleshoot badly behaving application pools
  b. Capture a memory dump, as described [here][LINK9]
13. Click on the Advanced Settings for the CSharpGuitarBugs and confirm Manual Recycle to TRUE, as shown in Figure 14.

![configure application pool event logging][FIGURE14]
###### Figure 14, configure application pool event logging

14. Manually recycle or stop/start the the application pool and check the event log for the entry.

## Conclusion

You now understand the different components of the IIS architecture, each one has it’s own way of managing it and each responds differently when it is having an issue or disabled. Each writes error logs to a different location for analysis and review. Each component needs to be healthy and functional for a request to receive a valid responds.

This image illustrates the flow of a request to the W3WP process.

![Using HTTP, W3SVC, WAS and W3WP][FIGURE1]
###### Figure 15, Using HTTP, W3SVC, WAS and W3WP

[FIGURE1]: ../images/2016/msdn-0723.png "Figure 1, Using HTTP, W3SVC, WAS and W3WP"
[FIGURE2]: ../images/2016/msdn-0724.png "Figure 2, stop and start the http service"
[FIGURE3]: ../images/2016/msdn-0725.png "Figure 3, stop and start the world wide web publishing service"
[FIGURE4]: ../images/2016/msdn-0726.png "Figure 4, IIS effective configuration example"
[FIGURE5]: ../images/2016/msdn-0727.png "Figure 5, stop and start the windows process activation service"
[FIGURE6]: ../images/2016/msdn-0728.png "Figure 6, viewing the W3WP process"
[FIGURE7]: ../images/2016/msdn-0729.png "Figure 7, the application pool identity name"
[FIGURE8]: ../images/2016/msdn-0730.png "Figure 8, http listener is stopped, black squares on web sites"
[FIGURE9]: ../images/2016/msdn-0731.png "Figure 9, W3SVC, world wide publishing service is stopped, black squares on web sites"
[FIGURE10]: ../images/2016/msdn-0732.png "Figure 10, start the W3SVC service"
[FIGURE11]: ../images/2016/msdn-0733.png "Figure 11, stop and start the W3SVC service from a command prompt"
[FIGURE12]: ../images/2016/msdn-0734.png "Figure 12, a stopped application pool"
[FIGURE13]: ../images/2016/msdn-0735.png "Figure 13, 503 service is unavailable"
[FIGURE14]: ../images/2016/msdn-0736.png "Figure 14, configure application pool event logging"

[LINK1]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Hypertext
[LINK2]: http://technet.microsoft.com/en-us/library/cc725882(v=WS.10).aspx
[LINK3]: http://support.microsoft.com/kb/820129
[LINK4]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#WWW
[LINK5]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#WAS
[LINK6]: http://blogs.msdn.com/b/benjaminperkins/archive/2012/09/17/debug-true.aspx
[LINK7]: https://technet.microsoft.com/en-us/library/cc735247(v=ws.10).aspx
[LINK8]: ../2015/2015-09-create-a-w3wp-memory-dump-quick-and-easy.md
[LINK9]: ../2013/2013-07-troubleshooting-badly-behaving-iis-application-pools.md
