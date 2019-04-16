# Enable logging for your Hybrid Connection Manager, troubleshooting

This article pertains to the Azure Biztalk based Hybrid Connection feature discussed [here][LINK1].

To read about the new version of the Hybrid Connection feature based on Azure Relay, read [this][LINK2] article.

#HybridConnection #Azure #Microsoft - There are 2 kinds of logging you can use to troubleshoot any issues with your Azure Hybrid Connection Manager.  The first one is to enable System.Net tracing as I describe here.  And the second is to add an App Setting named HYBRIDCONNECTIVITY_LOGGING_ENABLED with a value of 1 to the Azure App Service that is using the hybrid connection.  As shown in Figure 1.

![troubleshoot a troublesome Hybrid Connection Manager, Azure, Azure App Service, Web App][FIGURE1]
###### Figure 1, troubleshoot a troublesome Hybrid Connection Manager, Azure, Azure App Service, Web App

You will need to restart the App Service.  After the restart, wait a few moments and a log file named HybridConnectivity_UserLog will be written into the LogFiles directory.  You can get them via FTP or the KUDU console as I discuss here.  See Figure 2 and Figure 3.

![download Hybrid Connection Manager logs using FTP][FIGURE2]
###### Figure 2, download Hybrid Connection Manager logs using FTP

![download or view Hybrid Connection Manager via KUDU/SCM][FIGURE3]
###### Figure 3, download or view Hybrid Connection Manager via KUDU/SCM

## TCP Ports

You can find the following and additional information information here.  (Hybrid Connection Overview)

| Port | Why you need it |
| ---- | --------------- |
| 9350 - 9354 | These ports are used for data transmission. The Service Bus relay manager probes port 9350 to determine if TCP connectivity is available. If it is available, then it assumes that port 9352 is also available. Data traffic goes over port 9352.  Allow outbound connections to these ports. |
| 5671	| When port 9352 is used for data traffic, port 5671 is used as the control channel.  Allow outbound connections to this port. |
| 80, 443	| These ports are used for some data requests to Azure. Also, if ports 9352 and 5671 are not usable, then ports 80 and 443 are the fallback ports used for data transmission and the control channel.  Allow outbound connections to these ports.
Note It is not recommended to use these as the fallback ports in place of the other TCP ports. The HTTP/WebSocket is used as the protocol instead of native TCP for data channels. It could result in lower performance. | 

If you are getting poor performance you may be using the HTTP/Websocket ports instead of the native TCP ports.

To test if a specific port is open and functional, from the machine where you are running your Hybrid Connection mananger, execute the following:

```tcping <serverOnNetwork> <port>```

Where <serverOnNetwork> is some service like a database that an Azure App Service Web App would be connecting to.  And the <port> is the port number you have configured the Hybrid Connection manager to use in the Azure Portal, here.

The output will show the speed of the pings and whether the port is actually working.  If you are getting poor performance, make sure to use and that outbound connections are configured for the ports identitifed in the above table.  Consider port 5671 as well.

My Azure Hybrid Connection is slow.  Why is my Hybrid Connection Manager slow?  How to increase Hybrid Connection Manager speed.  Make Hybrid Connection Manager faster.

[FIGURE1]: ../images/2017/msdn-1156.png "Figure 1, troubleshoot a troublesome Hybrid Connection Manager, Azure, Azure App Service, Web App"
[FIGURE2]: ../images/2017/msdn-1157.png "Figure 2, download Hybrid Connection Manager logs using FTP"
[FIGURE3]: ../images/2017/msdn-1158.png "Figure 3, download or view Hybrid Connection Manager via KUDU/SCM"

[LINK1]: https://docs.microsoft.com/en-us/azure/biztalk-services/integration-hybrid-connection-overview
[LINK2]: https://docs.microsoft.com/en-us/azure/app-service/app-service-hybrid-connections
