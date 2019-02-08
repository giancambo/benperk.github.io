# Using Process Monitor to solve any problem, including DebugDiag

I attended TechReady15 and took part in a session from Mark Russinovich, the creator of Process Monitor and many if not all of the System Internals tools.  He mentioned that there is no problem which Process Monitor cannot be used to help resolve. I put that to the test when I received the error message, Figure 1, from DebugDiag while trying to analyze a memory dump.

![DebugDiag error, ShellExecute failed to display the report.  The returned code was 2][FIGURE1]
###### Figure 1, DebugDiag error, ShellExecute failed to display the report.  The returned code was 2

I started up Process Monitor and reproduced the issue.  In the amount of time it took to reproduce the error, Process Monitor had logged 100,000s of events.  No problem, this is where the filtering comes in handy.  Figure 2 illustrates the filter I used to reduce the events to just those used by the DebugDiag process.  You can get to the filter window by clicking on the filter icon, circled in red in Figure 2, selecting Filter -> Filter… or by pressing CTRL + L.

![Debugging a DebugDiag error using Process Monitor][FIGURE2]
###### Figure 2, Debugging a DebugDiag error using Process Monitor

Even after the inclusion of the filter I found that there were still almost 100,000 events, so next I turned to the Count feature.  This feature is accessible by selecting Tools -> Count Occurrences.  I am interested in the Result column, so I select that from the Column drop-down and click the Count button.  Figure 3 shows the result.

![Process Monitor -> Count Occurrences][FIGURE3]
###### Figure 3, Process Monitor -> Count Occurrences

The ACCESS DENIED value interested me a lot.  Double-clicking it automatically applies a filter for that result value.  The result is shown in Figure 4.

![Process Monitor -> Filtered view][FIGURE4]
###### Figure 4, Process Monitor -> Filtered view

Not bad, I have been able to reduce the number of events from half a million to 17.  By default, User Name is not added to the column list.  Right-click on the column and I can see which credentials are being used and are receiving the ACCESS DENIED error.  I was using my own credentials which did not have the required rights to create the required files.

## Solution

I opened DebugDiag as an administrator as shown in Figure 5 and the issue did not happen anymore.

![DebugDiag, Run as administrator][FIGURE5]
###### Figure 5, IIS

I recommend adding Process Monitor to your skill set as you can troubleshoot and resolve a lot of problems with it…even on your own machine…

[FIGURE1]: ../images/2013/msdn-0232.png "Figure 1, DebugDiag error, ShellExecute failed to display the report.  The returned code was 2"
[FIGURE2]: ../images/2013/msdn-0233.png "Figure 2, Debugging a DebugDiag error using Process Monitor"
[FIGURE3]: ../images/2013/msdn-0234.png "Figure 3, Process Monitor -> Count Occurrences"
[FIGURE4]: ../images/2013/msdn-0235.png "Figure 4, Process Monitor -> Filtered view"
[FIGURE5]: ../images/2013/msdn-0236.png "Figure 5, DebugDiag, Run as administrator"
