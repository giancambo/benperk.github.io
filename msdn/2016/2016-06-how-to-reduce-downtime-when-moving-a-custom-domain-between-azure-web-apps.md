# How to reduce downtime when moving a custom domain between Azure Web Apps

I not only support App Service Azure Web Apps, I am a customer of them too. This is something we refer to as ‘eating our own Dogfood’ here at Microsoft. Using the services which you support is certainly a benefit when working together with a customer as it makes it easier to understand their perspective.

I had a custom domain that was bound to a SHARED Azure Web App in one subscription and I wanted to move it to a STANDARD Azure Web App in a different subscription. I did think about using [deployment slots][LINK1] and read more about [A/B Testing][LINK2], but they just didn’t seem to meet what I was trying to achieve in this case. My goal, was to move my entire website from SHARED mode in, for example Pay-As-You-Go subscription 1 to a STANDARD mode Azure Web App in to Pay-As-You-Go subscription 2.

The only thing I wasn’t certain about was the possible downtime I might have when I unbind my custom domain from my SHARED Azure Web App and bind it to my STANDARD Azure Web App. The following is how I did it, I must mention that I did have about 30 seconds of downtime, just the amount of time between the selection from my SHARED web app and the addition to my STANDARD one. However, you need to take the correct steps so that more is not incurred. Here are the steps I followed:

+ Create my web app in the new subscription and scaled to my desired size and mode
+ Test the new website using the *.azurewebsites.net URL to make sure all is ok
+ Configure the DNS CNAME at my DNS provider
+ Remove the custom domain from my SHARED Azure Web App
+ Add the custom domain to my STANDARD Azure Web App

## Creating and Testing the new Azure Website

There is not a lot I need to share on this one, just make sure you website is ready to accept the traffic about to be sent to it one your custom domain is routing the traffic there. It is good practice to have test plans and feature lists that you want to walk through before going live with a newly built/configured system. Back when I was an administrator the words ‘Testing in Production’ were taboo. However, not so much the case in the cloud. [Here][LINK3] and [here][LINK4] are 2 videos and [here][LINK5] is 1 link which I find interesting about this new concept.

## Configure the DNS CNAME to route your traffic to the new site

The DNS configuration for your custom domain name is managed by a 3rd party company. At the time of writing the article, Microsoft doesn’t support this. I have written a number of articles about ‘How to configure a custom domains with an Azure Web App” [here][LINK6] and more recently [here][LINK7]. As shown in Figure 1, I added ‘awverify.www’ as the NAME and ‘awverify.*****.azurewebsites.net’ as the DATA for the CNAME record.

![custom domain management on Azure][FIGURE1]
###### Figure 1, custom domain management on Azure

Notice that the existing DATA linked to the ‘www’ NAME was the Azure Website I had linked to the SHARED website which I was moving away from.

## Removing and adding the custom domain with limited downtime

Just as a test I navigated to the new STANDARD Azure Website and tried to add the domain, but as shown in Figure 2, that did not function, it was expected, but I like to try things like that just to see what happens.  I received "the host name www.* is already assigned to another Azure Web App".

![managing custom domain names in Azure][FIGURE2]
###### Figure 2, managing custom domain names in Azure

Then it was just a matter of getting the domain removed from my SHARED and added to my STANDARD. I think it took me 8 mouse clicks to remove and then read the custom domain, which took maybe 10 seconds maximum. There we no delay in the propagation of my domain, it was removed, re-added and again available in real-time. The end result was that shown in Figure 3, where the first operation was the removal and the second the addition.

![successful migration of custom domain][FIGURE3]
###### Figure 3, successful migration of custom domain

For my personal testing and confirmation, I took a moment, after I removed the custom domain from my SHARED website, to check what the response was from my custom domain. I did see the expected 404, as mentioned in this other post I wrote some time back.

It worked and I was happy the migration went smoothly without any significant downtime. I find this to be a very good product.

[FIGURE1]: ../images/2016/msdn-0649.png "Figure 1, custom domain management on Azure"
[FIGURE2]: ../images/2016/msdn-0650.png "Figure 2, managing custom domain names in Azure"
[FIGURE3]: ../images/2016/msdn-0651.png "Figure 3, successful migration of custom domain"

[LINK1]: http://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/
[LINK2]: http://blogs.msdn.com/b/tomholl/archive/2014/11/10/a-b-testing-with-azure-websites.aspx
[LINK3]: http://channel9.msdn.com/Shows/Azure-Friday/Azure-Websites-Testing-in-Production-Automation-with-Galin-Iliev
[LINK4]: http://channel9.msdn.com/Shows/Web+Camps+TV/Enabling-Testing-in-Production-in-Azure-Websites
[LINK5]: http://azure.microsoft.com/en-in/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/
[LINK6]: http://blogs.msdn.com/b/waws/archive/tags/custom+domains/
[LINK7]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/05/03/mapping-a-custom-domain-name-to-an-azure-app-service/
