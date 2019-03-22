# Lab 12: IntelliTraceCollector

## Prerequisites

+ Read about the InteliTraceCollector [here][LINK1]
+ Read about debugging the iTrace in Visual Studio [here][LINK2]
+ Download the InteliTraceCollector [here][LINK3], view system requirements
+ Some PowerShell understanding, perform [Lab 8][LINK4]

## Setup

+ Install and Configure CSharpGuitarBugs on an IIS server as described in [Lab 1][LINK5]
+ Copy IntelliTraceCollection.exe to the IIS web server and expand into c:\IntelliTraceCollection and install it, as per the instructions in the lab…not now J
+ Create C:\IntelliTraceLogFiles directory

## Lab 12-1

1. Open PowerShell as Administrator
2. Expand the Cab file by entering the following command

![IntelliTraceCollector][FIGURE1]
###### Figure 1, MSIntelliTraceCollectorDN

3. Enter ```Set-ExecutionPolicy Unrestricted``` if required
4. Enter ```import-module c:\IntelliTraceCollector\Microsoft.VisualStudio.IntelliTrace.PowerShell.dll```

![IntelliTraceCollector][FIGURE2]
###### Figure 2, IntelliTraceCollector

5. ```Start-IntelliTraceCollection "<ApplicationPool>" <PathToCollectionPlan> <FullPathToITraceFileDirectory>```, ***TIP***: when entering the Path, just enter the first few letter then TAB.
a. Ex: ```Start-IntelliTraceCollection "WebExceptionApp" "C:\IntelliTraceCollector\collection_plan.ASP.NET.default.xml " "C:\IntelliTraceLogFiles"```

![IntelliTraceCollector][FIGURE3]
###### Figure 3, IntelliTraceCollector

6. Open the CSharpGuitarBugs website and click on the “View by 6, 7 or 8 String“ link
7. Enter the following command: ```Stop-IntelliTraceCollection "<ApplicationPool>"```. The execution of this commend generates the iTrace file.

![IntelliTraceCollector][FIGURE4]
###### Figure 4, IntelliTraceCollector

8. The iTrace file is created and stored in the C:\IntelliTraceLogFiles directory
9. Double-click on the .iTrace file which opens Visual Studio Ultimate/Enterprise, only available in Ultimate/Enterprise
10. There is a summary of Exception Data, you are interested in finding out why the System.Net.Exception was thrown. So double-click on that one.
11. Once the Symbols have been downloaded, the debugger will most likely navigate directly to the line of code which triggered the error…
a. If this is native code or the source code is not visible, copy symbols (.pbd files) into same directory as the iTrace file
12. Click on the InteliTrace tab on the right-hand side of the IDE, same place a Solution Explorer usually is. Navigate up and down the trace to see how the events and method executed leading to the exception and after the exception.

![IntelliTraceCollector][FIGURE5]
###### Figure 5, IntelliTraceCollector

[FIGURE1]: ../images/2016/msdn-0811.png "Figure 1, IntelliTraceCollector"
[FIGURE2]: ../images/2016/msdn-0812.png "Figure 2, IntelliTraceCollector"
[FIGURE3]: ../images/2016/msdn-0813.png "Figure 3, IntelliTraceCollector"
[FIGURE4]: ../images/2016/msdn-0814.png "Figure 4, IntelliTraceCollector"
[FIGURE5]: ../images/2016/msdn-0815.png "Figure 5, IntelliTraceCollector"

[LINK1]: http://msdn.microsoft.com/library/hh398365(v=vs.110).aspx
[LINK2]: http://msdn.microsoft.com/en-us/magazine/ee336126.aspx
[LINK3]: https://www.microsoft.com/en-US/download/details.aspx?id=44909
[LINK4]: 2016-IISLAB-lab-8-powershell-webadministration-module.md
[LINK5]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
