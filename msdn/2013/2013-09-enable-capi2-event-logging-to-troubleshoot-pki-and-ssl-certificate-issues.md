# Enable CAPI2 event logging to troubleshoot PKI and SSL Certificate Issues

The CryptoAPI 2.0 Diagnostics is a feature available on Windows Server 2008+ that supports the trouble shooting of issues concerned with, for example:
Certificate Chain Validation
Certificate Store Operations
Signature Verification

This article describes how to enable the CAPI2 Diagnostic, but for an in depth review of the capability, check [here][LINK1].

Enable CAPI2 logging by opening the Event Viewer and navigating to the Event Viewer (Local)\Applications and Services Logs\Microsoft\Windows\CAPI2 directory and expand it.  You should see a view named Operational as illustrated in Figure 1.

![CAPI2 Diagnostics in Event Viewer][FIGURE1]
###### Figure 1, CAPI2 Diagnostics in Event Viewer

Next, right-click on the Operational view and click the Enable Log menu item as shown in Figure 2.

![Enable CAPI2 Event Logging][FIGURE2]
###### Figure 2, Enable CAPI2 Event Logging

Once enabled, any warnings or errors are logged into the viewer.  Reproduce the problem you are experiencing and check if the issue is logged.

[FIGURE1]: ../images/2013/msdn-0273.png "Figure 1, CAPI2 Diagnostics in Event Viewer"
[FIGURE2]: ../images/2013/msdn-0274.png "Figure 2, Enable CAPI2 Event Logging"

[LINK1]: http://technet.microsoft.com/en-us/library/cc749296(v=WS.10).aspx
