# Configure Application Request Routing with Windows Authentication, Kerberos

If you have not already read this [article][LINK1], please do which is a prequel to this one that explains the basic installation and configuration of ARR using anonymous authentication.

The steps for configuring Application Request Routing (ARR) to use Windows Authentication -> Kerberos, are:

+ Set authentication protocol on IIS (Server Farm) servers to Windows Authentication
+ Make sure IWA works from client to IIS servers in the Server Farm
+ Set authentication provider to Anonymous on ARR server
+ Set application pools credentials to a domain account, same custom account on ALL servers
+ Create SPN account for ARR server and content servers
  + SETSPN -S HTTP/arrserver.contoso.com contoso\arraccount
  + SETSPN -S HTTP/iis1server.contoso.com contoso\arraccount
+ Set useAppPoolCredentials on ALL IIS servers, including the ARR server

## Enable Windows Authentication on the IIS servers

The authentication protocol, Windows Authentication -> Kerberos, is set on the IIS server(s) in the Server Farm, not on the ARR server.  ARR acts like a proxy and will simply pass the credential through to the servers configured into the ARR Server Farm.  Enable Windows Authentication on all the IIS servers for the Server Farm that you want ARR to send requests to, similar to that shown in Figure 1.

![configure Windows Authentication on ARR Server Farm IIS servers, not on the ARR server itself][FIGURE1]
###### Figure 1, configure Windows Authentication on ARR Server Farm IIS servers, not on the ARR server itself

## Make sure Windows Authentication work correctly

Make sure when you access the servers in the ARR Server Farm directly, that you get authenticated using Windows Authentication Kerberos.  If you get a popup or authentication fails via direct access, then it will fail too when the request flows through ARRs reverse proxy or load balancing features.  [Here][LINK2] is a short post about how I confirmed a client was using Kerberos.

## Set the authentication protocol on ARR Server

The authentication protocol required to support Windows Authentication on the ARR server is Anonymous.  It is worth mentioning again that ARR acts like a proxy and will simply pass the credential through to the servers configured into the ARR Server Farm.  Similar to that shown in Figure 2.

![onfigure Windows Authentication on ARR Server Farm IIS servers, not on the ARR server itself][FIGURE2]
###### Figure 2, onfigure Windows Authentication on ARR Server Farm IIS servers, not on the ARR server itself

If you need to use client certificates with ARR; read this article [here][LINK3].

Noting that the installation and configuration of the ARR Server Farm is a prerequisite for this exercise, documented here if you were to access the ARR URL from a client machine at this point, you would get a challenge response / credential pop up, you would enter in your credentials 3 times and then get a 401 error page, similar to Figure 3.

![access denied when requesting a page configured for Windows Authentication and routed through ARR][FIGURE3]
###### Figure 3, access denied when requesting a page configured for Windows Authentication and routed through ARR

This is expected at this stage.

## Configure application pool on ALL IIS servers

On the domain controller, I created an account called ARRACCOUNT with access to the groups shown in Figure 4.

![custom application pool account for IIS application pool running ARR][FIGURE4]
###### Figure 4, custom application pool account for IIS application pool running ARR

I am not certain at this point if the account needs to be a member of the Administrators group.  I am documenting these instructions as I perform the configuration.  Once I get this working, I will come back and remove the account from the group.  I hope I remember to update this.

Use this account on all the IIS servers and the ARR server to run the application pool that is associated to the web site responding to client requests through ARR and to the Windows Authenticated IIS Server Farm Servers.  On each of the servers in your ARR environment, open IIS and click on the web site to which you are sending traffic/requests to and click on the Basic Settings…. link as shown in Figure 5.

![finding the application pool for an IIS web site][FIGURE5]
###### Figure 5, finding the application pool for an IIS web site

The name of the application pool is then displayed.

The select the Application Pools folder, as shown in Figure 6, select the Application Pool shown in Figure 5 and select the Advanced Settings… link in the Action pane.  Set the Identity to the account created, shown in Figure 4. 

I also recommend setting the Start Mode to AlwaysRunning and to disable the Idle Timeout, by setting it to 0 on the ARR server, only.  On the IIS servers in the Server Farm, you only need to set the Identity to the custom one as just discussed.

![setting the application pool custom identity for ARR Windows Authentication][FIGURE6]
###### Figure 6, setting the application pool custom identity for ARR Windows Authentication

You need to set the identity on all the servers in this list, for example.

![which servers need a custom identity for Windows Authentication to work with ARR][FIGURE7]
###### Figure 7, which servers need a custom identity for Windows Authentication to work with ARR

## Create SPNs

This is the hardest one for me to get my head around.  To be honest I got this configuration to work numerous times without really understanding 100% how I got it to work, most of the time this was the place where my time was spent.  I think and am very confident that when you make SPNs it takes some time for them to be propagated into the domain.  Have some patience during this step, I promise I got it to work using these steps discussed in this article.

I write the above comments so you know that this is a difficult step and you need some patience:

1. Make the changes slowly and don’t expect a change to the SPN to become effective immediately
2. Don’t give up, I got somewhat frustrated on this step, but in every case, I got it to work, you can too
3. After the next step, reboot everything, take a short break and come back to test it

***NOTE***: I do/did use a tool called KERBLIST that lets me look at the Kerberos tickets on the client to see which ones exists there.  This helped me better understand Kerberos, but can say it did not really progress my implementation along.

I like this article [here][LINK4] where SPNs are discussed.

I created these 4 SPNs, notice that they are the NETBIOS and FQDM for both the ARR server and the IIS server in the Server Farm.  *You would need the same if you have Server01, Server02 and Server03 in your Server Farm, but I just added those for example, in Figure 7.  In Figure 8, I confirm that there are currently no SPNs for the custom application pool account.

![SETSPN to list the SPNs for an customer user account][FIGURE8]
###### Figure 8, SETSPN to list the SPNs for an customer user account

Then I set the SPNs for each of the servers in the Server Farm, including the ARR server.  For example:

+ SETSPN -S HTTP\servername.contoso.com contoso\arraccount

I did it for all the servers and for both the NETBIOS and FQDM, as shown in Figure 9.

![SETSPN for adding HTTP protocol to support ARR and Windows Authentication][FIGURE1]
###### Figure 9, SETSPN for adding HTTP protocol to support ARR and Windows Authentication

The I performed the SETSPN -L command to see the result of my SPN creations, shown in Figure 10.  Compare that to the results of Figure 8.

![confirm my SPN additions were correct for SPNs needed for ARR and Windows Authentication ][FIGURE10]
###### Figure 10, confirm my SPN additions were correct for SPNs needed for ARR and Windows Authentication 

## Enable useAppPoolCredentials on ALL IIS servers

Lastly, you need to enable useAppPoolCredentials by setting the parameter to True on ALL the servers, including the ARR server.  Do this by opening the Configuration Editor at the Sites level and navigating to, also shown in Figure 11.

```System.webServer\security\authentication\windowsAuthentication```

![nable useAppPoolCredentials via the Configuration Manager][FIGURE11]
###### Figure 11, nable useAppPoolCredentials via the Configuration Manager

Set the value to True and Apply the changes, as shown in Figure 12.

![setting and applying useAppPoolCredentials for Windows Authentication and ARR][FIGURE12]
###### Figure 12, setting and applying useAppPoolCredentials for Windows Authentication and ARR

## Test It

From a client machine, access the ARR URL and the request if forwarded, along with the Kerberos credential and you get the expected response.  I was happy to see that and never want to see a Challenge Response pop-up again.

*NOTE: I like to modify the image on the IIS servers in the ARR Server Farm so I am certain the page is not being returned from the ARR server, thus the green circles in Figure 13.  I am certain that is from an IIS server in the Application Request Router (ARR) Server Farm and not the ARR server itself and I know it is using Windows Authentication with Kerberos because it is the only authentication provider enabled on the IIS servers.  You can confirm by looking at the IIS logs on both server or running a Fiddler trace or Network Monitor, but I will leave that for other days and articles, or you can check [this one][link2] out.

![success, how sweet it is, Application Request Routing with Windows Authentication Kerberos][FIGURE13]
###### Figure 13, success, how sweet it is, Application Request Routing with Windows Authentication Kerberos

Here are some other links I found helpful during my personal configuration.

+ [A quick solution when windows authentication is required on backend web server for ARR scenario][LINK5]
+ [Kerberos authentication and Application Request Routing][LINK6]
+ [The biggest mistake: ServicePrincipalName’s][LINK4]

[FIGURE1]: ../images/2015/msdn-0547.png "Figure 1, configure Windows Authentication on ARR Server Farm IIS servers, not on the ARR server itself"
[FIGURE2]: ../images/2015/msdn-0548.png "Figure 2, onfigure Windows Authentication on ARR Server Farm IIS servers, not on the ARR server itself"
[FIGURE3]: ../images/2015/msdn-0549.png "Figure 3, access denied when requesting a page configured for Windows Authentication and routed through ARR"
[FIGURE4]: ../images/2015/msdn-0550.png "Figure 4, custom application pool account for IIS application pool running ARR"
[FIGURE5]: ../images/2015/msdn-0551.png "Figure 5, finding the application pool for an IIS web site"
[FIGURE6]: ../images/2015/msdn-0552.png "Figure 6, setting the application pool custom identity for ARR Windows Authentication"
[FIGURE7]: ../images/2015/msdn-0553.png "Figure 7, which servers need a custom identity for Windows Authentication to work with ARR"
[FIGURE8]: ../images/2015/msdn-0554.png "Figure 8, SETSPN to list the SPNs for an customer user account"
[FIGURE9]: ../images/2015/msdn-0555.png "Figure 9, SETSPN for adding HTTP protocol to support ARR and Windows Authentication"
[FIGURE10]: ../images/2015/msdn-0556.png "Figure 10, confirm my SPN additions were correct for SPNs needed for ARR and Windows Authentication "
[FIGURE11]: ../images/2015/msdn-0557.png "Figure 11, nable useAppPoolCredentials via the Configuration Manager"
[FIGURE12]: ../images/2015/msdn-0558.png "Figure 12, setting and applying useAppPoolCredentials for Windows Authentication and ARR"
[FIGURE13]: ../images/2015/msdn-0559.png "Figure 13, success, how sweet it is, Application Request Routing with Windows Authentication Kerberos"

[LINK1]: 2015-08-configure-application-request-routing.md
[LINK2]: 2015-08-why-am-i-getting-this-challenge-response-popup.md
[LINK3]: ../2014/2014-06-configure-application-request-routing-arr-with-client-certificates.md
[LINK4]: http://blogs.iis.net/brian-murphy-booth/the-biggest-mistake-serviceprincipalname-s
[LINK5]: http://blogs.msdn.com/b/asiatech/archive/2013/06/04/a-quick-solution-when-windows-authentication-is-required-on-backend-web-server-for-arr-scenario.aspx
{LINK6]: http://blogs.technet.com/b/latam/archive/2015/06/24/kerberos-authentication-and-application-request-routing.aspx
