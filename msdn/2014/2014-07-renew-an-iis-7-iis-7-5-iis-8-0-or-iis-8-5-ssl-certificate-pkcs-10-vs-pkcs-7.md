# Renew an IIS 7, IIS 7.5, IIS 8.0 or IIS 8.5 … SSL Certificate (PKCS #10 vs. PKCS #7)

I will confess to not being a security expert.  So this article is not about how the process works, instead about a change that I had a hard time finding when working with a customer some months ago.  Security is serious business, so I like to leave those kinds of things to the experts and specialists.  I read a footer in an email message from a security expert here at Microsoft that helped me better recognize that fact, it said:

“It’s not what you don’t know that can hurt you, it’s what you know that isn’t so.”

When you have an SSL certificate that you want to renew and you click on it in the IIS Management console you will see the Renew… link in the Action pane as shown in Figure 1.

![Renew and SSL certificate in IIS 7, IIS 7.5, IIS 8, etc…][FIGURE1]
###### Figure 1, Renew and SSL certificate in IIS 7, IIS 7.5, IIS 8, etc…

In IIS 7, IIS 7.5, IIS 8.0 and IIS 8.5, and I would assume all future releases of IIS, the request is delivered in the PKCS #7 format.  This is different than the format provided in IIS 6, where it was in the PKCS #10 format.  [Here][LINK1] is a link that discusses why and gives a reason.

This causes some issues with the renewal process as some Certificate Authorities do not accept the KCS #7 format.  Microsoft has released [KB971832][LINK2] about this here which is simply stating that you should create a New request instead of a Renew.

For some customers this might not be the best or wanted process.  I did some research and had some discussions with team members and this is simply how it is if you want to use the IIS Management Console for the SSL Certificate Renewal process.  The renewal process from the IIS Management console creates a PKCS #7 request.

If you absolutely must have a PKCS #10, then I found a tool called [CERTREQ][LINK3] when I looked for some deeper information about creating a PKCS #10 requests located [here][LINK4].  NOTE:  I did not confirm this, but it is likely that the IIS Management console Renew wizard is using CERTREQ, or similar, to create the requests.  You could use, confirm and learn a lot from performing the Renew or Create an SSL Certificate by running Process Monitor while doing it.

At the bottom of the CERTREQ page, there was an article written called “[How to create a web server SSL certificate manually][LINK5]” which might be helpful in your attempt to create the PKCS #10 request.

Pulling all this information together, I would guess that using CERTREQ where the RequestType=pkcs10, that you could get the Renew to work.

I did not test this process, I just wanted to put together my noted and share my experience.  I recommend creating a new request as per KB971832, but if you can’t here is at least something you try.  If you do get this to work, let me know so we have some confirmation.

[FIGURE1]: ../images/2014/msdn-0405.png "Figure 1, Renew and SSL certificate in IIS 7, IIS 7.5, IIS 8, etc…"

[LINK1]: http://blogs.iis.net/rlucero/archive/2009/05/28/certificate-renewals-in-iis-7.aspx
[LINK2]: http://support.microsoft.com/kb/971832/EN-US
[LINK3]: http://technet.microsoft.com/library/cc725793.aspx
[LINK4]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa379078(v=vs.85).aspx
[LINK5]: http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx
