# Cannot publish to Soveriegn Cloud

I was reading this article where it discusses some of the different kinds of Clouds that Azure supports.  For example:

+ Public Cloud
+ China Cloud
+ German Cloud
+ US Government Cloud

There may be more, but I was working on publishing an Azure App Service to the German Cloud and I was not finding my subscription in the dropdown of the Publish wizard, as shown in Figure 1. #GermanCloud #AzureGermanCloud

![cannot publish to the German Cloud][FIGURE1]
###### Figure 1, cannot publish to the German Cloud

So I downloaded the German Cloud Azure App Service Publish Profile manually as shown in Figure 2, from the portal.

![manual download of the German Cloud Azure App Services Web App publish profile][FIGURE2]
###### Figure 2, manual download of the German Cloud Azure App Services Web App publish profile

Then I Imported the downloaded *.PublishSettings file, as shown in Figure 3 and my App Service Web App published just fine.

![manually import German Cloud Azure Web App publish profile][FIGURE3]
###### Figure 3, manually import German Cloud Azure Web App publish profile

This is only a work around until the flow works the same for all instances of the cloud.

[FIGURE1]: ../images/2017/msdn-1093.png "Figure 1, cannot publish to the German Cloud"
[FIGURE2]: ../images/2017/msdn-1094.png "Figure 2, manual download of the German Cloud Azure App Services Web App publish profile"
[FIGURE3]: ../images/2017/msdn-1095.png "Figure 3, manually import German Cloud Azure Web App publish profile"
