# How to view the event logs of your Azure App Service

I have mentioned how to download the eventlog.xml file to view the events which are happening on your Azure App Service (Web App, Mobile App, API App, Logic App) [here][LINK1] and [here][LINK2]. You can also look at the event logs in a event viewer using KUDU, I discussed KUDU [here][LINK3]. Keep in mind that if you are running multiple instances, that you are viewing the event logs for a single instance only.

To access the event logs using KUDU, after login in select Support from the Tools menu as shown in Figure 1.

![view event logs in KUDU][FIGURE1]
###### Figure 1, view event logs in KUDU

Once the Support page renders, click on the Analyze link, then Event Viewer, as shown in Figure 2.

![Azure App Service Event Viewer details][FIGURE2]
###### Figure 2, Azure App Service Event Viewer details

[FIGURE1]: ../images/2016/msdn-0936.png "Figure 1, view event logs in KUDU"
[FIGURE2]: ../images/2016/msdn-0937.png "Figure 2, Feature"

[LINK1]: ../2014/2014-09-using-logparser-to-analyze-the-eventlog-xml-azure-websites.md
[LINK2]: ../2015/2015-04-connection-timeout-timeout-expired-on-azure-web-app-site-sql-azure.md
[LINK3]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
