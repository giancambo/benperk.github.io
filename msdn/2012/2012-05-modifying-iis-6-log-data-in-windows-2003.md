# Modifying IIS 6 log data in Windows 2003

All IIS logging properties are not selected by default.  Some of them, which are not selected by default can add some valuable information when troubleshooting performance or availability issues.
To modify the properties which IIS 6 logs, first select the web site which you want to log and select properties as show in Figure 1.

![display the properties of the Default Web Site][FIGURE1]
###### Figure 1, display the properties of the Default Web Site

Next, on the Web Site tab, make sure the Enable Logging check box is enabled and then select the Properties button as shown in Figure 2.

![IIS 6 logging enabled and IIS 6 log file properties button][FIGURE2]
###### Figure 2, IIS 6 logging enabled and IIS 6 log file properties button

Then select the Advanced tab on the provided window which allows the selection of properties for IIS 6 to log.  Figure 3 displays the window and properties list.

![IIS 6 Logging Properties window][FIGURE3]
###### Figure 3, IIS 6 Logging Properties window

Make the modifications, I like to store all the possible information, then select Apply, OK and OK to get back to the IIS 6 Manager console.

In IIS 6, you will need to restart IIS before the logging will be realized.

Once logging has been enhanced, you can then use the additional data to troubleshoot the current problem.  Be sure to manage your log files as the size and removal schedule may itself cause an impact on performance.

[FIGURE1]: ../images/2012/msdn-0129.png "Figure 1, display the properties of the Default Web Site"
[FIGURE2]: ../images/2012/msdn-0130.png "Figure 2, IIS 6 logging enabled and IIS 6 log file properties button"
[FIGURE3]: ../images/2012/msdn-0131.png "Figure 3, IIS 6 Logging Properties window"
