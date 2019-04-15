# How to connect a PowerShell client to the German Cloud

As you many know there is a new sovereign instance of Azure in Germany the the German Cloud.  #GermanCloud

There are some differences between then, for example you need a new subscription, the one for the public cloud does not transfer over, find out about that here.  Also, there is a different address for the Azure Germany German Cloud management portal, here it is.

However, this article is about connecting a PowerShell client to the German Cloud.  I wrote numerous other Azure related PowerShell articles, like the below:

+ Moving an Azure App Service
+ Deploy an App Service using Azure PowerShell to a Deployment Slot
+ How to tell which Azure PowerShell version you are running
+ Using KUDU with Microsoft Azure Web Apps
+ All my PowerShell articles

To connect your PowerShell client to the #GermanCloud #AzureGermanCloud #AzureSovereignCloud execute the following:

```
Login-AzureRmAccount -EnvironmentName AzureGermanCloud
Set-AzureRmContext -SubscriptionId "########-####-####-####-############"
```

After entering the Login-AzureRmAccount command which include the –EvironmentName of AzureGermanCloud you are prompted to enter your credentials.  Enter the credentials and, just in case you have multiple subscriptions in the German Cloud run the Set-AzureRmContext to the desired Subscription.  You should see something similar to that shown in Figure 1.

![how to connect PowerShell to German Cloud][FIGURE1]
###### Figure 1, how to connect PowerShell to German Cloud

[FIGURE1]: ../images/2017/msdn-1155.png "Figure 1, how to connect PowerShell to German Cloud"
