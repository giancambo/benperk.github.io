# IIS Service will not start (Event Id 4275)

When you are troubleshooting IIS issues there are tools like Failed Request Tracing, IIS logs, etc. which will let you see what is going on.  But what do you do when IIS itself is not working?

A common cause is that the applicationHost.config file is corrupted.  Perhaps there was a manual modification made to it and the XML structure has been compromised.  If this is the case, you may be able to simply open the IIS management console and hope that you receive a message, as shown in Figure 1.

![IIS Management console error][FIGURE1]
###### Figure 1, IIS Management console error

If the message is rendered, you can open the file in Visual Studio, navigate to the reference line number and check was is going on.
I recently worked on a case where we could not get that far.  The IIS service, I.e the World Wide Web Publishing Service and/or IIS Admin Service would not start.  It was crashing with the error shown in Figure 2.

![IISReset error message][FIGURE2]
###### Figure 2, IISReset error message

The error message is, "The IIS Admin Service or the World Wide Web Publishing Service, or a service dependent on them failed to start.  The service, or dependent services, may had an error during its startup or may be disabled."

I've troubleshot many issues over the years and have looked in the Event Viewer so many times and found no help that I sometimes don't look in it.  I don't recommend this, because in this case the error is logged into the Event Viewer and a search for the
event ID will lead to a possible solution.  Figure 3 shows the System event log entry.

![System Event Viewer log for Event ID 4275][FIGURE3]
###### Figure 3, System Event Viewer log for Event ID 4275

I have seen ErrorCode: 0xC00CE552 and 0xC00CE558.  Now the question is how to solve it.

## Solution

If you are working in a web farm environment and you have exact replicas of the websites on another server, then can back-up the existing MetaBase.xml file and copy a working copy to the broken web server.

If this is not the case, then you can navigate to c:\windows\system32\inetsrv\history where you will find some older versions of the MetaBase.xml file, as shown in Figure 4.

![MetaBase.xml history folder][FIGURE4]
###### Figure 4, MetaBase.xml history folder

Take one of these files from a time when you know the server was working and copy/paste it into the c:\windows\system32\inetsrv directory.  Do this only after you back-up the existing one.  That's it!

Rerun the IISReset command and you should get the results shown in Figure 5.

![Successful IISRestart][FIGURE5]
###### Figure 5, Successful IISRestart

***NOTE***: If you are receiving a different event id then this recomendation may not resolve your issue.

[FIGURE1]: ../images/2012/msdn-0140.png "Figure 1, IIS Management console error"
[FIGURE2]: ../images/2012/msdn-0141.png "Figure 2, IISReset error message"
[FIGURE3]: ../images/2012/msdn-0142.png "Figure 3, System Event Viewer log for Event ID 4275"
[FIGURE4]: ../images/2012/msdn-0143.png "Figure 4, MetaBase.xml history folder"
[FIGURE5]: ../images/2012/msdn-0144.png "Figure 5, Successful IISRestart"
