# 500.19 when setting up WebDAV on a UNC File share

I have written some instruction on how I setup a WebDAV installation that uses Windows Authentication and UNC [here][LINK1].

An important concept to understand before you start setting up a WebDAV solution using this configuration is the difference in behavior when you are logged on and testing from the IIS server versus testing from a client machine.  When you are on the IIS server, you are not going to be using Kerberos, instead you will use NTLM.  This is just how it is.  However, when accessing the WebDAV solution from a client machine, if IIS and the browser are configured to use Kerberos, then it will be.  You can use Fiddler or Network Monitor to see which authentication provider is used for authentication.  [Here][LINK2] is an article I wrote about how Negotiate in IIS works.

That being said, here are some experiences I had when setting up a WebDAV installation using Windows Authentication and a UNC share.

After installing the WebDAV role, creating the web site that is mapped to a UNC, I accessed the web site from the IIS server and received the error shown in Figure 1.  When I accessed the same from a client machine, I received that shown in Figure 2.

![WebDAV HTTP Error 401.3 - Unauthorized][FIGURE1]
###### Figure 1, WebDAV HTTP Error 401.3 - Unauthorized

![WebDAV HTTP Error 401 – Unauthorized][FIGURE2]
###### Figure 2, WebDAV HTTP Error 401 – Unauthorized

Initially, looking at these errors and having seen them very often in the past, I thought, ok I can solve these easy.  Unfortunately, it did not turn out that way….

What I did next was to enable Windows Authentication, I forgot to do that initially, though it was going to be a quick win, nope…  I did however experience a strange result…  While logged onto the IIS server and having enabled Windows Authentication, I access the WebDAV website and got a 500.19 status message, as shown in Figure 3.

![WebDAV HTTP Error 500.19 – Internal Server Error][FIGURE3]
###### Figure 3, WebDAV HTTP Error 500.19 – Internal Server Error

I used the KERBLIST tool to check what tickets I had and as seen in Figure 4, there were none.

![WebDAV Kerblist, no tickets][FIGURE4]
###### Figure 4, WebDAV Kerblist, no tickets

I then refreshed the page and BABANG!!! There is was, I got this!  Figure 6 shows the INDEX.HTML file I hosted on the UNC share protected by Windows Authentication.

![WebDAV success UNC, Windows Authentication][FIGURE5]
###### Figure 5, WebDAV success UNC, Windows Authentication

Checked the KERBLIST again and after the refresh I have 3 tickets on my machine, as illustrated in Figure 6.

![WebDAV Kerblist, 3 tickets][FIGURE6]
###### Figure 6, WebDAV Kerblist, 3 tickets

The only thing I can think of why this happened is because I left Anonymous Authentication enabled.  I disabled Anonymous Authentication and the issue stopped happening.  I am not 100% certain if this the root cause, but since doing that I have not seen the 500.19 again.

Unfortunately, from my client machine I still received a challenge/response pop-up and after 3 attempts received the 401 message as shown in Figure 7.

![WebDAV HTTP Error 401 – Unauthorized][FIGURE7]
###### Figure 7, WebDAV HTTP Error 401 – Unauthorized

Additionally, prior to disabling Anonymous Authentication, I set the useAppPoolCredentials true, as shown in Figure 8.  After this, I was able to almost reproduce the same behavior I experienced when logged onto the IIS server.

![Set useAppPoolCredentials to true][FIGURE8]
###### Figure 8, Set useAppPoolCredentials to true

My initial request resulted in a 500 response as shown in Figure 9, when I looked at the IIS log for the request, it was indeed a 500.19 HTTP Status response.  When I refreshed my request, I did not receive a request for my credentials, but instead, received a 401 directly, as shown in Figure 10.  I felt I was making some progress.

![WebDAV HTTP Error 500 – Internal Server Error][FIGURE9]
###### Figure 9, WebDAV HTTP Error 500 – Internal Server Error

![ebDAV HTTP Error 401 - Unauthorized][FIGURE10]
###### Figure 10, ebDAV HTTP Error 401 - Unauthorized

In this case, the disabling of Anonymous Authentication had no impact.

I like to use Process Monitor, sometimes it just jogs my brain and leads me to my next steps.  Figure 11 is the Process Monitor log when I made the request from the IIS server and Figure 12 is from the client.  Nothing really unexpected there.

![Successful WebDAV Process Monitor log][FIGURE11]
###### Figure 11, Successful WebDAV Process Monitor log

![ACCESS DENIED WebDAV Process Monitor log][FIGURE12]
###### Figure 12, ACCESS DENIED WebDAV Process Monitor log

What I learned was that in both cases the same User credential were being used to perform the authentication, but I had left the Connect As settings default, which means that pass-through authentication is used.  Figure 13 shows this setting.

![IIS pass-through authentication][FIGURE13]
###### Figure 13, IIS pass-through authentication

If this is set, doesn’t that mean my credential is used for authentication?  Well, this is where it got complicated for me originally, because this issue crosses a technology boundary, the issue was that my account nor the IIS web server was configured for delegation.  Unless you have seen this problem before or you are/were a domain administrator, you would probably not know this.  Well, there you go, now you know, hey that rhymed. 🙂

There are 2 (maybe more?) ways you can configure the solution from this point, you can run your application pool as the default ApplicationPoolIdentity or you can run the application pool using a custom domain account.  The following 2 sections discuss how to do each.

## Using the ApplicationPoolIdentity for WebDAV

When using this method, you need to grant, at least read access on the share to the machine account, the account is usually SERVERNAME$, as shown in Figure 14, for example.

![Grant Machine account access to WebDAV UNC share, SERVERNAME$][FIGURE14]
###### Figure 14, Grant Machine account access to WebDAV UNC share, SERVERNAME$

For the machine account I clicked the “Trust this computer for delegation to any service (Kerberos Only)” as shown in Figure 15.

![onfigure delegation for the IIS Server][FIGURE15]
###### Figure 15, onfigure delegation for the IIS Server

I then accessed the WebDAV website from the client machine and received a challenge/response, man I was disappointed, I thought I had this.  I went through the process of entering my credentials for the 1,000th, wait, 1,000,000th time and WHAT?  It worked after the first try, I saw the same result as previously shown in Figure 6…progress. 

Why did this happen?  I used KERBLIST to look at the tickets I had cached on my client, Figure 16, which shows the one I was using.

![Configure delegation for the IIS Server, cached Kerberos ticket][FIGURE16]
###### Figure 16, Configure delegation for the IIS Server, cached Kerberos ticket

What I noticed was that there was no DELEGATE ticket flag.  ***This is a very important topic to mention.***  The ticket will remain cached on my client machine, so regardless of what I change on the domain, for example, delegation, it will not be realized until I delete the cached ticket or the End Time or Renew Time thresholds are breached.  There may be some way to force out these changes, but I am not aware of any domain commands like that.

I used KERBLIST to purge the locally cached Kerberos tickets so that my next request would result in the retrieval of the fresh ticket with DELEGATE.  My next attempt to access the WebDAV website using Windows Authentication and a UNC resulted in SUCCESS, no challenge/response window and the sweet rendering of the INDEX.HTML file.

I used KERBLIST to look at the ticket and DELEGATE was there, as shown in Figure 17.

![Configure delegation for the IIS Server, renewed Kerberos ticket][FIGURE17]
###### Figure 17, Configure delegation for the IIS Server, renewed Kerberos ticket

## Using a custom identity for WebDAV

Before I started this one, I was sure to remove the delegation of the machine account and purge my locally cached Kerberos tickets.  I tested accessing it, and got the same error as always from the client.  3 attempts = 401…enough said.  I wanted to get this resolved as fast as possible.  I did notice that the rendered error message changed, as shown in Figure 18.

![WebDAV HTTP Error 401, different than Unauthorized][FIGURE18]
###### Figure 18, WebDAV HTTP Error 401, different than Unauthorized

When using a custom identity, you must do 2 things:

+ Create an SPN
+ Configure Delegation

If you attempt to configure delegation before an SPN is created, you will not see the Delegation tab for the account, as shown in Figure 19.  (***NOTE***:  I am using the Administrator account here, ***NEVER*** use the Administrator account to run an application pool, I use it based on my limited knowledge with creating domain accounts and setting their permissions.)

![Account Delegation without an SPN has no Delegation tab][FIGURE19]
###### Figure 19, Account Delegation without an SPN has no Delegation tab

Create an SPN for the domain account, as shown in Figure 20 and then the Delegation tab is visible as shown in Figure 21.

![create an SPN for the WebDAV / application pool account][FIGURE20]
###### Figure 20, create an SPN for the WebDAV / application pool account

![Delegation tab is visible and can be configured][FIGURE21]
###### Figure 21, Delegation tab is visible and can be configured

Select the “Trust this user for delegation to any service (Kerberos only)” and save the configuration.  Execute the following command, as shown in Figure 22, to make sure the SPN exists.

![Check for the existence of the SPN][FIGURE22]
###### Figure 22, Check for the existence of the SPN

For some reason, this did not work for me…took me some time to figure out, but what I know about Kerberos and using SPNs is that if the SPN used to validate the request does not exist or is wrong, then Kerberos will not work.  I used Network Monitor to check and see which SPN is being used, and found that it was using the FQDN and not the NETBIOS name.  I created the required SPN, made sure it was successful, as shown in Figure 23.

![Create another SPN and check for the existence][FIGURE23]
###### Figure 23, Create another SPN and check for the existence

And that is it, it works…see Figure 6, which while working on this article became a very beautiful image.
[Here][LINK3] and [here][LINK4] are 2 additional links for troubleshooting the WebDAV redirector.

[FIGURE1]: ../images/2013/msdn-0282.png "Figure 1, WebDAV HTTP Error 401.3 - Unauthorized"
[FIGURE2]: ../images/2013/msdn-0283.png "Figure 2, WebDAV HTTP Error 401 – Unauthorized"
[FIGURE3]: ../images/2013/msdn-0284.png "Figure 3, WebDAV HTTP Error 500.19 – Internal Server Error"
[FIGURE4]: ../images/2013/msdn-0285.png "Figure 4, WebDAV Kerblist, no tickets"
[FIGURE5]: ../images/2013/msdn-0286.png "Figure 5, WebDAV success UNC, Windows Authentication"
[FIGURE6]: ../images/2013/msdn-0287.png "Figure 6, WebDAV Kerblist, 3 tickets"
[FIGURE7]: ../images/2013/msdn-0288.png "Figure 7, WebDAV HTTP Error 401 – Unauthorized"
[FIGURE8]: ../images/2013/msdn-0289.png "Figure 8, Set useAppPoolCredentials to true"
[FIGURE9]: ../images/2013/msdn-0290.png "Figure 9, WebDAV HTTP Error 500 – Internal Server Error"
[FIGURE10]: ../images/2013/msdn-0291.png "Figure 10, ebDAV HTTP Error 401 - Unauthorized"
[FIGURE11]: ../images/2013/msdn-0292.png "Figure 11, Successful WebDAV Process Monitor log"
[FIGURE12]: ../images/2013/msdn-0293.png "Figure 12, ACCESS DENIED WebDAV Process Monitor log"
[FIGURE13]: ../images/2013/msdn-0294.png "Figure 13, IIS pass-through authentication"
[FIGURE14]: ../images/2013/msdn-0295.png "Figure 14, Grant Machine account access to WebDAV UNC share, SERVERNAME$"
[FIGURE15]: ../images/2013/msdn-0296.png "Figure 15, onfigure delegation for the IIS Server"
[FIGURE16]: ../images/2013/msdn-0297.png "Figure 16, Configure delegation for the IIS Server, cached Kerberos ticket"
[FIGURE17]: ../images/2013/msdn-0298.png "Figure 17, Configure delegation for the IIS Server, renewed Kerberos ticket"
[FIGURE18]: ../images/2013/msdn-0299.png "Figure 18, WebDAV HTTP Error 401, different than Unauthorized"
[FIGURE19]: ../images/2013/msdn-0300.png "Figure 19, Account Delegation without an SPN has no Delegation tab"
[FIGURE20]: ../images/2013/msdn-0301.png "Figure 20, create an SPN for the WebDAV / application pool account"
[FIGURE21]: ../images/2013/msdn-0302.png "Figure 21, Delegation tab is visible and can be configured"
[FIGURE22]: ../images/2013/msdn-0303.png "Figure 22, Check for the existence of the SPN"
[FIGURE23]: ../images/2013/msdn-0304.png "Figure 23, IIS"
[FIGURE24]: ../images/2013/msdn-0305.png "Figure 24, IIS"

[LINK1]: ../2013/2013-07-setting-up-webdav-on-iis-using-windows-authentication-and-a-unc-mapped-drive-or-file-share.md
[LINK2]: ../2011/2011-08-integrated-windows-authentication-with-negotiate.md
[LINK3]: http://www.iis.net/learn/publish/using-webdav/using-the-webdav-redirector
[LINK4]: http://www.iis.net/learn/publish/using-webdav/using-the-webdav-redirector
