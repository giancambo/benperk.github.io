# Mapping a custom subdomain to an Azure Website

Mapping a custom domain name to your Azure Web App (Website) is based around a very important principle.  The principle being that you need to add a CNAME with a HOST = awverify.  This is how we confirm that you are indeed the owner of this domain.  If you have access to the DNS file configuration, I think it is a safe bet that you own or are responsible for the domain.

In the case of a sub domain, however, there is one small difference I have seen which needs some guidance or explanation.  Instead of the HOST = awverify CNAME that maps to your Azure Website, you need a CNAME for the subdomain and a HOST entry like awverify.subdomain.  Note, if you are mapping both the WWW and a subdomain, you need the HOST = awverify.

For example, I want to map subdomain http://is.benjamin.cool to my Azure Web App called http://reserved.azurewebsites.net. 

First, I added a CNAME for my “IS” sub domain via my DNS hosting providers’ management console, as shown in Figure 1.

![generic CNAME record for a sub domain][FIGURE1]
###### Figure 1, generic CNAME record for a sub domain

After I save the changes and the DNS data propagates, accessing the sub domain results in a 404.  I have written about how to fix this [here][LINK1], but it is caused by the non-existent link between my custom domain and my Azure Web App on the Azure platform.

If I were to access the Azure Management Platform and attempt to add the link now, I would get the error illustrated in Figure 2.

![Cannot add sub-domain, subdomain to my Azure Web App][FIGURE2]
###### Figure 2, Cannot add sub-domain, subdomain to my Azure Web App

There is sometimes a little confusion about the wording of the message.  It is my understanding that when you are adding a custom domain that has WWW, then you need only the CNAME = www.yourcustomdomainname.com that points to your Azure Web App (Website).  However, if you are wanting to map a naked domain or a subdomain to an Azure Web App you will need both, the CNAME that contains the subdomain, plus the AWVERIFY record.

Once I added the AWVERIFY record via my DNS provider console, as illustrated in Figure 3, I was able to map the subdomain to my Azure Web App, see Figure 4, and requests to my subdomain rendered as expected.

![configuring an AWVERIFY subdomain CNAME for binding to an Azure Web App][FIGURE3]
###### Figure 3, configuring an AWVERIFY subdomain CNAME for binding to an Azure Web App

![binding the custom subdomain to an Azure Web App, last step][FIGURE4]
###### Figure 4, binding the custom subdomain to an Azure Web App, last step

See the list of articles below for information about that and how to map other types of custom domains to your Azure Web App (Website).

+ Mapping a subdomain to an Azure Web App (Website), here
+ Mapping a naked domain to an Azure Web App (Website), here
+ Mapping a custom domain to a Traffic Manager domain, here
+ Mapping a custom domain that includes a special character, here
+ Mapping a wildcard domain to an Azure Web App, here

[FIGURE1]: ../images/2014/waws-0012.png "Figure 1, generic CNAME record for a sub domain"
[FIGURE2]: ../images/2014/waws-0013.png "Figure 2, Cannot add sub-domain, subdomain to my Azure Web App"
[FIGURE3]: ../images/2014/waws-0014.png "Figure 3, configuring an AWVERIFY subdomain CNAME for binding to an Azure Web App"
[FIGURE4]: ../images/2014/waws-0015.png "Figure 4, binding the custom subdomain to an Azure Web App, last step"

[LINK1]: https://benperk.github.io/msdn/2013/2013-10-404-file-or-directory-not-found-when-mapping-custom-domain-to-a-windows-azure-web-site-server-error.html
