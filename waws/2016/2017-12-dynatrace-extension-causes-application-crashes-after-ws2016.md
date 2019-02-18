# Dynatrace Extension causes application crashes after WS2016

Some customers have reported that since Windows Server 2016 their applications are crashing unexpectedly. Restarts and scaling did not help. This has been isolated to an issue with versions of the Dynatrace Extension (installed through the Azure portal) prior to v1.18. The current version of the extension is 1.18.1009 and old versions can be updated through the Extensions blade of the Web App in the Azure portal.
Our suggested action is to ***remove the extension*** from the Web App if you find this to be the root cause of issues. If that is not an option then they must update the extension. Make sure to fully investigate common application crash causes such as resource exhaustion or application errors in addition to this extension.

![Dynatrace Extension][FIGURE1]
###### Figure 1, Dynatrace Extension

Click to update the extension or delete it:

![Dynatrace Extension][FIGURE2]
###### Figure 2, Dynatrace Extension

How to find out which version your App Service is running on can be found here and here.

[FIGURE1]: ../images/2016/waws-0117.png "Figure 1, Create"
[FIGURE2]: ../images/2016/waws-0118.png "Figure 2, Dynatrace Extension"

[LINK1]: https://github.com/Azure/app-service-announcements/issues/63
[LINK2]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/12/07/how-to-check-if-azure-app-service-is-on-2016-what-version-of-iis/
