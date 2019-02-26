# IIS Admin Service fails to start

I was working on an unrelated issue and all of a sudden I started getting some strange errors in IIS.  Something like the errors shown in Figure 1 and Figure 2.  Figure 1 was rendered when I opened the IIS management console and attempted to expand the server name to see the websites on the server.  The error is:  There was an error when trying to connect.  DO you want to retype your credentials and try again?  Details: Creating an instance of the COM component with CLSID {} from the IClassFactory failed due to the following error:  800700b7 Cannot create a file when that file already exists. (Exception from HRESULT: 0x800700B7).  The difficult thing about that message is that I had no idea what file it was talking about.

![Failed to connect, there was an error when trying to connect. COM component with CLSID][FIGURE1]
###### Figure 1, Failed to connect, there was an error when trying to connect. COM component with CLSID

Additionally, IIS was no longer running and may attempts to restart it resulted in this error: The IIS Admin Service or the World Wide Web Publishing Service, or a service dependent on them failed to start.  The service, or dependent services, may had an error during its startup or may be disabled.  However, when I look in the Services console, all the dependent services were running and functional.

![cannot start IIS using IISReset, Restart attempt failed][FIGURE2]
###### Figure 2, cannot start IIS using IISReset, Restart attempt failed

The first action I took was to try and find which file was being referred to Figure 1.  I assumed a process had a handle on a file which IIS needed to create or re-create, so I used [Handle][LINK1] to take a look around.  After lots of time looking at all the open handles and determining none of them would be impacting the IIS management console, I decided to use Process Monitor, the tool that can solve almost every problem.

After I opened [Process Monitor][LINK2], I reproduced the problem and filtered the logs where Process Name = InetMgr.exe.  I initially looked through the Result column for an error of some kind, but saw everything was SUCCESS, as shown in Figure 3.  I concluded that what was happening at the end of the log, must be what was causing the issue.

![Process Monitor to solve IISReset failed to start][FIGURE3]
###### Figure 3, Process Monitor to solve IISReset failed to start

I had been working on a WebDAV issue and remembered creating a copy of the schema file.  I removed the WebDAV_schema.xml copy and the issue went away, as shown in Figure 4.

![IISReset successfully started][FIGURE4]
###### Figure 4, IISReset successfully started

The lessons I learned is that the issue will not always show up as an ERROR in the log and what comes towards the end of the log for this kind of issue, is likely the cause of the issue.  Once resolved I could get back to working on my original problem…

[FIGURE1]: ../images/2014/msdn-0473.png "Figure 1, Failed to connect, there was an error when trying to connect. COM component with CLSID"
[FIGURE2]: ../images/2014/msdn-0474.png "Figure 2, cannot start IIS using IISReset, Restart attempt failed"
[FIGURE3]: ../images/2014/msdn-0475.png "Figure 3, Process Monitor to solve IISReset failed to start"
[FIGURE4]: ../images/2014/msdn-0476.png "Figure 4, IISReset successfully started"

[LINK1]: http://technet.microsoft.com/en-us/sysinternals/bb896655.aspx
[LINK2]: http://technet.microsoft.com/en-us/sysinternals/bb896645.aspx
