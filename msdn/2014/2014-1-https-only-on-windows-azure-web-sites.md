# HTTPS only on Azure App Service Web Apps

If you are looking for a resource that describes installing an SSL certificate on a Azure Web App, check [here]LINK1].

[Here][LINK2] is more information about this configuration.

There are multiple modes currently supported on the Azure Web App platform:

+ ***SNI base SSL*** – This is a new feature in IIS 8+ (SNI) that extends the ability for multiple security certificates to be bound to multiple HOSTNAMEs on a server with a single IP and PORT.  (modern browsers support this SSL mode)
+ ***IP based SSL*** – The traditional binding of a certificate to a unique IP and PORT on a server

For some further information on how to implement both, please look [here][LINK3].

In some cases you might want to prevent users from accessing your website using anything other than HTTPS.  To achieve this, add the following code, illustrated in Listing 1, to your web.config file.

```
<system.webServer>
 <rewrite>
  <rules>
   <rule name="Redirect to https">
    <match url="(.*)"/>
    <conditions>
     <add input="{HTTPS}" pattern="Off"/>
    </conditions>
    <action type="Redirect" url="https://{HTTP_HOST}/{R:1}"/>
   </rule>
  </rules>
 </rewrite>
</system.webServer>
```
###### Listing 1, Prevent HTTP connectivity to you Azure Web App, allow HTTPS only

Once added, deploy the web.config file to your Azure Web App and requests to HTTP are redirected to HTTPS using this URL Rewrite rule.  That is how you would prevent HTTP traffic onto you Azure Web App.

Note: the configuration shown in Listing 1 contains a condition base on the request method.  For example, GET and HEAD.  In this case only requests using those verbs will be redirected to HTTPS, if, for example, POST is used, the rule would not be executed as the conditions would not be meet.

UPDATE 17-JAN-2014

[Here][LINK4] is an article concerning the support and installation of intermediate certificates on windows Azure Web Apps.

[LINK1]: http://www.windowsazure.com/en-us/develop/net/common-tasks/enable-ssl-web-site
[LINK2]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples#redirect-http-traffic-to-https
[LINK3]: http://ruslany.net/2013/07/how-to-setup-ip-ssl-on-windows-azure-web-sites/
[LINK4]: http://blogs.msdn.com/b/windowsazure/archive/2014/01/06/windows-azure-web-sites-waws-and-intermediate-certificates.aspx
