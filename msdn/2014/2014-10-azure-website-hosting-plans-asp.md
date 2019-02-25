# Azure App Service Plans (ASP)

There is a great overview of Web Hosting Plans here, so this blog is not about what they are but 2 experiences I have had with them.

+ Why do I think they exist now
+ How I recently cleaned up my subscription with them

For someone who has been using Azure App Services for some time might remember when Web Hosting Plans did not exist.  Before the inception of them, when a customer created an Azure Web App, they were prompted to select which region to place the web site into.  For example, West Europe, East US, Southeast Asia, etc…  What you ended up with was, let’s assume it’s in Standard mode, a dedicated virtual machine which could be Large, Medium or Small, could host up to 500 websites and that could be scaled to 10 instances.

But what happens if you had a web site that needed more than 10 instances of a Large virtual machine, or a lot of cohosted web sites that exceeded the same?  Without Web Hosting Plans you could not run another Standard mode virtual machine in that same region…  The only option you had was to create your additional capacity into another region, which may or may not be most preferred.  However now, you can create a new Web Hosting Plan in the same region and place another Standard Large instance in it if your requirements necessitate.

## How I recently cleaned up my subscription

The second point is how I cleaned up my subscription using Web Hosting Plans.  When WHPs were deployed it was decided to create default WHP and place your Azure Website(s) into them.  I ended up with something like that shown in Figure 1.

![Default0, Default1, Default2, etc… Web Hosting Plans][FIGURE1]
###### Figure 1, Default0, Default1, Default2, etc… Web Hosting Plans

And additionally I have something like 29 Azure Web Apps created, which wasn’t a pretty situation either, all sizes, locations and instance numbers were different, see Figure 2.  This is not a super situation to be in when you are trying to control and manage things. 

![Azure Management Console, with lots of resources][FIGURE2]
###### Figure 2, Azure Management Console, with lots of resources

The way that I reduced my Web Hosting Plans was by using the new Azure Portal located here.  Selecting Browse -> App Services, as shown in Figure 3, I was able to get a list of all my Azure App Services.

![A list of all my Azure App Services in the new Azure Portal][FIGURE3]
###### Figure 3, A list of all my Azure App Services in the new Azure Portal

I clicked on one of my Azure Web Apps as shown in Figure 4, click on the ellipses (…) and then next, on the Web Hosting Plan link.

![Select the Web Hosting Plan for your Azure Web Site][FIGURE4]
###### Figure 4, Select the Web Hosting Plan for your Azure Web Site

Clicking on the Web Hosting Plan link opened a blade which allowed me to create a new one.  What I did was create a Web Hosting Plan for the Region/Mode/Size (North Central US/Standard/S1) for each of the Regions I had an Azure App Service in.  For example I created a Web Hosting Plan named CH1S1, as shown in Figure 5.

![creating a Web Hosting Plan per region, mode and size][FIGURE5]
###### Figure 5, creating a Web Hosting Plan per region, mode and size

After creating the Web Hosting Plan in the North Central US region, I could relocate or move my Azure Web App into that Web Hosting Plan.  After creating a new Web Hosting Plan for each region/mode/size of all my web sites, I used the output from Figure 3 to consolidate all my sites.  Note, the Azure App Service must be in the same region as the Web Hosting Plan, additionally, there are limits the number of WHP per mode type you can create.  You will figure that out once you perform the migration. 

As shown in Figure 6, I use the same Web Hosting Plan link from the website blade for an Azure App Service hosted in North Europe to change between the old Web Hosting Plan to my new one.  In this example, my Azure App Service is in a DefaultServerFarm and I want to move it to my new DB3F1 WHP.  Selecting the new WHP, DB3F1, results in the Azure App Service moving to that WHP as desired.

![moving my Azure Web App to a new Web Hosting Plan][FIGURE6]
###### Figure 6, moving my Azure Web App to a new Web Hosting Plan

After all the Azure App Service were migrated to my new Web Hosting Plans, I deleted the old ones.  It took me a little time to find out how to delete them, because I was only able to find a delete button on the WHP itself initially, but onece I had no more Azure Web App linked to the WHP I couldn’t find out how to get there, but Figure 7 shows you how.

![How to delete a Web Hosting Plan which is not linked to an Azure Web App][FIGURE7]
###### Figure 7, How to delete a Web Hosting Plan which is not linked to an Azure Web App

Select Browse, then Everything -> then Web Hosting Plan.  To delete them select each one and then the delete button.  The output makes it pretty easy to find WHP with no Azure App Services as there is a count of 0 placed in the Web App column.  Zippty zat, that is that.

Now, compare to the drop-down in the Azure Management Console, shown in Figure 1, I have a much more pleasant and controllable set of Web Hosting Plans, as seen in Figure 8.  With a similar view in the new Azure Portal, illustrated in Figure 9.

![How to get more control over your Azure App Services][FIGURE8]
###### Figure 8, How to get more control over your Azure App Services

![How to get more control over your Azure App Services][FIGURE9]
###### Figure 9, How to get more control over your Azure App Services

[FIGURE1]: ../images/2014/msdn-0458.png "Figure 1, Default0, Default1, Default2, etc… Web Hosting Plans"
[FIGURE2]: ../images/2014/msdn-0459.png "Figure 2, Azure Management Console, with lots of resources"
[FIGURE3]: ../images/2014/msdn-0460.png "Figure 3, A list of all my Azure App Services in the new Azure Portal"
[FIGURE4]: ../images/2014/msdn-0461.png "Figure 4, Select the Web Hosting Plan for your Azure Web Site"
[FIGURE5]: ../images/2014/msdn-0462.png "Figure 5, creating a Web Hosting Plan per region, mode and size"
[FIGURE6]: ../images/2014/msdn-0463.png "Figure 6, moving my Azure Web App to a new Web Hosting Plan"
[FIGURE7]: ../images/2014/msdn-0464.png "Figure 7, How to delete a Web Hosting Plan which is not linked to an Azure Web App"
[FIGURE8]: ../images/2014/msdn-0465.png "Figure 8, How to get more control over your Azure App Services"
[FIGURE9]: ../images/2014/msdn-0466.png "Figure 9, management"
