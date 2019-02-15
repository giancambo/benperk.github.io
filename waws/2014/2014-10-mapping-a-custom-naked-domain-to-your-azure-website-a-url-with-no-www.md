# Mapping a custom naked domain to your Azure Website, a URL with no www

A naked URL means that there is no www before the domain name and domain extension.  For example, http://---.me which I use often to troubleshoot and experiment with mapping custom domains to my Azure Web App.  An example of how to do map a custom domain is provided [here][LINK1], and would more than likely result in your naked domain (http://--.me) working.  However, if some configuration gets skipped or mixed up, you might need these instructions too.

The key point is that you must have an A Record that points to the IP address provided via the Microsoft Azure management portal.  For example, that shown in Figure 1.

![mapping a naked custom domain to Azure Web App, I.e. no www][FIGURE1]
###### Figure 1, mapping a naked custom domain to Azure Web App, I.e. no www

Note that the IP address is 137.135…., this is the value you need to set you’re a Record to.

Before I could enter --.me into the Manage custom domain section I needed to add a CNAME as mentioned in Figure 1.  I logged into my DNS providers web site and made the addition as shown in Figure 2.  ***Note**** that for this example, I am binding the custom domain to an Azure Web App named IISLOGS.

![adding a CNMAE record for enablement of the naked Azure Web App, no www][FIGURE2]
###### Figure 2, adding a CNMAE record for enablement of the naked Azure Web App, no www

Once that was added and after it propagated, I was able to add the custom naked domain to my Azure Web App, as shown in Figure 3.

![adding a naked custom domain to Azure Web App, no www][FIGURE3]
###### Figure 3, adding a naked custom domain to Azure Web App, no www

Next step was to then add the A Record using my DNS providers’ web site, as shown in Figure 4.

![adding an A Record for the naked custom URL, no www][FIGURE4]
###### Figure 4, adding an A Record for the naked custom URL, no www

Once the A Record propagates then accessing http://---.me will work.  During this configuration testing, it did take some time for the A Record to propagate.  I used NSLOOKUP to check what IP address was linked to it.  Once I saw that the NSLOOKUP resulted showing the 137.135.* IP address, it worked as expected, as shown in Figure 5. 

![using NSLOOKUP to check for A Record propagation, naked URL, no www][FIGURE5]
###### Figure 5, using NSLOOKUP to check for A Record propagation, naked URL, no www

See the list of articles below for information about that and how to map other types of custom domains to your Azure Web App.

+ Mapping a subdomain to an Azure Web App, here
+ Mapping a naked domain to an Azure Web App, here
+ Mapping a custom domain to a Traffic Manager domain, here
+ Mapping a custom domain that includes a special character, here
+ Mapping a wildcard domain to an Azure Web App, here

[FIGURE1]: ../images/2014/waws-0016.png "Figure 1, mapping a naked custom domain to Azure Web App, I.e. no www"
[FIGURE2]: ../images/2014/waws-0017.png "Figure 2, adding a CNMAE record for enablement of the naked Azure Web App, no www"
[FIGURE3]: ../images/2014/waws-0018.png "Figure 3, adding a naked custom domain to Azure Web App, no www"
[FIGURE4]: ../images/2014/waws-0019.png "Figure 4, adding an A Record for the naked custom URL, no www"
[FIGURE5]: ../images/2014/waws-0020.png "Figure 5, using NSLOOKUP to check for A Record propagation, naked URL, no www"

[LINK]: http://blogs.msdn.com/b/waws/archive/2014/01/22/three-easy-steps-to-add-a-custom-domain-to-a-waws-site.aspx
