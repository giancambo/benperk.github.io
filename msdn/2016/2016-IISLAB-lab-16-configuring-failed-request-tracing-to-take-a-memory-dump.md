# Lab 16: Configuring Failed Request Tracing to take a memory dump

## Prerequisites

+ Read this, using ADPLUS [here][LINK1]
+ Download/Read about Procdump [here][LINK2]
+ Have a look at [Lab 19][LINK3] and [Lab 20][LINK4], for other examples of Procdump
+ Review this blog [here][LINK5]

## Setup

+ Install and Configure CSharpGuitarBugs on an IIS server as described in [Lab 1][LINK6]
+ Install and Configure Failed Request Tracing on the same IIS server as described in [Lab 4][LINK7]
+ Copy PROCDUMP.exe to the c:\windows\system32 directory on the IIS server

## LAB

Perform the following to configure Failed Request Tracing to capture a memory dump

1. Configure a Server Level Failed Request Rule to run when the Status Code = 400-599 (seems to work best when the rule is base on Time Taken)

![Configuring Failed Request Tracing to take a memory dump][FIGURE1]
###### Figure 1, Configuring Failed Request Tracing to take a memory dump

![Configuring Failed Request Tracing to take a memory dump][FIGURE10]
###### Figure 2, Configuring Failed Request Tracing to take a memory dump

2. Open the IIS management console and click on the IIS Server name

3. Open the Configuration Editor

![Configuring Failed Request Tracing to take a memory dump][FIGURE2]
###### Figure 3, Configuring Failed Request Tracing to take a memory dump

4. Click on the … of the (Collection) and set customActionEnabled and enabled to TRUE

![Configuring Failed Request Tracing to take a memory dump][FIGURE3]
###### Figure 4, Configuring Failed Request Tracing to take a memory dump

5. Then, Apply the changes

6. Download and copy ProcDump

a. http://technet.microsoft.com/en-us/sysinternals/dd996900.aspx

b. Copy Procdump.exe (procdemp64.exe) into the c:\windows\system32 directory, create a c:\temp\dumps directory to store the dump

7. Click on the Failed Request Tracing… link, for the CSharpGuitarBugs website, in the Actions pane and Enable it.

![Configuring Failed Request Tracing to take a memory dump][FIGURE4]
###### Figure 5, Configuring Failed Request Tracing to take a memory dump

8. Click on the CSharpGuitarBugs Web Site, then on the Configuration Editor

![Configuring Failed Request Tracing to take a memory dump][FIGURE5]
###### Figure 6, Configuring Failed Request Tracing to take a memory dump

9. Click on the … and Add the following

a. customActionExe: c:\windows\system32\procdump.exe

b. customActionParams: -accepteula –ma %1% c:\dumps

c. customActionTriggerLimit: 50

d. path: *

![Configuring Failed Request Tracing to take a memory dump][FIGURE6]
###### Figure 7, Configuring Failed Request Tracing to take a memory dump

10. Apply the changes and click on the Search Forums link in the CSharpGuitarBugs website, or with the Time Take rule, click on the View By Manufacturer link…, notice that the %1% is the parameter that passes the PID to Procdump.

![Configuring Failed Request Tracing to take a memory dump][FIGURE7]
###### Figure 8, Configuring Failed Request Tracing to take a memory dump

![Configuring Failed Request Tracing to take a memory dump][FIGURE8]
###### Figure 9, Configuring Failed Request Tracing to take a memory dump

11. A memory dump of the W3WP process is created and placed in the c:\temp\dumps directory. You can now use WindDbg or any other memory dump analysis tool to find out the root cause of the issue.

![Configuring Failed Request Tracing to take a memory dump][FIGURE9]
###### Figure 10, Configuring Failed Request Tracing to take a memory dump

[FIGURE1]: ../images/2016/msdn-0837.png "Figure 1, Configuring Failed Request Tracing to take a memory dump"
[FIGURE2]: ../images/2016/msdn-0838.png "Figure 2, Configuring Failed Request Tracing to take a memory dump"
[FIGURE3]: ../images/2016/msdn-0839.png "Figure 3, Configuring Failed Request Tracing to take a memory dump"
[FIGURE4]: ../images/2016/msdn-0840.png "Figure 4, Configuring Failed Request Tracing to take a memory dump"
[FIGURE5]: ../images/2016/msdn-0841.png "Figure 5, Configuring Failed Request Tracing to take a memory dump"
[FIGURE6]: ../images/2016/msdn-0842.png "Figure 6, Configuring Failed Request Tracing to take a memory dump"
[FIGURE7]: ../images/2016/msdn-0843.png "Figure 7, Configuring Failed Request Tracing to take a memory dump"
[FIGURE8]: ../images/2016/msdn-0844.png "Figure 8, Configuring Failed Request Tracing to take a memory dump"
[FIGURE9]: ../images/2016/msdn-0845.png "Figure 9, Configuring Failed Request Tracing to take a memory dump"
[FIGURE10]: ../images/2016/msdn-0846.png "Figure 10, Configuring Failed Request Tracing to take a memory dump"

[LINK1]: http://blogs.msdn.com/b/friis/archive/2010/05/11/using-freb-to-generate-a-dump-on-a-long-running-request.aspx
[LINK2]: http://technet.microsoft.com/en-us/sysinternals/dd996900.aspx
[LINK3]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/06/16/lab-19-debugging-a-high-cpu-hang-w3wp-process-using-windbg/
[LINK4]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/06/16/lab-20-debugging-a-low-cpu-hang-w3wp-process-using-windbg/
[LINK5]: ../2013/2013-12-using-procdump-and-failed-request-tracing-to-capture-a-memory-dump.md
[LINK6]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK7]: 2016-IISLAB-lab-4-install-and-configure-failed-request-tracing.md
