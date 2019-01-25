# Secure channel compatibility support with SSL and TLS

I wrote 2 previous blogs about [NTLM][LINK1] and [Negotiate/Kerberos][LINK2] which discussed briefly about how those authentication packages work within the context of Integrated Windows Authentication.

Another authentication package supported in Windows is called Secure Channel, also known as Schannel.  This blog will not discuss the details of Schannel because the detail can be found [here][LINK3].  Instead, this blog will discuss a bit about the client/server support and brief configuration requirements for TLS protocols on Windows.

There are a number of SSL (SSL 1.0, SSL 2.0 and SSL 3.0) and TLS (TLS 1.0, TLS 1.1 and TLS 1.2) versions.  You can check and see which SSL/TLS version your Internet Explorer supports by selecting **Tools -> Internet Options -> Advanced Tab**.  Scroll down to the Security group and you may see something as shown in Figure 1.

![possible SSL and TLS support within an Internet Explorer][FIGURE1]
###### Figure 1, possible SSL and TLS support within an Internet Explorer

It is important to realize that the supportable SSL and TLS versions depend on the underlying Windows operating system version and its associated Schannel (schannel.dll) component.  The following table shown in figure 2 represents the supported SSL and TLS version per Windows Server and Client version.

![Support for SSL and TLS protocol on Windows][FIGURE2]
###### Figure 2, Support for SSL and TLS protocol on Windows 

You notice that only Windows Server 2008 R2 (which is 64bit) and Windows 7 support the TLS 1.1 and TLS 1.2 protocol version.  To implement TLS 1.1 or TLS 1.2 both the client and the server need to support the protocol version.

## Secure Channel / TLS Configuration 

Unfortunately, I have not seen an implementation example that is applicable to all scenarios.  Therefore, I can only provide some tips and experiences towards what may or may not be required to get TLS 1.1 or TLS 1.2 up and running on your environment.  Security is a very important aspect for companies and individuals; therefore if you are considering implementing a secure channel, I recommend strongly working with an IT security professional.

Some information can be found here and here.

## Configuring the Browser

If the client operating system and browser that you are using supports TLS 1.1 or TLS 1.2, for example Internet Explorer version 9 on Windows 7, you should make sure the check boxes are enabled for those protocol versions, as shown previously in Figure 1.

## Configuring IIS 7.5 (Internet Information Services)

Configuring IIS 7.5 to support TLS 1.1 and 1.2 is more resource intensive.  It is not enabled by default and there are more activities required when compared to configuring client/browser support.

+ For testing I created a web site on IIS 7.5 which accepted secure access on port 443, I made sure that the browser had the default settings (SSL 3.0 and TLS 1.0 enabled) and I accessed the web site.  OK
+ Next, I modified the browser setting to only support TLS 1.1 and 1.2, restarted the browser and accessed the web site.  FAIL
+ Next, using the Group Policy snap-in I enabled the following, located at Computer Configuration\Windows Settings\Security Settings\Local Policies\Security Options.
  ![system cryptography][FIGURE3]
+ Lastly, I rebooted the server, made some registry modifications and accessed the web site.  OK

The effects of enabling system cryptography can be viewed [here][LINK4].

Using Fiddler I captured the following request header.  I am interested why it mentions a SSLv3-Compatible handshake when I had only TLS 1.2 enabled.  Perhaps it is referring to SSL 3.3 which is synonymous with TLS 1.2.  I am not sure; let me know if you find out.

![SSL and TLS protocol on Windows wireshark response][FIGURE4]

[FIGURE1]: ../images/2011/msdn-0018.png "Figure 1, possible SSL and TLS support within an Internet Explorer"
[FIGURE2]: ../images/2011/msdn-0019.png "Figure 2, Support for SSL and TLS protocol on Windows"
[FIGURE3]: ../images/2011/msdn-0020.png "Figure 3, system cryptography"
[FIGURE4]: ../images/2011/msdn-0021.png "Figure 4, system cryptography"

[LINK1]: 2011-08-integrated-windows-authentication-with-ntlm.md
[LINK2]: 2011-08-integrated-windows-authentication-with-negotiate.md
[LINK3]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa380123(v=VS.85).aspx
[LINK4]: http://support.microsoft.com/kb/811833
