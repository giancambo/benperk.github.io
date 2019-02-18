# Auto Heal your Azure Web App

If your Web App is not behaving like you expect, you might consider implementing Auto Healing.  Auto Healing for a Web App lets you take an action when certain events occurr, for example:

+ When request count breaches a certain limit
+ When requests are running slower that expected
+ When an unexpected http status code is received X number of times
+ When the process consumes more than expected memory

[Here][LINK1] is an article that describes how to configure this manually.

But did you know, you can implement the same via the KUDU console which I speak about [here][LINK2]?

Once you access the KUDU console, select Tools -> Support, as shown in Figure 1.

![auto heal web app via KUDU][FIGURE1]
###### Figure 1, auto heal web app via KUDU

When the page is rendered, select the Mitigate link, as shown in Figure 2.  You can see options for Max Requests, Status Code, Slow Requests and Memory Private Set.

![azure app service support auto healing kudu][FIGURE2]
###### Figure 2, azure app service support auto healing kudu

I wrote an article [here][LINK3] where I discussed how I manually implemented Auto Healing to resolve an issue I had, let’s do the same using the Mitigate tool.

First, click on the Status Codes link and the Add new rule button, shown in Figure 3.

![add new auto healing rule web apps azure][FIGURE3]
###### Figure 3, add new auto healing rule web apps azure

Then a pop-up is rendered, as shown in Figure 4 where you can enter the details for the rule.

![status code auto healing rule web app azure kudu][FIGURE4]
###### Figure 4, status code auto healing rule web app azure kudu

Then press the Add button to add the rule.  Next click on the Action link which is rendered after the rule is configured, as shown in Figure 5.

![when rule is triggered, what action to take auto healing web app azure][FIGURE5]
###### Figure 5, when rule is triggered, what action to take auto healing web app azure

When you click on the Action link, you are given a drop-down with options to take, as shown in Figure 6.  One of which is Recycle.

![select the action for the auto healing web app rule][FIGURE6]
###### Figure 6, select the action for the auto healing web app rule

Lastly, press the Update button to apply the rule to your Web App.

Note, if you have multiple Web Apps, you need to apply the rule onto each of the Web Apps and not just on one.  All of your Web Apps in the same App Hosting Plan will be on the same VM.  This means that all your Web Apps are competing for the same resources.

[FIGURE1]: ../images/2015/waws-0092.png "Figure 1, auto heal web app via KUDU"
[FIGURE2]: ../images/2015/waws-0093.png "Figure 2, azure app service support auto healing kudu"
[FIGURE3]: ../images/2015/waws-0094.png "Figure 3, add new auto healing rule web apps azure"
[FIGURE4]: ../images/2015/waws-0095.png "Figure 4, status code auto healing rule web app azure kudu"
[FIGURE5]: ../images/2015/waws-0096.png "Figure 5, when rule is triggered, what action to take auto healing web app azure"
[FIGURE6]: ../images/2015/waws-0097.png "Figure 6, select the action for the auto healing web app rule"

[LINK1]: https://azure.microsoft.com/en-us/blog/auto-healing-windows-azure-web-sites/
[LINK2]: ../../msdn/2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK3]: http://blogs.msdn.com/b/benjaminperkins/archive/2015/04/02/connection-timeout-timeout-expired-on-azure-web-app-site-sql-azure.aspx
