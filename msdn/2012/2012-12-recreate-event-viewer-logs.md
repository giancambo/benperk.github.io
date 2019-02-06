# Recreate Event Viewer logs

If you ever receive "the event log file is corrupted" on Windows 2008 R2, try the following list of actions to fix it.  Following these steps result in the recreation of the event logs.

1. Open a command as an Administrator and enter "NET STOP EVENTLOG" as shown in Figure 1.

![stopping event logging][FIGURE1]
###### Figure 1, stopping event logging

2. Navigate to %SystemRoot%\System32\Winevt\Logs.  You can confirm where the event log are being written to by opening Event Viewer, navigating to the Windows Logs folder, selecting on one of the Event logs and clicking the Properties link on the Actions pane.  This is illustrated by Figure 2.

![where event logy are stored][FIGURE2]
###### Figure 2, where event logy are stored

3. Rename the following files:
    ..1. Application.evtx -> Application.evtx.old
    ..2. Security.evtx -> Security.evtx.old
    ..3. Setup.evtx -> Setup.evtx.old
    ..4. System.evtx -> System.evtx.old
4. Execute "NET START EVENTLOG" and you will notice that new EVTX files have been created, as shown in Figure 4.

![start event logging][FIGURE3]
###### Figure 3, start event logging

[FIGURE1]: ../images/2012/msdn-0190.png "Figure 1, stopping event logging"
[FIGURE2]: ../images/2012/msdn-0191.png "Figure 2, where event logy are stored"
[FIGURE3]: ../images/2012/msdn-0193.png "Figure 3, start event logging"
