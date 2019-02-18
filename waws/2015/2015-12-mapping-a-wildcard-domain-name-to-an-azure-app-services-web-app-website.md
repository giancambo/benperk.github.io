# Mapping a wildcard domain name to an Azure App Services Web App (Website)

The process for mapping a wildcard domain, for example *.contoso.com to a Web App can be achieved by performing the following:

+ Adding a CNAME at your domain hosting provider
+ Adding the custom domain within the Azure portal
+ Adding wildcard domain names to multiple Web Apps
Once these steps are complete, accessing your Web App using any subdomain name will be enabled.  For example, app.contoso.com, email.contoso.com, archive.contoso.com, etc…

## Adding a CNAME at your domain hosting provider

Access the portal where your DNS record is stored, for example, Go Daddy and add a CNAME as illustrated in Figure 1.

![adding a wildcard domain to an Azure Web App][FIGURE1]
###### Figure 1, adding a wildcard domain to an Azure Web App

Select the FINISH button and save the changes to DNS zone file.

You would then see something similar to that shown in Figure 2.

![wildcard HOST for CNAME mapping][FIGURE2]
###### Figure 2, wildcard HOST for CNAME mapping

It is possible that the replication of the DNS record can take up to 48 hours, but in most of my experiences, it happens with minutes, if not immediately…experiences may vary.

## Adding the custom domain within the Azure portal

Access the Azure portal, click on the Web App in which you want to bind the wildcard domain to.  Then click on the CONFIGURE link and scroll down to the domain names section, as shown in Figure 3.

![map wildcard domain to Azure Web App][FIGURE3]
###### Figure 3, map wildcard domain to Azure Web App

Click on the manage domain button and add any subdomain to the Azure Web App, as shown in Figure 4, and click the check button and then Save th configuration.

![adding the subdomains to a wildcard domain map][FIGURE4]
###### Figure 4, adding the subdomains to a wildcard domain map

Now, when I access any of those subdomains, they all route to the same Azure App Service Web App.

# Adding wildcard domains to multiple Web Apps

But what happens if I want yes.benjamin.cool to point to a different Web App than cool.benjamin.cool maps to?  As expected, when I try to add the subdomain to another Web App, I get an error, as shown in Figure 5.

![adding a wildcard domain to multiple Web Apps][FIGURE5]
###### Figure 5, adding a wildcard domain to multiple Web Apps

I had the idea then to go back to my DNS provider console and add another wildcard CNAME to this second Web App.  Similiar to that shown in Figure 1, but this time I wanted it to point to iislogs.azurewebsites.net instead of standard.azurewebsites.net.  And that did NOT work.  You can have only 1 HOST that equals to *.  So, to get yes.benjamin.cool to map to iislogs.azurewebsites.net I added a CNAME for that, as shown in Figure 6.

![adding a wildcard subdomain to a second Web App][FIGURE6]
###### Figure 6, adding a wildcard subdomain to a second Web App

Then after it propagated, I was able to add the subdomain to the Web App, as shown in Figure 7.

![adding a wildcard subdomain to a second Web App][FIGURE7]
###### Figure 7, adding a wildcard subdomain to a second Web App

Not being a DNS expert, I expect the code path for finding the IP to route a request to looks first to see if there is a matching HOST name and routes it to the Points To value if found and if no match is found then it maps to the wildcard domain Points To value.

+ Mapping a subdomain to an Azure Web App (Website), here
+ Mapping a naked domain to an Azure Web App (Website), here
+ Mapping a custom domain to a Traffic Manager domain, here
+ Mapping a custom domain that includes a special character, here
+ Mapping a wildcard domain to an Azure Web App, here

[FIGURE1]: ../images/2015/waws-0098.png "Figure 1, adding a wildcard domain to an Azure Web App"
[FIGURE2]: ../images/2015/waws-0099.png "Figure 2, wildcard HOST for CNAME mapping"
[FIGURE3]: ../images/2015/waws-0100.png "Figure 3, map wildcard domain to Azure Web App"
[FIGURE4]: ../images/2015/waws-0101.png "Figure 4, adding the subdomains to a wildcard domain map"
[FIGURE5]: ../images/2015/waws-0102.png "Figure 5, adding a wildcard domain to multiple Web Apps"
[FIGURE6]: ../images/2015/waws-0103.png "Figure 6, adding a wildcard subdomain to a second Web App"
[FIGURE7]: ../images/2015/waws-0104.png "Figure 7, adding a wildcard subdomain to a second Web App"
