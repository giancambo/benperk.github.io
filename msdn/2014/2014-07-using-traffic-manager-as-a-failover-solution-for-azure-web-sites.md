# Using Traffic Manager as a FAILOVER solution for Azure App Services

I wanted to share an experience I had when using the Azure Traffic Manager with 2 Microsoft Azure App Service Web Apps.  If you are not clear on what the Traffic Manager does, read my other post [here][LINK1].  Simply, it is a way to balance load across multiple identical instances of an application so that requests can continue to be responded to when there is a performance issue or downtime incident on 1 or more of the web sites.  Kind of like the behaviour you would experience in a web farm, but better, IMO.

***NOTE***:  you will notice that FAILOVER only exists in the Traffic Manager profiles (classic) mode.  In classic you have Performance, Round Robin and Failover, and the ARM version in the new portal [here][LINK2] you have Performance, Weighted and Priority.  Keep in mind that this blog is a little older and that FAILOVER is only available in classic mode, I.e. in the AUX portal [here][LINK3].

As shown in Figure 1, I have created a Traffic Manager and added 2 Microsoft Azure App Service Web Apps to it.

![A Microsoft Azure Traffic Manager with two Microsoft Azure App Service Web Apps][FIGURE1]
###### Figure 1, A Microsoft Azure Traffic Manager with two Microsoft Azure App Service Web Apps

I can also do an NSLOOKUP command for the traffic manager as illustrated in Figure 2.

![doing an NSLOOKUP on the traffic manager][FIGURE2]
###### Figure 2, doing an NSLOOKUP on the traffic manager

What I see in Figure 2 is the results of the NSLOOKUP for the traffic manager, plus one of the Web Apps linked to it.  Why do I see only one?  If you look on the CONFIGURE page for the traffic manager, as shown in Figure 3, you will see that IISLOGS is the first Web App in the FAILOVER PRIORITY LIST.  What I have found is that in a scenario where the LOAD BALANCING METHOD is FAILOVER, that this is the location where requests are sent to.  Don’t confuse this with a ROUND ROBIN or PERFORMACE configuration.  This is the FAILOVER solution.

![Traffic Manager FAILOVER PRIORITY LIST for Microsoft Azure Web Sites][FIGURE3]
###### Figure 3, Traffic Manager FAILOVER PRIORITY LIST for Microsoft Azure Web Sites

If you change the FAILOVER PRIORITY LIST order as shown in Figure 4, then perform a NSLOOKUP again, as shown in Figure 5, you will notice the change in the link between the traffic manager and the Web Apps.

![change FAILOVER PRIORITY LIST for a traffic manager][FIGURE4]
###### Figure 4, change FAILOVER PRIORITY LIST for a traffic manager

![different Web Apps linked to traffic manager][FIGURE1]
###### Figure 1, different Web Apps linked to traffic manager

To test, I access the traffic manager URL and am redirected to the RASKULLS Web App as expected.  Next I shutdown that Web App using the [Microsoft Azure PowerShell][LINK4] command STOP-AZUREWEBSITE as shown in Figure 6 and the Microsoft Azure Console in Figure 7.

![stop a Microsoft Azure App Service Web App using Microsoft Azure PowerShell][FIGURE6]
###### Figure 6, stop a Microsoft Azure App Service Web App using Microsoft Azure PowerShell

***NOTE***: Prior to shutting down the RASKULLS Web App, if I turn off IISLOGS, I would not expect any impact.

When I access the traffic manager URL again I am indeed redirected to that IISLOGS website.  This is exactly the way I would expect the FAILOVER configuration to work.

![a stopped Microsoft Azure App Service Web App on the Traffic Manager page][FIGURE7]
###### Figure 7, a stopped Microsoft Azure App Service Web App on the Traffic Manager page

[FIGURE1]: ../images/2014/msdn-0406.png "Figure 1, A Microsoft Azure Traffic Manager with two Microsoft Azure App Service Web Apps"
[FIGURE2]: ../images/2014/msdn-0407.png "Figure 2, doing an NSLOOKUP on the traffic manager"
[FIGURE3]: ../images/2014/msdn-0408.png "Figure 3, Traffic Manager FAILOVER PRIORITY LIST for Microsoft Azure Web Sites"
[FIGURE4]: ../images/2014/msdn-0409.png "Figure 4, change FAILOVER PRIORITY LIST for a traffic manager"
[FIGURE5]: ../images/2014/msdn-0410.png "Figure 5, different Web Apps linked to traffic manager"
[FIGURE6]: ../images/2014/msdn-0411.png "Figure 6, stop a Microsoft Azure App Service Web App using Microsoft Azure PowerShell"
[FIGURE7]: ../images/2014/msdn-0412.png "Figure 7, a stopped Microsoft Azure App Service Web App on the Traffic Manager page"

[LINK1]: 2014-06-using-traffic-manager-with-microsoft-azure-web-site.md
[LINK2]: https://portal.azure.com/
[LINK3]: https://manage.windowsazure.com/
[LINK4]: http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/
