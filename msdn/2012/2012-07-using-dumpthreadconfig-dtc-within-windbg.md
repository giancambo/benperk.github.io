# Using !DumpThreadConfig (!dtc) within WinDbg

I have written articles about how to capture a W3WP memory dump on [Windows 2003][LINK1] and [Windows 2008][LINK2].  Once you have the memory dump, you need to analyze it to find out what is causing the problem.  One command I use while analyzing a memory dump is !DumpThreadConfig or !dtc.  Figure 1 shows the output of this command.

![results of a !dtc command][FIGURE1]
###### Figure 1, results of a !dtc command

I look first at the value of autoConfig.  The default value is true and means that ASP.NET will automatically configure the attributes shown in the output of !dtc.  The attributes are:

| Attribute | Description |
| --------- | ----------- |
| MaxWorkerThreads | Limits the maximum number of worker threads. |
| MaxIoThreads | Limits the maximum number of completion threads. |
| minFreeThreads | Determines how many worker threads and completion threads must be available to start a remote request. |
| minLocalRequestFreeThreads | Determines how many worker threads and completion threads must be available to start a local request. |
| maxConnection | Determines how many connections can be made to a specific IP address. |

These settings can be edited in the machine.config file for the specific version of the .NET installation.  Modifications to this configuration impact all ASP.NET applications running on the server and require an application restart. The autoConfig setting is an attribute within the [processModel][LINK3] element found within the machine.config file, as shown in Figure 2.

![default autoConfig setting found in the machine.config file][FIGURE2]
###### Figure 2, default autoConfig setting found in the machine.config file

When changes are made to the [processModel][LINK3] attributes, the results of the !dtc command displays the autoConfig as being false.  When I find this situation I always ask why the default settings have been altered.  It is ok to change them; however it is important that the administrator has the understanding of how changes impact all applications on that server.  A detailed and deeper explanation of these attributes is found [here][LINK4].

There are many commands which I use to get an understanding of what is happening at the time the memory dump is created.  This is simply one example with further explanation into how it links with the specific XML based configurations of the application.

[LINK1]: ../2011/2011-12-creating-a-w3wp-memory-dump-on-windows-server-2003.md
[LINK2]: ../2012/2012-02-creating-a-w3wp-memory-dump-on-windows-server-2008-r2.md
[LINK3]: http://msdn.microsoft.com/en-us/library/vstudio/7w2sway1(v=vs.100).aspx
[LINK4]: http://support.microsoft.com/kb/821268/en-us

[FIGURE1]: ../images/2012/msdn-0149.png "Figure 1, results of a !dtc command"
[FIGURE2]: ../images/2012/msdn-0150.png "Figure 2, default autoConfig setting found in the machine.config file"
