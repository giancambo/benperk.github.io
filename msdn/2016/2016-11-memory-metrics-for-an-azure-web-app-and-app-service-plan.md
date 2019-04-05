# Memory metrics for an Azure Web App and App Service Plan

Did you ever wonder why memory utilization for a Web App is different than the memory utilization of the App Service Plan, as shown in Figure 1? The App Service Plan in thei example is a SMALL BASIC mode machine.

Average memory is 50.68MB which is about ~2.8% of the 1.75GB available for a SMALL BASIC. 57% memory utilization of the App Service Plan would be about 1GB.

![azure web app and app service plan memory consumption different][FIGURE1]
###### Figure 1, azure web app and app service plan memory consumption different

The answer is: each app service is its own process and consumes memory and that is what you see on the app service memory metrics, while the memory metrics you see on the App Service Plan contains all the app services in the plan.

[FIGURE1]: ../images/2016/msdn-1014.png "Figure 1, azure web app and app service plan memory consumption different"
