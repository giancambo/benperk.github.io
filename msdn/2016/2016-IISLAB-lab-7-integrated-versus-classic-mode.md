# Lab 7: Integrated versus Classic mode

## Prerequisites
+ Read [this][LINK1] article about the processModel
+ Read [this][LINK2] article about ASP.NET Threads
+ Read [this][LINK3] article about Breaking Changes between Classic and Integrated mode
+ See LAB 24: Deploy a Custom Module and Handler, Integrated mode support both Managed and Native Modules, once available
+ Read [this][LINK4] article about ASP.NET Page Events
+ Read Contention, poor performance, deadlocks with ASP.NET [here][LINK5].

## Setup

+ Setup CSharpGuitarBugs website, as per Lab 1

## Lab 7-1

### Check max concurrent request limits

![Integrated versus Classic mode][FIGURE1]
###### Figure 1, Integrated versus Classic mode

1. Take a memory dump of CSharpGuitarBugs using one of [this][LINK6] method after accessing an ASPX file or MVC route.  Q:  Why is taking a memory after accessing the ASPX file important?  A:  It is important because none of the binaries will be compiled into the process until there is a request made to it, learn more about this in [Lab 3][LINK7].  I created the memory dump after clicking a few links on the web site, nothing particular.

![Integrated versus Classic mode][FIGURE2]
###### Figure 2, Integrated versus Classic mode

2. Open the memory dump in WinDbg and load the SOS.dll version corresponding to the .NET version which the web site is running in.  For example, the one I used was C:\Windows\Microsoft.NET\Framework64\v4.0…\SOS.dll.  I chose the Framework64 becuase my W3WP process is running in 64 bit mode.

![Integrated versus Classic mode][FIGURE3]
###### Figure 3, Integrated versus Classic mode

3. I know that the MaxWorkerThreads, RequestQueueLimit  and MaxIoThreads propertires are contained within the System.Web.Configuration.ProcessModelSection class described [here][LINK8].  The minFreeThreads and minLocalRequestFreeThreads are contained within the System.Web.Configuration.HttpRuntimeSection class described [here][LINK9].   The maxConnection is contained in the System.Web.Configuration.ConnectionManagement class described [here][LINK10].  Additional properties like MaxConcurrentRequestsPerCPU and MaxConcurrentThreadsPerCPU which are contained in the System.Web.Hosting.HostingEnvironment class described [here][LINK11].  Historically, you could have used PSSCOR4.DTC to achieve about the same as dumping them all out manually, see [here][LINK12], but at the moment there are some issues with that, and doing it old school gives some better depth and understanding.

NOTE(1):  I wrote another post about the HostingEnvironment class [here][LINK13], where you can look at the _shutDownReason to determine why an ASP.NET app is recycling.  It might be worth a quick look.

NOTE(2): If you look at the source code for the MaxConcurrentRequestsPerCPU and MaxConcurrentThreadsPerCPU in the System.Web.Hosting.HostingEnvironment class [here][LINK14] you will notice that it returns an [UnsafeIISMethods.MgdGetMaxConcurrentThreadsPerCPU();][LINK15].  click on the [MgdGetMaxConcurrentThreadsPerCPU()][LINK16] method and you find that it is calling a method with the same name via a [DllImport][LINK17] on a non open source IIS module.  What this means is that you will not be able to dump this without private symbols, I tested with the public ones and I was not able to dump them.  You can also see in the code that those 2 properties are available in Integrated mode.

Let’s start by dumping out the RequestQueueLimit in the within the System.Web.Configuration.ProcessModelSection class see the source code [here][LINK18] and notice it is stored in _propRequestQueueLimit

+ .load <path>\sos.dll
+ !sos.dumpheap –type System.Web.Configuration.ProcessModelSection
+ !do <address>

![Integrated versus Classic mode][FIGURE4]
###### Figure 4, Integrated versus Classic mode

4. Although you cannot currently enter “dt webengine4!W3_MGD_REQUEST_QUEUE::***”, as mentioned above in NOTE(2) due to needing private symbols, I have done it to show the the value?  Q: Why is the vale 30000? A: Because the default value is 5000 per CPU, because I am running on a 6 CPU machine (6 * 5000 = 30000).

![Integrated versus Classic mode][FIGURE5]
###### Figure 5, Integrated versus Classic mode

5. For the CSharpGuitarBugs web site, configure MaxConcurrentRequestsPerCPU in the system registry to 6000.

![Integrated versus Classic mode][FIGURE6]
###### Figure 6, Integrated versus Classic mode

6. Restart IIS and run the CSharpGuitarBugs and access a few pages, create a new memory dump.

  a. Q: What does Restart IIS mean?  A: Because I made a change to the registry I stopped/started HTTP, started W3SVC and manually restarted the application pool and the site via the portal.

7. Enter “dt webengine4!W3_MGD_REQUEST_QUEUE::***”.  Q: What would you expect the result to be, why? See #4 above.

![Install][FIGURE7]
###### Figure 7, Install

8. The CSharpGuitarBugs website cannot run in Classic mode, however, as mentioned above, this setting is only available in Integrated mode.

## Lab 7-2

Make the same configuration by modifying the aspnet.config file.

9. The same modification can be made in the <system.web><applicationPool> in the associated aspnet.config for the corresponding ASP.NET version. See below table.

| Version of .NET Framework | Location of Aspnet_regiis.exe file |
| ------------------------- | ---------------------------------- |
| .NET Framework version 3.0, and version 3.5 (32-bit systems) | %windir%\Microsoft.NET\Framework\v2.0.50727 |
| .NET Framework 3.0, and version 3.5 (64-bit systems) | %windir%\Microsoft.NET\Framework64\v2.0.50727 |
| .NET Framework version 4 (32-bit systems) | %windir%\Microsoft.NET\Framework\v4.0.30319 |
| .NET Framework version 4 (64-bit systems) |	%windir%\Microsoft.NET\Framework64\v4.0.30319 |

10. Make the change as shown below to the aspnet.config associated to the .NET Framework version your application pool is running under.

```
<?xml version="1.0" encoding="UTF-8" ?>
  <configuration>
    <system.web>
      <applicationPool maxConcurrentRequestsPerCPU="7000" requestQueueLimit="9555" />
    </system.web>
  </configuration>
```

***NOTE***: The configuration in the aspnet.config file will override any registry value (can be set in registry for 3.5+ too) and will not be valid in Classic mode.

11.  Take another memory dump and look at both the requestQueueLimit and the maxConcurrentRequestsPerCPU (if possible).

## Lab 7-3

### Pipeline Integration

Important topic is when migrating to IIS 7+ from IIS 6, if you get errors check to see if it runs in Classic mode. If yes, it’s because of a breaking change in the pipeline. As IIS 6 has retired over a year ago this may not be of much value, however, to know what once was does sometimes provide better understanding of why we have what currently exists.

+ IIS 6 has 2 pipeline
  + Native Code – Filters
  + Managed Code – ASP.NET
+ IIS 7+ has a single pipeline
  + IIS and ASP.NET authentication stages are integrated
    + Cannot use both Forms and IWA, Basic
    + Results of authentication are available only in PostAuthenticateRequest


| IIS Pipeline (example) | ASP.NET Pipeline (HttpApplication Events) |
| ---------------------- | ----------------------------------------- |
| BEGIN_REQUEST | BeginRequest |
| .NEAUTHENTICATE_REQUEST | AuthenticateRequest |
| AUTHORIZE_REQUEST | AuthorizeRequest |
| RESOLVE_REQUEST_CACHE | ResolveRequestCache |
| MAP_REQUEST_HANDLER | AcquireRequestState |
| ... | ... |
| PRE_EXECUTE_REQUEST_HANDLER |  |
| EXECUTE_REQUEST_HANDLER |PreRequestHandlerExecute  |
| RELEASE_REQUEST_STATE | PostRequestHandlerExecute |
| UPDATE_REQUEST_CACHE | ReleaseRequestState |
| LOG_REQUEST | UpdateRequestCache |
| END_REQUEST | EndRequest |

***In Classic Mode the ASP.NET pipeline is run within the EXECUTE_REQUEST_HANDLER IIS Event.

### Page Lifecycle Events

#### Page_PreInit()

![Pipeline Integration][FIGURE8]
###### Figure 8, Pipeline Integration

+ Dynamic Master/Theme pages
+ Localization
+ Dynamic Control Inclusion

#### Page_Init()

![Pipeline Integration][FIGURE9]
###### Figure 9, Pipeline Integration

+ Set control properties

#### Page_InitComplete()

![Pipeline Integration][FIGURE10]
###### Figure 10, Pipeline Integration

+ Include code which must be executed after initialization is complete

#### Page_PreLoad()

![Pipeline Integration][FIGURE11]
###### Figure 11, Pipeline Integration

+ VIEWSTATE is loaded

#### Page_Load()

![Pipeline Integration][FIGURE12]
###### Figure 12, Pipeline Integration

+ Created by default and is the most common place for the execution of custom code

#### Page_LoadComplete()

![Pipeline Integration][FIGURE13]
###### Figure 13, Pipeline Integration

+ Validation
+ All controls have been loaded

#### Page_PreRender()

![Pipeline Integration][FIGURE14]
###### Figure 14, Pipeline Integration

+ Final content change before conversion to HTML and send to client

#### Page_SaveStateComplete()

![Pipeline Integration][FIGURE15]
###### Figure 15, Pipeline Integration

+ VIEWSTATE complete
+ No changes to content allowed/will not be sent
+ Code which needs complete VIEWSTATE

[FIGURE1]: ../images/2016/msdn-0723.png "Figure 1, Integrated versus Classic mode"
[FIGURE2]: ../images/2016/msdn-0737.png "Figure 2, Integrated versus Classic mode"
[FIGURE3]: ../images/2016/msdn-0738.png "Figure 3, Integrated versus Classic mode"
[FIGURE4]: ../images/2016/msdn-0739.png "Figure 4, Integrated versus Classic mode"
[FIGURE5]: ../images/2016/msdn-0740.png "Figure 5, Integrated versus Classic mode"
[FIGURE6]: ../images/2016/msdn-0741.png "Figure 6, Integrated versus Classic mode"
[FIGURE7]: ../images/2016/msdn-0742.png "Figure 7, Pipeline Integration"
[FIGURE8]: ../images/2016/msdn-0743.png "Figure 8, Pipeline Integration"
[FIGURE9]: ../images/2016/msdn-0744.png "Figure 9, Pipeline Integration"
[FIGURE10]: ../images/2016/msdn-0745.png "Figure 10, Pipeline Integration"
[FIGURE11]: ../images/2016/msdn-0746.png "Figure 11, Pipeline Integration"
[FIGURE12]: ../images/2016/msdn-0747.png "Figure 12, Pipeline Integration"
[FIGURE13]: ../images/2016/msdn-0748.png "Figure 13, Pipeline Integration"
[FIGURE14]: ../images/2016/msdn-0749.png "Figure 14, Pipeline Integration"
[FIGURE15]: ../images/2016/msdn-0750.png "Figure 15, Pipeline Integration"

[LINK1]: http://msdn.microsoft.com/en-us/library/7w2sway1(v=vs.85).aspx
[LINK2]: http://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx
[LINK3]: http://mvolo.com/breaking-changes-for-aspnet-20-applications-running-in-integrated-mode-on-iis-70/
[LINK4]: http://msdn.microsoft.com/en-us/library/system.web.ui.page_events(v=vs.110).aspx
[LINK5]: https://support.microsoft.com/en-us/kb/821268
[LINK6]: ../2015-09-create-a-w3wp-memory-dump-quick-and-easy.md
[LINK7]: 2016-IISLAB-lab-3-install-and-configure-application-initialization.md
[LINK8]: https://msdn.microsoft.com/en-us/library/system.web.configuration.processmodelsection(v=vs.110).aspx
[LINK9]: https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection(v=vs.110).aspx
[LINK10]: https://msdn.microsoft.com/en-us/library/system.net.configuration.connectionmanagementelement(v=vs.110).aspx
[LINK11]: https://msdn.microsoft.com/en-us/library/system.web.hosting.hostingenvironment(v=vs.110).aspx
[LINK12]: https://blogs.msdn.microsoft.com/benjaminperkins/2012/07/31/using-dumpthreadconfig-dtc-within-windbg/
[LINK13]: https://blogs.msdn.microsoft.com/benjaminperkins/2015/12/01/why-is-my-asp-net-application-recycling/
[LINK14]: http://referencesource.microsoft.com/#System.Web/Hosting/HostingEnvironment.cs
[LINK15]: http://referencesource.microsoft.com/System.Web/Hosting/IISUnsafeMethods.cs.html#11c3b7523aefc1ba
[LINK16]: http://referencesource.microsoft.com/System.Web/Hosting/IISUnsafeMethods.cs.html#11c3b7523aefc1ba
[LINK17]: http://referencesource.microsoft.com/mscorlib/A.html#19ddb78edbb90890
[LINK18]: http://referencesource.microsoft.com/#System.Web/Configuration/ProcessModelSection.cs
