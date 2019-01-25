# Kerberos authPersistNonNTLM authentication, request based vs. session based authentication

Kerberos is a request based authentication protocol.  That means with each request, there is a resulting authentication step.  See the following figure 1 where you notice a Ticket request for each GET Http Command.

![Network Monitor log for request based authentication][FIGURE1]
###### Figure 1, Network Monitor log for request based authentication

This article covers the implementation of the authPersistNonNTLM attribute, that when set to true, changes Kerberos authentication from request based to session based.  When the authPersistNonNTLM is set to true, the TCP session is used  to identify the authenticated client.

**NOTE:** A TCP session is an IP address combined with the utilized port number (ex: 192.168.1.1:80).

I had a review of this article [here][LINK1] where a discussion of poor performance in regards to using Integrated Windows Authentication in combination with Kerberos.  A very good description of why the performance degradation may occur can be found [here][LINK2].  The performance degradation is caused because a Kerberos ticket includes all group memberships of the user requiring authentication.  The more groups a user is associated with the larger the ticket and therefore the request header grows.  The bigger/more data that is sent the longer it takes, and recall from Figure 1 that each GET Http Command results in a Ticket request.

In addition to the information found in these other articles, I want to expand on them in 3 ways:
1. How to modify the authPersistNonNTLM attribute using the Internet Information Services manager
2. Show how to confirm whether request based or session based authentication is implemented using Fiddler and Network Monitor
3. In what scenario does authPersistNonNTLM not work

You may find some value reviewing my other 2 blogs about [Kerberos][LINK3] and [NTLM][LINK4].  They will explain what is happening between the client and server, plus how to enable them within IIS.

## How to modify the authPersistNonNTLM attribute using the Internet Information Services manager

To modify the authPersistNonNTLM attribute using IIS manager, open the Internet Information Services (IIS) Manager and select the server name within the connection pane.  As shown below in Figure 2.  You can also implement the setting at the web site level. 

![selection of the server within IIS manager][FIGURE2]
###### Figure 2, selection of the server within IIS manager

Then, in the Management section of the Features View pane select the Configuration Manager, as shown in Figure 3.

![Management section][FIGURE3]
###### Figure 3, Management section

Then, navigate to the windowsAuthentication element as shown in the following figure, as visualized in Figure 4.

![windowsAuthentication configuration][FIGURE4]
###### Figure 4, windowsAuthentication configuration

You will then see the authPersistNonNTLM element with a drop-down list box containing true or false, as shown in Figure 5.

![authPersistNonNTLM element][FIGURE5]
###### Figure 5, authPersistNonNTLM element

## Show how to confirm whether request based or session based authentication is implemented using Fiddler and Network Monitor

The default value is false. Therefore without changing authPersistNonNTLM the Fiddler response contains the following, as shown in Figure 6, note the value of Persistent-Auth.  Recall also from Figure 1, that each GET Http Command results in a Kerberos Ticket request, as shown in the Kerberos Summary column of Figure 1.

![authPersistNonNTLM element][FIGURE6]
###### Figure 6, Fiddler log shows Persistent-Auth: false

After setting authPersistNonNTLM to true, the following is present in the Fiddler response as shown in Figure 7, note the value of Persistent-Auth.

![authPersistNonNTLM element][FIGURE7]
###### Figure 7, Fiddler log shows Persistent-Auth: true

You noticed that the Persistent-Auth changes between the 2 responses.  You can find more information about this attribute [here][LINK5].

In contrast to that of Figure 1, when authPersistNonNTLM is set to true only the first GET Http Command results in a Kerberos Ticket request for the specific TCP Session, as shown in Figure 8. Notice that there is only a single entry in the Kerberos Summary column.

![authPersistNonNTLM element][FIGURE8]
###### Figure 8, Network Monitor log for session based authentication

## In what scenario does authPersistNonNTLM not work
As already mentioned in this article, Kerberos authentication is dependent on a TCP Session. 

**QUESTION:** Why is there a 401 – Access Denied HTTP status code at step 437 when there has already been a 401 on step 425, assuming this is the same client?

![authPersistNonNTLM element][FIGURE9]
###### Figure 9, Fiddler log example

**ANSWER:** The port number changed from 1006 to 1003 and this means that Kerberos recognized the request as a different, unauthorized TCP session and therefore needed to reauthorize the client.

This means that if your application has any mechanism which can change the port being used between the client and server, then you may encounter multiple authentications throughout a client’s session.  If the TCP Session in not maintained (ex: IP:PORT), then setting the authPersistNonNTLM attribute may appear to not act as expected.

[FIGURE1]: ../images/2011/msdn-0022.png "Figure 1, Network Monitor log for request based authentication"
[FIGURE2]: ../images/2011/msdn-0023.png "Figure 2, selection of the server within IIS manager"
[FIGURE3]: ../images/2011/msdn-0024.png "Figure 3, Management section"
[FIGURE4]: ../images/2011/msdn-0025.png "Figure 4, Management section"
[FIGURE5]: ../images/2011/msdn-0026.png "Figure 5, authPersistNonNTLM element"
[FIGURE6]: ../images/2011/msdn-0027.png "Figure 6, Fiddler log shows Persistent-Auth: false"
[FIGURE7]: ../images/2011/msdn-0028.png "Figure 7, Fiddler log shows Persistent-Auth: true"
[FIGURE8]: ../images/2011/msdn-0029.png "Figure 8, Network Monitor log for session based authentication"
[FIGURE9]: ../images/2011/msdn-0030.png "Figure 9, Fiddler log example"

[LINK1]: http://support.microsoft.com/kb/954873
[LINK2]: http://blogs.iis.net/thomad/archive/2009/10/23/kerberos-authentication-issues.aspx
[LINK3]: 2011-08-integrated-windows-authentication-with-negotiate.md
[LINK4]: 2011-08-integrated-windows-authentication-with-ntlm.md
[LINK5]: http://msdn.microsoft.com/en-us/library/dd341152(PROT.10).aspx
