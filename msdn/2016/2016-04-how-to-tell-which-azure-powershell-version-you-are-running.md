# How to tell which Azure PowerShell version you are running

I have see a lot of articles about running complicated cmdlets searching for the version of Azure PowerShell they are running.

First, I always install Microsoft components using the Web Platform Installer, similar to that shown in Figure 1.

![how to install Microsoft Azure PowerShell][FIGURE1]
###### Figure 1, how to install Microsoft Azure PowerShell

Then to see what version it is, I access the Programs and Features window from the Control Panel, click on the Microsoft Azure PowerShell I have installed and it shows the product version, as shown in Figure 2.

![what version of Azure PowerShell am I running][FIGURE2]
###### Figure 2, what version of Azure PowerShell am I running

![what version of Azure PowerShell am I running][FIGURE3]
###### Figure 3, what version of Azure PowerShell am I running

***Note***:  also try ```(get-module azure).version```

[FIGURE1]: ../images/2016/msdn-0630.png "Figure 1, how to install Microsoft Azure PowerShell"
[FIGURE2]: ../images/2016/msdn-0631.png "Figure 2, what version of Azure PowerShell am I running"
[FIGURE3]: ../images/2016/msdn-0632.png "Figure 3, what version of Azure PowerShell am I running"
