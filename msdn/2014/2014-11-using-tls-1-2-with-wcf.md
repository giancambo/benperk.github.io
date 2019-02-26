# Using TLS 1.2 with WCF

***NOTE***: Security is a very serious topic and you should always engage an IT security expert before deploying an application that needs to be secure.  This article is intended to share my experience versus being an article to use as a guideline.

With all the news about the different vulnerabilities like heartbleed and poodle, developers and companies alike are taking an extra hard look at the protocols, hashes, cyphers and encryption technology they have implemented in their environment.  Rightly so.

One thing I found was that when I was testing a WCF call to one of my test Azure Websites, the cypher suite which the client and server were agreeing on was TLS 1.0 and TLS_RSA_WITH_RC4_128_SHA, as shown in the Network Monitor trace, Figure 1.

![WCF using TLS 1.0 instead of TLS 1.2][FIGURE1]
###### Figure 1, WCF using TLS 1.0 instead of TLS 1.2

I confirm the agreed upon Cypher Suite by looking into the Server Hello message within the Network Monitor trace shown in Figure 2.

![WCF TLS 1.0 agreed on TLS_RSA_WITH_RC4_128_SHA instead of 1.2][FIGURE2]
###### Figure 2, WCF TLS 1.0 agreed on TLS_RSA_WITH_RC4_128_SHA instead of 1.2

As per this [article][LINK1], Cypher TLS_RSA_WITH_RC4_128_SHA does support TLS 1.2.  So why did it not use TLS 1.2?

Before I answer that question, when I used a browser like Internet Explorer 11, which was configured to support TLS 1.2 and accessed the same WCF service, I am indeed using TLS 1.2, as per Figure 3.

![TLS 1.2 is supported on Azure Websites, based on my findings][FIGURE3]
###### Figure 3, TLS 1.2 is supported on Azure Websites, based on my findings

The way I ultimately got my WCF client to use TLS 1.2 is by installing this [hotfix][LINK2], and you must use .NET Framework 4.5, please correct me if I am wrong, see below links…

I am not part of the development team nor do I have access to much of the information they use to determine what gets included or not in the framework, but it seems that the inclusion of TLS 1.2 into WCF is an opt-in decision.  You opt-in to using TLS 1.2 by knowingly installing the hotfix.

Some additional articles in context:

+ [How to Determine the Cipher Suite for the Server and Client][LINK3]
+ [SslProtocols Enumeration .NET Framework 4.5 - notice TLS 1.2][LINK4]
+ [SslProtocols Enumeration .NET Framework 4 - notice no TLS 1.2 or 1.1][LINK5]

***UPDATE***:  Check out a registry setting as well.

Under following keys for all the version key listed like V1.0, V2.0.50727,v3,v4.0.30319. Create key SchUseStrongCrypto(type dword, value 1)

HKLM\software\Wow6432Node\Microsoft\.NETFramework\
HKLM\software\microsoft\,NETFramework\

[FIGURE1]: ../images/2014/msdn-0467.png "Figure 1, WCF using TLS 1.0 instead of TLS 1.2"
[FIGURE2]: ../images/2014/msdn-0468.png "Figure 2, WCF TLS 1.0 agreed on TLS_RSA_WITH_RC4_128_SHA instead of 1.2"
[FIGURE3]: ../images/2014/msdn-0469.png "Figure 3, TLS 1.2 is supported on Azure Websites, based on my findings"

[LINK1]: http://msdn.microsoft.com/en-us/library/aa374757(VS.85).aspx
[LINK2]: http://support.microsoft.com/kb/2960358
[LINK3]: http://support2.microsoft.com/kb/299520
[LINK4]: http://msdn.microsoft.com/en-us/library/system.security.authentication.sslprotocols(v=vs.110).aspx
[LINK5]: http://msdn.microsoft.com/en-us/library/system.security.authentication.sslprotocols(v=vs.100).aspx
