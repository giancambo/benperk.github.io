# My SSL Certificate doesn’t work on Azure Websites

In this post I will discuss 2 issues you might encounter when configuring an SSL certificate on Azure Web App.

+ The certificate does not show in the drop-down.
+ Browsing to your website over SSL / HTTPS you receive a security warning and the Azure Website wildcard certificate is delivered.

##The certificate does not show in the drop-down

You might encounter an issue after uploading an SSL certificate and when attempting to configure SSL, the certificate doesn’t show up in the CHOOSE A CERTIFICATE drop-down in the SSL BINDINGS section.  Similar to that shown in Figure 1.

![SSL certificate on Azure Web App does not show in drop-down][FIGURE1]
###### Figure 1, SSL certificate on Azure Web App does not show in drop-down

Firstly, you must have a custom domain linked to the Azure Web App in order to configure an SSL BINDING.  Looking at Figure 1, I indeed have that.  Secondly, the domain for which the certificate is created for, must match the custom domain.  I look at the details of my certificate and see this is the case, as shown in Figure 2.

![an SSL certificate for Azure Web App][FIGURE2]
###### Figure 2, an SSL certificate for Azure Web App

So why doesn’t it show in the drop-down?  One common reason is that the domain to which the certificate is for is not configured for the Azure Web App.  Reconfirm that the custom domain name in which you have bound to your Azure Website exists in the SSL certificate you have uploaded.  For example, as shown in Figure 2, open the certificate, click on the Details tab and look at the domain the certificate is for in the Subject Field, does it match the custom domain you have bound to your Azure Website?  Additionally, you may have a certificate which support Subject Alternative Names (SAN).  Scroll down to that field and confirm that your custom domain bound to your Azure Web App exists in the list.

A second common reason is missing or not supported intermediate certificate.  [Here][LINK1] is a very nice overview of this, which I also provide a link to below.  Point is, when you export your certificate for upload to an Azure Web App, make sure you include and intermediate certificate. 

The next issue discussed here has to do with wildcard certificates.  I learned what I needed to know [here][LINK2].  The point here is that if you get a wildcard certificate, for example like, *.contoso.com, it does not mean that it works for infinity as you move further away from the TLD.

Actually, the ultimate answer is that there are a lot of reasons for this, but what I can recommend is that you create a test certificate and upload it and then test again.  By doing this you will be certain that there is no issue with the platform, rather the issue lies with the certificate itself and its ability or supportability on the Azure Web App platform.

I wrote some instructions on how to create a certificate using MAKECERT [here][LINK3].  I have looked [here][LINK4], which discusses using a more current method for creating a certificate, it looks good, but have not personally used it yet.  If after creating the test certificate, you upload it and you do see the certificate in the drop-down, Figure 3, there is something in your official certificate that is not supported or causing some problems.

![SSL certificate on Azure Web App shows up in drop-down][FIGURE3]
###### Figure 3, SSL certificate on Azure Web App shows up in drop-down

You might want to check these locations for some hints as to why your certificate is not working.

+ [Windows Azure Web App (Websites) and Intermediate Certificates][LINK1]
+ [Enable HTTPS for an Azure Web App][LINK5]

You might be able to save some time (some days) if you perform this action as then you’d know better which support team to contact, if required.

## Security warning and wildcard certificate

If you access your Azure Web App URL (https://*??*.azurewebsites.net) using HTTPS you will receive the Azure Web App wildcard certificate.  If you have a custom domain without a successfully configured SSL certificate, accessing the custom domain using HTTPS would result in that shown in Figure 4.

![Certificate Error – Azure Web App - *.azurewebsites.net][FIGURE4]
###### Figure 4, Certificate Error – Azure Web App - *.azurewebsites.net

This is expected behavior if your certificate is not configured correctly or invalid.  For example, you may have configured SNI based SSL which requires 'modern' browsers.  If this is the case, try using IP Based SSL to see if the issue remains.

In regards to Wildcard certificates, something which I mentioned earlier, a common reason is a misunderstanding of the wildcard certificate requirements and limitiations.  I learned what I needed to know [here][LINK2].  The point is that if you get a wildcard certificate, for example like, *.contoso.com, it does not mean that it works for infinity as you move further upwards and away from the TLD.  For example, a wildcard certificate in this format *.contoso.com will work for all subdomains, like admin.contoso.com, my.contoso.com, but will not work with ssl.admin.contoso.com or billing.my.contoso.com, you would need a wildcard certificate for *.admin.contoso.com and *.my.contoso.com, to support both the subdomain names.

[FIGURE1]: ../images/2014/waws-0001.png "Figure 1, SSL certificate on Azure Web App does not show in drop-down"
[FIGURE2]: ../images/2014/waws-0002.png "Figure 2, an SSL certificate for Azure Web App"
[FIGURE3]: ../images/2014/waws-0003.png "Figure 3, SSL certificate on Azure Web App shows up in drop-down"
[FIGURE4]: ../images/2014/waws-0004.png "Figure 4, Certificate Error – Azure Web App - *.azurewebsites.net"

[LINK1]: http://azure.microsoft.com/blog/2014/01/06/windows-azure-web-sites-waws-and-intermediate-certificates/
[LINK2]: http://blogs.msdn.com/b/kaushal/archive/2013/06/13/working-with-wild-card-certificates.aspx
[LINK3]: http://blogs.msdn.com/b/benjaminperkins/archive/2014/05/05/make-your-own-ssl-certificate-for-testing-and-learning.aspx
[LINK4]: https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6#content
[LINK5]: http://azure.microsoft.com/en-us/documentation/articles/web-sites-configure-ssl-certificate/
