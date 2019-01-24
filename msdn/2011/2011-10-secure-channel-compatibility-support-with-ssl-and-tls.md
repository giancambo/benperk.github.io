# Secure channel compatibility support with SSL and TLS

I wrote 2 previous blogs about [NTLM][LINK1] and [Negotiate/Kerberos][LINK2] which discussed briefly about how those authentication packages work within the context of Integrated Windows Authentication.

Another authentication package supported in Windows is called Secure Channel, also known as Schannel.  This blog will not discuss the details of Schannel because the detail can be found [here][LINK3].  Instead, this blog will discuss a bit about the client/server support and brief configuration requirements for TLS protocols on Windows.

There are a number of SSL (SSL 1.0, SSL 2.0 and SSL 3.0) and TLS (TLS 1.0, TLS 1.1 and TLS 1.2) versions.  You can check and see which SSL/TLS version your Internet Explorer supports by selecting **Tools -> Internet Options -> Advanced Tab**.  Scroll down to the Security group and you may see something as shown in Figure 1.



[LINK1]: 2011-08-integrated-windows-authentication-with-ntlm.md
[LINK2]: 2011-08-integrated-windows-authentication-with-negotiate.md
[LINK3]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa380123(v=VS.85).aspx
