# Adding a Hostname to your SSL certificate binding on port 443

I see a lot of articles and posts about being able to bind different websites on the same server to port 443.  This article is my take on the subject.

It is possible to bind multiple websites hosted on the same server to port 443, but you can’t easily do it from the GUI because the Host name is greyed out as shown in Figure 1.  However, if the SSL Certificate begins with a *, then it is enabled.  This is likely to support wild card certificates as shown in Figure 2.

The confusion comes when you attempt to bind different SSL certificates to the different website both of which want to use port 443.  That won’t work.

![Host name is greyed out][FIGURE1]
###### Figure 1, Host name is greyed out

I tried a hack just to test by binding SSL certificate 1 to port 443 and SSL certificate 2 to 444 from within the GUI.  Then using appcmd I modified the bindings for the 2 websites so that they both used port 443 and had a different host name.

The result of this was that the certificate originally bound to port 444, SSL certificate 2, was changed to use SSL certificate 1.  Meaning, the different SSL certificates being bound to port 443 where being forced to use a single SSL certificate.  The SSL certificate on port 444 was being replaced with the one bound to 443.

This means that you can only have 1 SSL certificate per IP:PORT combination.

![Host name is not greyed out when * exists][FIGURE2]
###### Figure 2, Host name is not greyed out when * exists

Another way to view this requirement is using the following [netsh command][LINK1]:

***netsh http show sslcert***

The result will show you the IP:PORT combination settings for the installed SSL certificate.  The reason this combination is required is due to the sequence of events that happen during the handshake.  A pseudo example:

1. CLIENT – resolve URL to IP and PORT
2. CLIENT – connects to IP and PORT
3. SERVER – Answers from that IP and PORT
4. CLIENT – Asks for SSL certificate
5. SERVER – Sends SSL certificate
6. CLIENT – Compares SSL certificate name from server to name from step 1
7. CLIENT – Assuming they match, begin encrypted exchange
8. CLIENT – Send encrypted information to server
9. SERVER – Sends encrypted information back to client

The reason that the IP and PORT combination is so important is because the server must know which certificate to return to the client in step 5.  The server does not know the hostname until step 8.  This is why IIS is setting the SSL certificate back to the original one bound to port 443 when it is attempted to bind it to an IP:PORT:Hostname combination.

Note that you can make some changes in the applicationHost.config file directly, however you will not be able to bind different SSL certificates to an IP:PORT:Hostname, where IP:PORT are the same.  You can bind the same SSL certificate to *:443:Hostname.

## Solution

IIS 8 introduces a new concept called [SNI (Server Name Indication)][LINK2].  Check it out.

[LINK1]: http://msdn.microsoft.com/en-us/library/cc307243(v=vs.85).aspx
[LINK2]: http://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-server-name-indication-sni-ssl-scalability

[FIGURE1]: ../images/2013/msdn-0208.png "Figure 1, Host name is greyed out"
[FIGURE2]: ../images/2013/msdn-0209.png "Figure 2, minBytesPerSecond set to 240"
