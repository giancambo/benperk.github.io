# How to redirect a WWW custom domain to a naked domain

I worte this article some time ago which discusses how to map a naked domain to an Azure App Service.  An updated version can be found here.

Also, check out these articles as well.

+ How to redirect a WWW custom domain to a naked domain
+ HTTPS only on Azure App Service Web Apps
+ Mapping a custom domain name to an Azure App Service
+ How to reduce downtime when moving a custom domain between Azure Web Apps
+ How to get a static (incoming) IP address for your Azure App Service Web App

In some cases you might want to prevent users from accessing your website using the www domain prefix.  To achieve this, add the following code, illustrated in Listing 1, to your web.config file.

Listing 1, Prevent HTTP connectivity to you Azure Web App, allow HTTPS only

```
<system.webServer>
 <rewrite>
  <rules>
   <rule name="Remove www prefix">
     <match url="(.*)" ignoreCase="true" />
     <conditions>
      <add input="{HTTP_HOST}" pattern="^www\.contoso\.com$" />
     </conditions>
     <action type="Redirect" url="http://contoso.com/{R:1}" redirectType="Permanent" />
   </rule>
  </rules>
 </rewrite>
</system.webServer>
```

Once added, deploy the web.config file to your Azure App Service and requests with a domain prefix of www are redirected to the naked domain using this URL Rewrite rule.  That is how you would prevent www traffic onto your Azure App Service.
