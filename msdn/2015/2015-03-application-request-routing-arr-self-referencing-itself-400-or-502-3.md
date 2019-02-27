# Application Request Routing (ARR) – self referencing itself 400 or 502.3

IMO the Application Request Routing server should be on a server which is not hosting the webFarm itself.  The only reason I can think of wanting to do this is to save costs on resources, both hardware and support.  I look at the documentation [here][LINK1] and do not see any mention of this kind of configuration.  However, the beauty of platforms, especially IIS, is that users can be creative and come up with solutions that work like this, so here is an example of how I did it.  The goal is to make what is shown in Figure 1 work, where the requests are handled by the Default Web Site on port 80 which are then redirected to sites APP3 and APP4.

![Routing requests to ARR back to itself][FIGURE1]
###### Figure 1, Routing requests to ARR back to itself

When I set this up initially I get a 400.0 Bad request – The request cannot be routed because it has reached the Max-Forwards limit, as shown in Figure 2 when I access either the APP3 or APP4 sites directly from on the server.

![ARR The request cannot be routed because it has reached the Max-Forwards limit][FIGURE2]
###### Figure 2, ARR The request cannot be routed because it has reached the Max-Forwards limit

And when I tried accessing the Default Web Site from on the server or from another client, I received a 502.3 – The server returned an invalid or unrecognized response, which I have discussed [here][LINK2] in more detail.

![ARR The server returned an invalid or unrecognized response][FIGURE3]
###### Figure 3, ARR The server returned an invalid or unrecognized response

Here are the steps I took to make this work in the way I wanted.

1. Modify the HOST file and bind the webFarm site names to the IP of the ARR server
2. Bind those webFarm site names to the websites
3. Disable the Server Level URL Rewrite Rule, create a new URL Rewrite rule on the Default Web Site
4. Set preserveHostHeader=”false”

## Modify the HOST file

The HOST file is a way you can run a pseudo DNS server on your machine.  Basically it’s where you can link an IP address to a domain name.  The HOST file is located in the c:\windows\system32\drivers\etc directory.  I have seen issues caused by this that took days to resolve, so make changes here with care and document it so others know you did this.  You need to add something similar to the following.

```
127.0.0.1 APP3.MSFT.TEST
127.0.0.1 APP4.MSFT.TEST
```

In my example I used the actual IP of the machine, however using this IP should work too.

## Bind the webFarm sites to the web sites

Ceteris Paribus, requests will come into the server on port 80 which the Default Web Site is configured to do.  To support the ‘self-reference’ solution you need to bind the domain names you added in the HOST file to the websites you want in the webFarm.  As shown in Figure 4, you see that APP3.MSFT.TEST is bound to the same on port 80.  You would need to add the binding for each of the domains in your webFarm.

![Adding a hostname binding to a self-reference ARR solution][FIGURE4]
###### Figure 4, Adding a hostname binding to a self-reference ARR solution

## Disable the default URL Rewrite rule and create a new one

If you try to navigate to any of these sites now, you will get either the 400 or 502.3 shown in Figure 2 and 3.  This is because by default, as shown in Figure 5, when you create an ARR server farm, it creates a server level URL Rewrite rule as well, that redirects requests to the server farm.  Disable that, once disabled you should be able to access all 3 sites from the server.  Note that the domain names you added to the HOST file can only be accessed from the server you placed them, you won’t be able to access them from another client.

![the default URL Rewrite Rule for ARR][FIGURE5]
###### Figure 5, the default URL Rewrite Rule for ARR

At the Default Web Site level, you need to create a new URL Rewrite Rule similar to this one, shown in Listing 1.

###### Listing 1, ARR self-redirection URL Rewrite Rule

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="ARR-SELF-REDIRECTION" patternSyntax="Wildcard" stopProcessing="true">
          <match url="*" />
          <action type="Rewrite" url="http: // MSFT.TEST/{R:0}" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Notice that the action type is Rewrite and the URL is the name of the Server Farm to which I want the requests redirected to.

If you try accessing the Default Web Site now you will still get the errors shown previously in Figure 2 and Figure 3.  The last thing you need to do is disable the preservation of the Host Header.

## Set preserverHostHeader=”false”

Using the Configuration Editor at the server level shown in Figure 6, set the preserveHostHeader parameter to false.

![disable preserverHostHeader to get ARR self-redirection to work][FIGURE6]
###### Figure 6, disable preserverHostHeader to get ARR self-redirection to work

Save the configuration change and access the ARR server and your request should now be routed to the IIS sites on the same server.

[FIGURE1]: ../images/2015/msdn-0486.png "Figure 1, Routing requests to ARR back to itself"
[FIGURE2]: ../images/2015/msdn-0487.png "Figure 2, ARR The request cannot be routed because it has reached the Max-Forwards limit"
[FIGURE3]: ../images/2015/msdn-0488.png "Figure 3, ARR The server returned an invalid or unrecognized response"
[FIGURE4]: ../images/2015/msdn-0489.png "Figure 4, Adding a hostname binding to a self-reference ARR solution"
[FIGURE5]: ../images/2015/msdn-0490.png "Figure 5, the default URL Rewrite Rule for ARR"
[FIGURE6]: ../images/2015/msdn-0491.png "Figure 6, Feature"

[LINK1]: http://www.iis.net/learn/extensions/planning-for-arr/using-the-application-request-routing-module
[LINK2]: ../2012/2012-09-application-request-router-arr-http-error-502-3-bad-gateway.md
