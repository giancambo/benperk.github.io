# Modify the Request Queue Limit, requestQueueLimit or Queue Length in IIS

When optimizing and tuning an ASP.NET application you may want to increase the value of the requestQueueLimit.  The requestQueueLimit is the maximum number of requests that can be queued by an ASP.NET process before errors get returned to the client.

## Modify Request Queue Limit within the Aspnet.config 

Below, the requestQueueLimit is configured in the Aspnet.config file for a 64 bit version of the .NET 4 Framework.  The Aspnet.config exists by default, for 64bit .Net 4, in the following directory:

C:\Windows\Microsoft.NET\Framework64\v4.0.30319

Adding the applicationPool element to the Aspnet.config, as shown in Figure 1, for the given .NET version means that all application pools running on IIS 7 in Integrated mode using this version of the .NET framework will be impacted by the modification.

![settings for the applicationPool element][FIGURE1]
###### Figure 1, settings for the applicationPool element

## Modify Queue Length for a given Application Pool

Below, shown in Figure 2, the Queue Length is configured from the Advanced Settings of a specific Application Pool.

![settings for the application pool][FIGURE2]
###### Figure 2, settings for the application pool

The Queue Length is the maximum number of requests that will be queued, when exceeded new requests will receive a 503 “Service Unavailable” response.

## Conclusion

In most cases the default configuration will work best, therefore only make modifications when you are certain of the need and impact.

[FIGURE1]: ../images/2011/msdn-0031.png "Figure 1, settings for the applicationPool element"
[FIGURE2]: ../images/2011/msdn-0032.png "Figure 2, settings for the application pool"
