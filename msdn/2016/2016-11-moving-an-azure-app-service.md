# Moving an Azure App Service

I like keeping my things in order, so over time as my Azure consumption grows, I begin to recognize and learn that maybe I should have put all the related resources into the same resource group.  Here is an example of how I moved an App Service Web app from one resource group to another.

See [here][LINK1] for the supported move configurations.

As shown in Figure 1, notice that I have two resource groups, one named FROM, linked to West Europe and another named TO which is linked to West US.

![Azure resource groups, move an Azure App Service Web App][FIGURE1]
###### Figure 1, Azure resource groups, move an Azure App Service Web App

I have a Web App in the FROM resource group hosted on an App Service Plan in West Europe, I.e. AM2, that I want to move West US, I.e. BAY, as shown in FIgure 2.

![moving an Azure Web App to a different resource group][FIGURE2]
###### Figure 2, moving an Azure Web App to a different resource group

Now, run the following PowerShell cmdlets to get the necessary information to move the Web App.

Execute this first command to get the ResourceId for use with the next PowerShell command, as shown in Figure 3, the ResourceId is rendered.

```Get-AzureRmResource -ResourceGroupName FROM -ResourceName InAM2moveToBAY -ResourceType Microsoft.Web/sites```

![getting the ResourceId for moving an Azure App Service Web App][FIGURE3]
###### Figure 3, getting the ResourceId for moving an Azure App Service Web App

Next, execute the following command to move the Web App to the TO resource group, when prompted, as shown in Figure 4, select Yes if you want to really change the resource group.

```Move-AzureRmResource -DestinationResourceGroupName TO -ResourceId /subscriptions/*******-****-****-****-******/resourceGroups/FROM/providers/Microsoft.Web/sites/InAM2moveToBAY```

![confirm the move of the azure app service web app][FIGURE4]
###### Figure 4, confirm the move of the azure app service web app

Check the configuration of the App Service and you will see that the Web App is now in the TO resource group as shown in Figure 5.

![the azure app service web app successfully moved][FIGURE5]
###### Figure 5, the azure app service web app successfully moved

Notice that the Resource Group is now set to TO as desired.  Also notice however, that the Location and App Service Plan have not been changed.  The movement of the Web App to another resource group is only a logical, or reference data change, the physical location of the Web App does not move.  In my opinion, if you need to move the actual location of your Web App, my recommendation is to do it manually or clone it as discussed [here][LINK2].  You can move the Web App to a different App Service Plan (ASP) from the portal by selecting Change App Service Plan from the settings blade.  The result opens a blade showing the other ASPs in that same scale unit to which the Web App can be moved to.

## Moving resources within the portal

Instead of using PowerShell you can also move the resources from within the Azure Portal, as shown in Figure 6.  For example click on the existing Resource Group where the resource exists then click the MOVE button which renders the Move Resources blade.  Select the resource that can be moved and, from the dropdown select the Resource group to which you want them to be moved to.

![moving an Azure App Service Web App to another resource group][FIGURE6]
###### Figure 6, moving an Azure App Service Web App to another resource group

Notice that the FROM resource group is located in West Europe and has 3 Web Apps in 2 different regions.  Also, not shown above, all 3 of the Web App are within different App Service Plans.  Navigate to the Resource Group to which these Web Apps were moved to and you will see them there, as shown in Figure 7.

![moving an Azure App Service Web App to another resource group][FIGURE7]
###### Figure 7, moving an Azure App Service Web App to another resource group

## Other options and links

Here are some helpful links on the subject.

+ [Supported Move Configurations][LINK3]
+ [Move resources to new resource group or subscription][LINK4]
+ [App Service limitations][LINK5]
+ [Azure App Service App Cloning Using Azure Portal][LINK6]
+ [Azure App Service App Cloning Using PowerShell][LINK7]
+ [Ability to move Web Apps to a different Resource Group][LINK8] –> you can move your (web) app between resource groups but you can’t move it to another app service plan if the destination app service plan is in a different resource group than the source resource group, even if both app service plans are on the same scale unit.

[FIGURE1]: ../images/2016/msdn-1015.png "Figure 1, Azure resource groups, move an Azure App Service Web App"
[FIGURE2]: ../images/2016/msdn-1016.png "Figure 2, moving an Azure Web App to a different resource group"
[FIGURE3]: ../images/2016/msdn-1017.png "Figure 3, getting the ResourceId for moving an Azure App Service Web App"
[FIGURE4]: ../images/2016/msdn-1018.png "Figure 4, confirm the move of the azure app service web app"
[FIGURE5]: ../images/2016/msdn-1019.png "Figure 5, the azure app service web app successfully moved"
[FIGURE6]: ../images/2016/msdn-1020.png "Figure 6, moving an Azure App Service Web App to another resource group"
[FIGURE7]: ../images/2016/msdn-1021.png "Figure 7, Feature"

[LINK1]: https://azure.microsoft.com/en-us/documentation/articles/app-service-move-resources/
[LINK2]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-app-cloning-portal/
[LINK3]: https://azure.microsoft.com/en-gb/documentation/articles/app-service-move-resources/
[LINK4]: https://azure.microsoft.com/en-gb/documentation/articles/resource-group-move-resources/
[LINK5]: https://azure.microsoft.com/en-gb/documentation/articles/resource-group-move-resources/#app-service-limitations
[LINK6]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-app-cloning-portal/
[LINK7]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-app-cloning/
[LINK8]: https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/8274639-ability-to-move-web-apps-to-a-different-resource-g?tracking_code=527e68102c912a972692fd5b41ceedbd
