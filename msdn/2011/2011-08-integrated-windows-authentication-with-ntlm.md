# Integrated Windows Authentication with NTLM

IIS provides a number of different authentication techniques.  One of which is Integrated Windows Authentication.  Integrated Windows Authentication utilizes Negotiate/Kerberos or NTLM to authenticate users based on an encrypted ticket/message passed between a browser and a server.

![Integrated Windows Authentication with NTLM][FIGURE1]

The preceding image shows a standard communication flow between Internet Explorer and IIS version 6+. The initial request from Internet Explorer typically uses Anonymous as the authentication method.  For this example it is assumed that IIS disables Anonymous Authentication and enables Integrated Windows Authentication to include the Negotiate and NTLM providers.  Considering those assumptions, when IIS receives an Anonymous request from Internet Explorer, a 401.2.x message is returned along with the authentication providers IIS is configured to accept.
Internet Explorer reads the supported authentication providers in the response header from IIS and determines which provider to use.  To configure Internet Explorer to use Integrated Windows Authentication select the Tools menu item of Internet Explorer click on the Advanced tab and you will find a checkbox similar to the below:

![Integrated Windows Authentication with NTLM Enable][FIGURE2]

When the checkbox is unchecked, Internet Explorer will use the NTLM authentication provider.  Internet Explorer will return the authentication provider it wants to use along with the encrypted security message.  You will notice when in comparison to Negotiate, there is an additional GET and response happening between the browser and the web server.  This is simply a requirement when using this provider.  NTLM is designed in a way that requires 2 more actions before a user is authenticated.  Ultimately, once the browser has sent 2 NTLM messages to the server, IIS then authenticates the users’ credentials, and if successful, return a 200.0.0 message.
On a Windows Server R2 2008 using IIS 7.5 the configuration which utilizes only Integrate Windows Authentication resembles the following image.

![Integrated Windows Authentication with NTLM in IIS][FIGURE3]

To view the request and response headers of a web request you can use a tool named Fiddler.  The following image is a textual representation of the first image in this post.  Using Fiddler you can see that a request is made from a browser, a web server 401.2.x response is returned containing the supported/configured authentication providers (Negotiate and NTLM).  A request from the browser is sent containing the preferred authentication provider and a message.  This message, referred to in the first picture in this post as ‘NTLM Message (1)’, contains an encrypted hostname and domain name of the client.  Next, the server returns a 401.1.0 response with another message referred to as ‘NTLM Message (2) which contains the servers challenge.  Finally, the browser returns another message, ‘NTLM Message (3)’ which contains the response which the server uses to validate the credentials.  Once validated, the requested resource is provided along with a 200 OK status.

![Integrated Windows Authentication with NTLM Request and Response headers][FIGURE4]

[FIGURE1]: ../images/2011/msdn-0006.png "Figure 1, Integrated Windows Authentication with NTLM"
[FIGURE2]: ../images/2011/msdn-0007.png "Figure 2, Integrated Windows Authentication with NTLM Enable"
[FIGURE3]: ../images/2011/msdn-0008.png "Figure 3, Integrated Windows Authentication with NTLM in IIS"
[FIGURE4]: ../images/2011/msdn-0009.png "Figure 4, Integrated Windows Authentication with NTLM Request and Response headers"
