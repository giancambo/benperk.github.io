# IIS Debugging Labs – Information and setup instructions

This is a scenarios of debugging demos focused on helping you get some hands on experience in debugging the most common type of IIS issues. For example, hang/performance, crash and memory issues.

## Installation

These labs are focused IIS 8.5, but can be run on the following versions of IIS

| IIS Version |	Operating System |
| ----------- | ---------------- |
| IIS 10 |	Windows Server 2016 |
| IIS 8.5 | Windows Server 2012 R2 |
| IIS 7.5	| Windows Server 2008 R2 |

## Test project – CSharpGuitarBugs

The labs utilize an ASP.NET MVC web application hosted on GitHub [here][LINK1]. This ASP.NET MVC web application is based on version 5 using Visual Studio 2015, therefore in Lab 2, you need to install the appropriate supporting MVC libraries and .NET Framework.

| Visual Studio Version |	.NET Version | MVC Version | ASP.NET Version |
| --------------------- | ------------ | ----------- | --------------- |
| 2008 | 3.5 | MVC 1 | ASP.NET 3.5 |
| 2010 | 4.0 | ASP.NET MVC 2/3 | ASP.NET 4.0 |
| 2012 | 4.5 | ASP.NET MVC 4 | ASP.NET 4.5 |
| 2013 | 4.5.* | ASP.NET MVC 5 | ASP.NET 4.5.* |
| 2015/16 | 4.6 |	ASP.NET Core 1 | ASP.NET 4.6 / Core 1 |

## Labs

| Lab instructions | Located here |
| ---------------- | ------------ |
| Lab 1: Install IIS and create a website | [Lab 1 √][IISL1] |
| Lab 2: Install the Web Platform Installer | [Lab 2 √][IISL2] |
| Lab 3: Install and configure Application Initialization | [Lab 3 √][IISL3] |
| Lab 4: Install and configure Failed Request Tracing | [Lab 4 √][IISL4] |
| Lab 5: Basic and Advanced Application Pool settings | [Lab 5 √][IISL5] |
| Lab 6: Using HTTP, W3SVC, WAS and W3WP | [Lab 6 √][IISL6] |
| Lab 7: Using Integrated vs. Classic mode | [Lab 7 √][IISL7] |
| Lab 8: PowerShell WebAdministration (update for IIS 10) | [Lab 8 √][IISL8] |
| Lab 9: Using AppCmd | [Lab 9 √][IISL9] |
| Lab 10: Using ASP.NET Tracing (Trace=“True“) | [Lab 10 √][IISL10] |
| Lab 11: aspnet_regiis | [Lab 11 √][IISL11] |
| Lab 12: IntelliTraceCollector | [Lab 12 √][IISL12] |
| Lab 13: LogParser | [Lab 13 √][IISL13] |
| Lab 14: ETW Tracing / PerfView |  |
| Lab 15: Debug Diagnostics | [Lab 15 √][IISL15] |
| Lab 16: Failed Request Tracing, Capture memory dump | [Lab 16 √][IISL16] |
| Lab 17: TBD |  |
| Lab 18: WinDbg - Crash | [Lab 18 √][IISL18] |
| Lab 19: WinDbg - High CPU Hang | [Lab 19 √][IISL19] |
| Lab 20: WinDbg - Low CPU Hang | [Lab 20 √][IISL20] |
| Lab 21: WinDbg - Memory consumption | [Lab 21 √][IISL21] |
| Lab 22: Deploy a custom Module and Handler | [Lab 22 √][IISL22] |
| Lab 23: Troubleshooting 400 and 404 | [Lab 23 √][IISL23] |
| Lab 25: Troubleshooting 500 |  |
| Lab 26: Troubleshooting 503 |  |
| Lab 27: SSL Certificate Troublshooting |  |
| Lab 28: The impact of debug=true | Lab 28 √][IISL28] |
| Lab 29: HTTP Caching |  |
| Lab 30: IIS Caching |  |

[LINK1]: https://github.com/benperk/CSharpGuitarBugs
[IISL1]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[IISL2]: 2016-IISLAB-lab-2-install-the-web-platform-installer.md
[IISL3]: 2016-IISLAB-lab-3-install-and-configure-application-initialization.md
[IISL4]: 2016-IISLAB-lab-4-install-and-configure-failed-request-tracing.md
[IISL5]: 2016-IISLAB-lab-5-basic-and-advanced-application-pool-settings.md
[IISL6]: 2016-IISLAB-lab-6-using-http-w3svc-was-and-w3wp.md
[IISL7]: 2016-IISLAB-lab-7-integrated-versus-classic-mode.md
[IISL8]: 2016-IISLAB-lab-8-powershell-webadministration-module.md
[IISL9]: 2016-IISLAB-lab-9-using-appcmd.md
[IISL10]: 2016-IISLAB-lab-10-using-asp-net-tracing-tracetrue.md
[IISL11]: 2016-IISLAB-lab-11-using-aspnet_regiis.md
[IISL12]: 2016-IISLAB-lab-12-intellitracecollector.md
[IISL13]: 2016-IISLAB-lab-13-using-logparser.md
[IISL15]: 2016-IISLAB-lab-15-debug-diagnostics.md
[IISL16]: 2016-IISLAB-lab-16-configuring-failed-request-tracing-to-take-a-memory-dump.md
[IISL18]: 2016-IISLAB-lab-18-debugging-a-handled-and-unhandled-exception.md
[IISL19]: 2016-IISLAB-lab-19-debugging-a-high-cpu-hang-w3wp-process-using-windbg.md
[IISL20]: 2016-IISLAB-lab-20-debugging-a-low-cpu-hang-w3wp-process-using-windbg.md
[IISL21]: 2016-IISLAB-lab-21-debugging-a-w3wp-process-with-high-memory-consumption.md
[IISL22]: 2016-IISLAB-lab-22-deploy-and-create-a-custom-module-and-handler.md
[IISL23]: 2016-IISLAB-lab-23-troubleshooting-400-and-404.md
[IISL28]: 2016-IISLAB-lab-28-the-impact-of-debugtrue.md
