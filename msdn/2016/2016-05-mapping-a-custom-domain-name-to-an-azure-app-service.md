# Mapping a custom domain name to an Azure App Service

As you may know, Azure Apps Services contains Web App, Mobile Apps, API Apps, Logic Apps, etc…and it is possible to map a custom domain name to each of those.

These articles [here][LINK1] cover all the basics of the domain configuration from a DNS, CNAME and A Record, plus from the wildcard, subdomain, naked, traffic manager and special character domains. This one in particular [here][LINK2] is focused on the basic configuration, but is showing from the old (AUX) portal. They are still valid, but I thought I’d write this one to show the blades in the new portal.

After the DNS configuration is complete as discussed in the articles linked to above, navigate to your App Service and click on Settings -> Custom domains and SSL, as shown in Figure 1.

![adding a custom domain to an Azure App Service][FIGURE1]
###### Figure 1, adding a custom domain to an Azure App Service

After clicking on the Custom domains and SSL link, the blade shown in Figure 2 is rendered.

![configure a custom domain for an Azure App Service][FIGURE2]
###### Figure 2, configure a custom domain for an Azure App Service

Click on the Bring External Domains button and enter your custom domain into the textbox on the Bring External Domains blade. Once is validates correctly, press the Save button.

If the custom domain does not validate, then your DNS configuration is not correct or it has not propagated.

[FIGURE1]: ../images/2016/msdn-0639.png "Figure 1, adding a custom domain to an Azure App Service"
[FIGURE2]: ../images/2016/msdn-0640.png "Figure 2, configure a custom domain for an Azure App Service"

[LINK1]: http://blogs.msdn.com/b/waws/archive/tags/custom+domains/
[LINK2]: http://blogs.msdn.com/b/waws/archive/2014/01/22/three-easy-steps-to-add-a-custom-domain-to-a-waws-site.aspx
