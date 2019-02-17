# Create an Azure Web App failover solution on a budget

When you choose a region to place your Web App into, it is placed onto a stamp or scale unit.  The scale unit is an isolated entity within the region or datacenter.  This entity encapsulates and protects the Web Apps contained within it from service interruptions that may happen in other scale units.   When you experience a disruption of service with your Web App, it probably becomes apparent that your entire business is running on a single 2 or 4 CPU virtual machine at a single location.

For some customers or small businesses, limited amounts of unavailability is acceptable, understandably unwanted, but, some limited amount of interruption, may not have a massive impact.  On the other hand, when your Web App is generating revenue and every minute/hour of unavailability costs you real money or credibility, you may want to consider taking some actions to reduce the impact.  If the latter is your case, would you really place everything you have onto a single 2 or 4 CPU VM?  Here is something you can do which will not cost a lot, but can result in getting your customers back to a working site in moments.

1. Configure Azure Traffic Manager (ATM) with a single ENDPOINT pointing to your primary Web App
2. Bind your custom domain to Azure Traffic Manager (ATM)
3. Create a replica of your Web App in another region, scaled to FREE mode
4. Fail over to the replica if there are service interuptions

## Configure Traffic Manager

I have written a few blogs about Azure Traffic Manager (ATM) [here][LINK1] and [here][LINK2].   Note that you can only configure Web Apps running in Standard or Premium mode into ATM.  I wouldn’t think being in STANDARD mode is an issue if your Web App is critical enough to need this solution.  Nonetheless, review the above articles which walk you through the configuration of ATM.  Add a single ENDPOINT to your primary Web App using, for example, the Round Robin load balancing method. 

## Binding a custom domain to Traffic Manager

I wrote this article which describes how to bind your custom domain to ATM.  Once successfully bound all your traffic is routed through ATM directly to your primary Web App ENDPOINT.

### Create a replica Web App in another region

The feasibility of this step is completely dependent on the coding and configuration complexities of your Web App.  There are 2 important points to make here:

1. Pick a region which is geographically nearest to your primary Web App.  For example, if your primary Web App is in North Europe, place you secondary Web App in West Europe.  Similarly, if your primary is in East US, place the secondary in Central US.
2. Point #1 is important because the secondary instance could/should continue to point to any databases or storage accounts in the primary location.  In most occurrences of Web App interruptions, databases and/or storage accounts in the primary region remain available. 

Moving, synchronizing and validating real-time data would be too time consuming for implementing this solution during the short service interruption.  Web App service interruptions are rare, but they do happen, and for those impacted, having this secondary instance on stand-by can solve the issue for you really fast.  Although there may be some performance impact due to the Web App being physically running in a different geographic location, it will be only for a short amount of time and it is better than your Web App being completely inaccessible.

Once successfully deployed to a secondary region, leave any configurations pointing to your database and storage accounts the same and run through a round of functional testing.  Then scale it down to FREE mode.  In this mode, you are not charged.

***NOTE***:  There is a charge for Azure Traffic Manager (ATM) as you can see [here][LINK3], but IMO it is worth the peace of mind to pay those extra cents.  Especially considering, as I mentioned earlier, that you are likely running your entire business on a 4 CPU virtual machine in a single location.

## Failover steps

Follow these steps when you become aware of a service interruption that is impacting your Web App.

1. Scale your secondary Web App to STANDARD
2. Add your secondary Web App as an ENDPOINT for the specific ATM, configured earlier
3. Disable the primary ENDPOINT so customer requests are not routed there

After doing this, requests to the custom domain bound to the ATM are flowing to your secondary Web App.  Although it is costing you some money having the second STANDARD mode Web App, you are not losing revenue or credibility.  If you are concerned about the cost, consider scaling down your primary instance to FREE until the service interruption is mitigated.  (Be aware though, that when you move traffic back to your primary Web App, it must be in STANDARD mode to be selectable as an ENDPOINT)

When all is over, route the traffic back to your primary Web App.  This is recommended as it is best to have your Web App in the same geographic location as any backend database, storage account or other kinds of processing.  Pointing requests back to your primary Web App can be done by following these steps:

1. Enable the primary ENDPOINT in ATM
2. Disable the secondary ENDPOINT in ATM
3. Scale your secondary Web App to FREE mode

## Conclusion

I know firsthand that every service interruption goes through thorough analysis to see why it happened and corrections are made as required.  Working on support, I directly hear you, the customer, when there is a service interruption, and I write this as a means to make the impact smaller.

[LINK1]: tbd
[LINK2]: tbd
[LINK3]: tbd
[LINK4]: http://azure.microsoft.com/en-us/pricing/details/traffic-manager/
