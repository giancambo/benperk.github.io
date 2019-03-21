# Lab 9: Using AppCmd

## Setup

+ Install and Configure CSharpGuitarBugs on an IIS server as described in [Lab 1][LINK1]
+ Review one of the first articles I wrote at Microsoft [here][LINK2]

## Lab - Taking/Recover a Backup

1. Open a command prompt as an Administrator, navigate to C:\windows\system32\inetsrv and enter the following command

![Using AppCmd][FIGURE1]
###### Figure 1, Using AppCmd

2. Review the contents of the backup

![Using AppCmd][FIGURE2]
###### Figure 2, Using AppCmd

3. Open the c:\windows\system32\inetsrv\config\applicationHost.config file and make a breaking change, like deleting a large portion of the file and saving it.

4. Attempt to open IIS or a browser pointing to http : //localhost and you should get some error messages

5. Restore the backup and correct the breaking change

![Using AppCmd][FIGURE3]
###### Figure 3, Using AppCmd

## Lab – Monitoring/Administering IIS

1. Open a command prompt, navigate to c:\windows\system32\inetsrv and enter AppCmd /? – This provides a list of objects and syntax of the tool. Review them.

![Using AppCmd][FIGURE4]
###### Figure 4, Using AppCmd

2. Access a few of the website on the IIS server so that the worker processes are initialized.

3. Execute: AppCmd wp /? to get the syntax for listing the active worker processes.

![Using AppCmd][FIGURE5]
###### Figure 5, Using AppCmd

4. Enter the command to list the active worker processes, notice that the PID is also provided. This piece of information is required to take a memory dump using, for example ProcDump.

![Using AppCmd][FIGURE6]
###### Figure 6, Using AppCmd

5. Execute: AppCmd Site /? to get the syntax for listing the sites on the web server.

![Using AppCmd][FIGURE7]
###### Figure 7, Using AppCmd

6. Enter the command to list the sites, notice that the bindings and ID are also listed. Did you know that when you have a multi-tenant environment and you want to look at the IIS log files for a specific website, the ID is the number which is at the end of the W3SVC* directory.

![Using AppCmd][FIGURE8]
###### Figure 8, Using AppCmd

![Using AppCmd][FIGURE9]
###### Figure 9, Using AppCmd

7. Next, make a request to the View By Manufacturer link within the CSharpGuitarBugs web site, while the request is running execute the following command: AppCmd list request

![Using AppCmd][FIGURE10]
###### Figure 10, Using AppCmd

8. What information in the result of the previous command do you find useful?

9. Execute: AppCmd stop appPool “CSharpGuitarBugs” to stop the worker process and AppCmd start appPool “CSharpGuitarBugs” to start the application pool again.

![Using AppCmd][FIGURE11]
###### Figure 11, Using AppCmd

![Using AppCmd][FIGURE12]
###### Figure 12, Using AppCmd

![Using AppCmd][FIGURE13]
###### Figure 13, Using AppCmd

## Lab – Making changes to the configuration

There are a lot of ways to make configuration to IIS, PowerShell, AppCmd, IIS Management Console, Configuration Editor, etc…, pick your tool and master it, there are better tools than other… Notepad is not a good idea…

1. In the IIS Management console, click on the CSharpGuitarBugs website and open the Authentication feature.

![Using AppCmd][FIGURE14]
###### Figure 14, Using AppCmd

2. Disable Anonymous Authentication using AppCmd by executing the following command: (check out step 6 for a tip on how to get the command)

![Using AppCmd][FIGURE15]
###### Figure 15, Using AppCmd

3. Refresh the Authentication feature in the IIS Management console and you will see it was indeed set to Disabled.

![Using AppCmd][FIGURE16]
###### Figure 16, Using AppCmd

4. You can achieve the same using Configuration Manager… With the CSharpGuitarBugs website selected, click on the Configuration Editor feature and navigate to the system.webServer/security/authentication/anonymousAuthentication property.

5. Set the enabled attribute to True

![Using AppCmd][FIGURE17]
###### Figure 17, Using AppCmd

6. Before you Apply the changes, click on the Generate Script link, notice that you get the C#, JavaScript, PowerShell (IIS 8+) and AppCmd statements for the change you just implemented.

![Using AppCmd][FIGURE18]
###### Figure 18, Using AppCmd

## Lab – IIS Management Console Worker Process Management

You can also see similar information from with the IIS Management Console by clicking on Worker Process feature.


![Install][FIGURE19]
###### Figure 19, Install

This will provide a list of all active worker process. Also notice the CPU % and Memory allocation per worker process, this is valuable information for benchmarking, right, why?


![Using AppCmd][FIGURE20]
###### Figure 20, Using AppCmd

1. Access the View by Manufacturer page in the CSharpGuitarBugs website, and notice the ‘real-time’ change in the feature.

![Using AppCmd][FIGURE21]
###### Figure 21, Using AppCmd

2. It would be nice to see which requests are being executed that are consuming the 17% of CPU. Access the View by Manufacturer page again, then double-click on the CSharpGuitarBugs Application Pool Name.


![Using AppCmd][FIGURE22]
###### Figure 22, Using AppCmd

3. This is some good information…why is this valuable information and what is it useful for?

[FIGURE1]: ../images/2016/msdn-0767.png "Figure 1, Using AppCmd"
[FIGURE2]: ../images/2016/msdn-0768.png "Figure 2, Using AppCmd"
[FIGURE3]: ../images/2016/msdn-0769.png "Figure 3, Using AppCmd"
[FIGURE4]: ../images/2016/msdn-0770.png "Figure 4, Using AppCmd"
[FIGURE5]: ../images/2016/msdn-0771.png "Figure 5, Using AppCmd"
[FIGURE6]: ../images/2016/msdn-0772.png "Figure 6, Using AppCmd"
[FIGURE7]: ../images/2016/msdn-0773.png "Figure 7, Using AppCmd"
[FIGURE8]: ../images/2016/msdn-0774.png "Figure 8, Using AppCmd"
[FIGURE9]: ../images/2016/msdn-0775.png "Figure 9, Using AppCmd"
[FIGURE10]: ../images/2016/msdn-0776.png "Figure 10, Using AppCmd"
[FIGURE11]: ../images/2016/msdn-0777.png "Figure 11, Using AppCmd"
[FIGURE12]: ../images/2016/msdn-0778.png "Figure 12, Using AppCmd"
[FIGURE13]: ../images/2016/msdn-0779.png "Figure 13, Using AppCmd"
[FIGURE14]: ../images/2016/msdn-0780.png "Figure 14, Using AppCmd"
[FIGURE15]: ../images/2016/msdn-0781.png "Figure 15, Using AppCmd"
[FIGURE16]: ../images/2016/msdn-0782.png "Figure 16, Using AppCmd"
[FIGURE17]: ../images/2016/msdn-0783.png "Figure 17, Using AppCmd"
[FIGURE18]: ../images/2016/msdn-0784.png "Figure 18, Using AppCmd"
[FIGURE19]: ../images/2016/msdn-0785.png "Figure 19, Using AppCmd"
[FIGURE20]: ../images/2016/msdn-0786.png "Figure 20, Using AppCmd"
[FIGURE21]: ../images/2016/msdn-0787.png "Figure 21, Using AppCmd"
[FIGURE22]: ../images/2016/msdn-0788.png "Figure 22, Using AppCmd"

[LINK1]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK2]: ../2011/2011-08-create-an-iis-configuration-backup.md
