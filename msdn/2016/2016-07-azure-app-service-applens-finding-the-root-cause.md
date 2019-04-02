# Azure App Service appLens – finding the root cause

There is a really cool new Azure App Service Web App tool called appLens that is explained in more detail [here][LINK1].  Open it by selecting the Settings –> AppLens from the App Service you are interested in analyzing, similar to that shown in Figure 1.

![AppLens shows this is an application issue not a platform issue][FIGURE1]
###### Figure 1, AppLens shows this is an application issue not a platform issue

Looking at the output of the report I see that there was some some down time on 3-JUN-2016 at around 00:30.  I then decided to take a look at the Event Viewer logs accessible via the KUDU console which I discuss [here][LINK2] and [here][LINK3].  And what I found is shown in Figure 2.

![why my azure app service web app is not working][FIGURE2]
###### Figure 2, why my azure app service web app is not working

It turns out that this was because an old issue with my web app that I discussed [here][LINK4] and [here][LINK5].

And the Warning event 2299 logged just after the one shown in Figure 2 further proves the root cause.  The detail for event 229 is:  Worker Process requested recycle due to 'Status Code' limit.

I also took a look into my IIS logs and see the 500s recorded, then after the restart all is back up and running.  I might actually fix this one day…

[FIGURE1]: ../images/2016/msdn-0938.png "Figure 1, AppLens shows this is an application issue not a platform issue"
[FIGURE2]: ../images/2016/msdn-0939.png "Figure 2, why my azure app service web app is not working"

[LINK1]: https://blogs.msdn.microsoft.com/appserviceteam/2016/06/02/applens/
[LINK2]: 2016-07-how-to-view-the-event-logs-of-your-azure-app-service.md
[LINK3]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK4]: ../../waws/2015/2015-11-auto-heal-your-azure-web-app.md
[LINK5]: ../2015/2015-04-connection-timeout-timeout-expired-on-azure-web-app-site-sql-azure.md
