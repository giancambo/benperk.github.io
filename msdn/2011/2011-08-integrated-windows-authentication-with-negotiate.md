# Integrated Windows Authentication with Negotiate

IIS provides a number of different authentication techniques.  One of which is Integrated Windows Authentication.  Integrated Windows Authentication utilizes Negotiate/Kerberos or NTLM to authenticate users based on an encrypted ticket/message passed between a browser and a server.

![Integrated Windows Authentication with Negotiate][FIGURE1]

The preceding image shows a standard communication flow between Internet Explorer and IIS version 6+.  The initial request from Internet Explorer typically uses Anonymous as the authentication method.  For this example it is assumed that IIS disables Anonymous Authentication and enables Integrated Windows Authentication to include the Negotiate and NTLM providers.  Considering those assumptions, when IIS receives an Anonymous request from Internet Explorer, a 401.2.x message is returned along with the authentication providers IIS is configured to accept.
Internet Explorer reads the supported authentication providers in the response header from IIS and determines which provider to use.  To configure Internet Explorer to use Integrated Windows Authentication select the Tools menu item of Internet Explorer, click on the Advanced tab and you will find a checkbox similar to the below:

![Enable Integrated Windows Authentication with Negotiate][FIGURE2]

When enabled, Internet Explorer will use the Negotiate authentication provider, which means Kerberos.  Internet Explorer will return the authentication provider it wants to use along with the encrypted security ticket.  IIS will then authenticate the users’ credentials, and if successful, return a 200.0.0 message.
On a Windows Server R2 2008 using IIS 7.5 the configuration which utilizes only Integrated Windows Authentication resembles the following image.

![Integrated Windows Authentication with Negotiate in IIS][FIGURE3]

To view the request and response headers of a web request you can use a tool named Fiddler.  The following image is a textual representation of the first image in this post.  Using Fiddler you see that a browser request is made, a 401.2.x server response is returned containing the supported/configured authentication providers (Negotiate and NTLM).  A request from the browser containing the Kerberos token is sent to the server which returns the requested content along with a 200 OK status.

![Integrated Windows Authentication with Negotiate Response Request Headers][FIGURE4]

Here is a very good [article][LINK1] covering how to troubleshoot Kerberos issues.

[FIGURE1]: ../images/2011/msdn-0002.png "Figure 1, Integrated Windows Authentication with Negotiate"
[FIGURE2]: ../images/2011/msdn-0003.png "Figure 2, Enable Integrated Windows Authentication with Negotiate"
[FIGURE3]: ../images/2011/msdn-0004.png "Figure 3, Integrated Windows Authentication with Negotiate in IIS"
[FIGURE4]: ../images/2011/msdn-0005.png "Figure 4, Integrated Windows Authentication with Negotiate Response Request Headers"

[LINK1]: http://blogs.msdn.com/b/friis/archive/2009/12/31/things-to-check-when-kerberos-authentication-fails-using-iis-ie.aspx
