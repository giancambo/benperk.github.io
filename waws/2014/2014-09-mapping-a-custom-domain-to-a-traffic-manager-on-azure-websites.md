# Mapping a custom domain to a Traffic Manager on Azure Websites

I wrote an article [here][LINK1] about using Traffic Manager with your Azure Web App, please take a look at it to get more context for this article.  

When you create a Traffic Manager, for example, with a name of EUROPE, similar to that shown in Figure 1, the URL which you must direct your requests to is http://*.trafficmanager.net.  When requests are made to that URL, the requests are routed to your endpoints configured for the Traffic Manager.

![The Traffic Manager URL][FIGURE1]
###### Figure 1, The Traffic Manager URL

Assuming you have setup your Azure Web App in different regions so that load is managed across them, the most appropriate Load Balancing Method could be the Round Robin method.  But do you want users to access your website using the Traffic Manager URL, probably not, you would prefer that your users access your site using your custom domain instead.  There is a description of how to do that [here][LINK2], but I want to expand on it some. 

Let's say you want bind this domain, ex: http://www.***.cool so that requests to that cool domain get handled by my EUROPE Traffic Manager.  To do this, first login to your domain provider and add a CNAME similar to that shown in Figure 2.

![configure custom domain for an Azure Web Site Traffic Manage][FIGURE2]
###### Figure 2, configure custom domain for an Azure Web Site Traffic Manage

Next, save the configuration and once propagated, the custom URL will point to the Azure Website Traffic Manager.  I waited for some time and accessing my custom URL resulted in a 404 Web Site not found error, similar to that shown in Figure 3.  I wrote another article about why this issue happens and how to resolve it here.

![404 web site not found when configuring Traffic Manager with a custom domain][FIGURE3]
###### Figure 3, 404 web site not found when configuring Traffic Manager with a custom domain

In this case, what I needed to do is configure the custom domain for each of the ENDPOINTS which I had configured for the Traffic Manager.  As shown in Figure 4 and referred to in this [article][LINK4], I had configured two Azure Web App, one in West Europe and on in North Europe.  I will need to follow [these instructions][LINK5] for both (all) of the Azure Web App so that I can attach my custom domain to the Traffic Manager URL and have it render as expected.

![Azure Web Site Traffic Manager Custom Domain][FIGURE4]
###### Figure 4, Azure Web Site Traffic Manager Custom Domain

After adding the custom domain to both/all of the Azure Web App configured as ENDPOINTS in the Traffic Manager, requests to my custom domain rendered as expect.

****Note**** that the support for naked domains are currently unsupported.

See the list of articles below for information about that and how to map other types of custom domains to your Azure Website.

+ Mapping a subdomain to an Azure Web App, here
+ Mapping a naked domain to an Azure Web App, here
+ Mapping a custom domain to a Traffic Manager domain, here
+ Mapping a custom domain that includes a special character, here
+ Mapping a wildcard domain to an Azure Web App, here

[FIGURE1]: ../images/2014/waws-0047.png "Figure 1, The Traffic Manager URL"
[FIGURE2]: ../images/2014/waws-0048.png "Figure 2, configure custom domain for an Azure Web Site Traffic Manage"
[FIGURE3]: ../images/2014/waws-0049.png "Figure 3, 404 web site not found when configuring Traffic Manager with a custom domain"
[FIGURE4]: ../images/2014/waws-0050.png "Figure 4, Azure Web Site Traffic Manager Custom Domain"

[LINK1]: tbd
[LINK2]: http://msdn.microsoft.com/en-US/library/azure/hh744837.aspx
[LINK3]: ../2013/2013-10-404-file-or-directory-not-found-when-mapping-custom-domain-to-a-windows-azure-web-site-server-error.md
[LINK4]: tbd
[LINK5]: http://blogs.msdn.com/b/waws/archive/2014/01/22/three-easy-steps-to-add-a-custom-domain-to-a-waws-site.aspx
