# Application Request Routing Error – 502.4 – Bad Gateway

I was setting up an Application Request Routing server and received the error shown in Figure 1 when I attempted to access the ARR URL.

![HTTP Error 502.4 – Bad Gateway][FIGURE1]
###### Figure 1, HTTP Error 502.4 – Bad Gateway

The cause was due to the Health Status of the servers being set to “Unhealthy” as shown in Figure 2.  They were set to "Unhealthy" because the Health Tests which I previously configured failed.

![Monitoring and Management, Health status set to Unhealthy][FIGURE2]
###### Figure 2, Monitoring and Management, Health status set to Unhealthy

To correct this, I first resolved the issues with my Health Tests.  I then selected one of the app* servers and in the Action pain selected “Set Server as Healthy” as shown in Figure 3.

![Monitoring and Management, Set Server as Healthy][FIGURE3]
###### Figure 3, Monitoring and Management, Set Server as Healthy

You should be aware that if the Health Test you configure fails, the server will be set to Unhealthy.

[FIGURE1]: ../images/2012/msdn-0115.png "Figure 1, HTTP Error 502.4 – Bad Gateway"
[FIGURE2]: ../images/2012/msdn-0116.png "Figure 2, Monitoring and Management, Health status set to Unhealthy"
[FIGURE3]: ../images/2012/msdn-0117.png "Figure 3, LINQ to NHibernate with Lambda Expression"
