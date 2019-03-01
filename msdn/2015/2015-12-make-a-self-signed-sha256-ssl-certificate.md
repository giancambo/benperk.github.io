# Make a self-signed SHA256 SSL certificate

I wrote an article about making an SSL certificate using MAKECERT here, but that example used the default SHA1 signature hash algorithm which is deprecating.  Therefore, instead of the command shown in Figure 6 on the referenced article, I recommend using this command, that includes the SHA256 attribute, similar to that shown in Figure 1:

makecert –a SHA256 -pe -iv benperkmeCA.pvk -n "CN=benjamin-perkins.me" -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -ic BenperkmeCA.cer IIS-ServerCert-Benperk.cer

![make a self signed certificate with stronger SHA hash algorithm][FIGURE1]
###### Figure 1, make a self signed certificate with stronger SHA hash algorithm

The MAKECERT tool is discussed [here][LINK2], where you can see that it supports numerous signature algorithms.  The executable itself is included in the Windows XP Server Tools package.  You might want to do some searching around for it and get it from there.

[FIGURE1]: ../images/2015/msdn-0582.png "Figure 1, make a self signed certificate with stronger SHA hash algorithm"

[LINK1]: ../2014/2014-05-make-your-own-ssl-certificate-for-testing-and-learning.md
[LINK2]: https://msdn.microsoft.com/library/windows/desktop/aa386968.aspx
