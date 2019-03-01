# Why am I getting this challenge response popup?

Real quick, the reason for the credential pop-up is because the URL you are accessing is not in the Local Intranet trusted sites lists, as shown in Figure 6a and 6b!

I setup a domain and did some learning on Kerberos.

The first lessons I learned working towards a better understanding of Kerberos was some NTLM internals.  This is because after configuring IIS as shown in Figure 1, to use Negotiate, when I accessed the IIS server from a client, the client and server negotiated NTLM instead of Kerberos.  So I had to find out why.  What I am confident was happening was caused by the account I was using.  I was using an Administrator account which had a match in the SAM database on the client.  I learned some good NTLM information [here][LINK1].

![configure Kerberos (negotiate) on IIS][FIGURE1]
###### Figure 1, configure Kerberos (negotiate) on IIS

In my lab I had my own CONTOSO domain and created a new account.  When I used this account, I was able to see that Kerberos was being used.  Once I got Kerberos to be used, I got the issue where I had to login once per browser session.  I.e. I would access the server URL, get prompted for my credentials, enter them and then I had access to the web site.  So Kerberos wasn’t failing, I was just getting that credential popup which was not desired.  See the following figures:

+ ***Figure 2*** - initial request using the FQDM where I see the WWW-Authenticate headers for both Negotiate and NTLM, normal to get a 401.2 returned, see [here][LINK2].
+ ***Figure 3*** - the response to the 401.2 from the client was to send the Authorization: Negotiate cookie with the next request.  I noticed the length of the cookie and can be confident that it is indeed Kerberos, NTLM would be shorter.
+ ***Figure 4*** - is a Network Monitor trace to additionally confirm Kerberos is used and looked at the SPN used for the Kerberos ticketing process
+ ***Figure 5*** - the challenge response pop-up I got when accessing the FQDM

![client request using fully qualified domain name (FQDM)][FIGURE2]
###### Figure 2, client request using fully qualified domain name (FQDM)

![client request send back Authorization: Negotiate cookie][FIGURE3]
###### Figure 3, client request send back Authorization: Negotiate cookie

![Network Monitor trace showing the SPN for my Kerberos request][FIGURE4]
###### Figure 4, Network Monitor trace showing the SPN for my Kerberos request

![Kerberos challenge response pop-up, works after entering credentials][FIGURE5]
###### Figure 5, Kerberos challenge response pop-up, works after entering credentials

Once I added the FQDM to the client browser, as shown in Figure 6, I no longer had to enter the credentials for Kerberos authentication to work.

![add the FQDM to the Local Intranet site, IE 10][FIGURE6]
###### Figure 6a, add the FQDM to the Local Intranet site, IE 10

![add the FQDM to the Local Intranet site, IE 11][FIGURE7]
###### Figure 6b, add the FQDM to the Local Intranet site, IE 11

![add the FQDM to the Local Intranet site, IE 11][FIGURE8]
###### Figure 6c, add the FQDM to the Local Intranet site, IE 11

![add the FQDM to the Local Intranet site, IE 11][FIGURE9]
###### Figure 7, add the FQDM to the Local Intranet site, IE 11

[FIGURE1]: ../images/2015/msdn-0532.png "Figure 1, configure Kerberos (negotiate) on IIS"
[FIGURE2]: ../images/2015/msdn-0533.png "Figure 2, client request using fully qualified domain name (FQDM)"
[FIGURE3]: ../images/2015/msdn-0534.png "Figure 3, client request send back Authorization: Negotiate cookie"
[FIGURE4]: ../images/2015/msdn-0535.png "Figure 4, Network Monitor trace showing the SPN for my Kerberos request"
[FIGURE5]: ../images/2015/msdn-0536.png "Figure 5, Kerberos challenge response pop-up, works after entering credentials"
[FIGURE6]: ../images/2015/msdn-0537.png "Figure 6a, add the FQDM to the Local Intranet site, IE 10"
[FIGURE7]: ../images/2015/msdn-0538.png "Figure 6b, add the FQDM to the Local Intranet site, IE 11"
[FIGURE8]: ../images/2015/msdn-0539.png "Figure 6c, add the FQDM to the Local Intranet site, IE 11"
[FIGURE9]: ../images/2015/msdn-0540.png "Figure 7, add the FQDM to the Local Intranet site, IE 11"

[LINK1]: http://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx
[LINK2]: ../2011/2011-08-integrated-windows-authentication-with-negotiate.md
