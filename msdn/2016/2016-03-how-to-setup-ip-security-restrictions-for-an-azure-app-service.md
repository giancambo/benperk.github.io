# How to setup IP Security Restrictions for an Azure App Service

I wrote an article [here][LINK1] that describes how to connect to a Web App from IIS manager. If you cannot do that, read down at the bottom of the article where I show the configuration you can add to the web.config manually.

[Here][LINK2] is a document that discusses this feature in more detail.

Once you have configured the App Service with the IIS Management console, you will see the IP Address and Domain Restrictions icon, similar to that in Figure 1.

![setup IP security restrictions for an Azure App Service (Web App, Mobile App, API App, Logic App)][FIGURE1]
###### Figure 1, setup IP security restrictions for an Azure App Service (Web App, Mobile App, API App, Logic App)

As a test I am going to Deny access to my IP address for this Web App. I found my IP address [here][LINK3]. With that information I added it to the Deny Restriction Rule, as shown in Figure 2.

![adding an IP deny restriction rule to an Azure App Service][FIGURE2]
###### Figure 2, adding an IP deny restriction rule to an Azure App Service

Clicked OK which added the Deny rule to the configuration. You should see the rule added to the feature pane similiar to that in Figure 3.

![adding an IP deny restriction rule to an Azure App Service][FIGURE3]
###### Figure 3, adding an IP deny restriction rule to an Azure App Service

***NOTE***: I did see a lag between the time I created this rule and the time it started acting as I expected. To make it effective quickly, I scaled up or down so my VM is recreated and it work immediately after that.  If you select Edit Feature Settings as shown in Figure 3, you can control the response (Deny Action Type) IIS sends to the client, see Figure 4.

![adding an IP deny action type an Azure App Service][FIGURE4]
###### Figure 4, adding an IP deny action type an Azure App Service

Where Unauthorized returns a 401, Forbidden a 403, Not Found a 404 and Abort, I received a 502.
You can also configure Dynamic IP Restrictions by clicking on the Edit Dynamic IP Restrictions, also shown in Figure 3 previously. Clicking on that link renders this window, Figure 5, for making the configurations.

![adding an IP deny action type an Azure App Service][FIGURE5]
###### Figure 5, adding an IP deny action type an Azure App Service

You can read about Dynamic IP Restrictions [here][LINK4] for the details. Quoted from [this][LINK4] page „The Dynamic IP Restrictions Extension for IIS provides IT Professionals and Hosters a configurable module that helps mitigate or block Denial of Service Attacks or cracking of passwords through Brute-force by temporarily blocking Internet Protocol (IP) addresses of HTTP clients who follow a pattern that could be conducive to one of such attacks.“

[FIGURE1]: ../images/2016/msdn-0605.png "Figure 1, setup IP security restrictions for an Azure App Service (Web App, Mobile App, API App, Logic App)"
[FIGURE2]: ../images/2016/msdn-0606.png "Figure 2, adding an IP deny restriction rule to an Azure App Service"
[FIGURE3]: ../images/2016/msdn-0607.png "Figure 3, adding an IP deny restriction rule to an Azure App Service"
[FIGURE4]: ../images/2016/msdn-0608.png "Figure 4, adding an IP deny action type an Azure App Service"
[FIGURE5]: ../images/2016/msdn-0609.png "Figure 5, Feature"

[LINK1]: ../2014/2014-08-configure-remote-iis-administration-for-iis-web-sites.md
[LINK2]: http://www.iis.net/configreference/system.webserver/security/ipsecurity
[LINK3]: http://www.bing.com/search?q=what%20is%20my%20ip%20address&qs=n&form=QBRE&pq=what%20is%20my%20ip%20&sc=8-14&sp=-1&sk=&ghc=2&cvid=8C18C3105C5E4987A2CAD2EB1736523E
[LINK4]: http://www.iis.net/downloads/microsoft/dynamic-ip-restrictions
