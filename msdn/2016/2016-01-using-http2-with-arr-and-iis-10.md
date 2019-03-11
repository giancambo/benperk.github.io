# Using HTTP/2 with ARR and IIS 10

I was asked a question about if the Application Request Router supports the HTTP 2 protocol on IIS 10 (not sure what it will be called).  I was pretty confident that it did, but nothing beats a test to confirm it.  So that is what I did.  I used this article [here][LINK1] that contains some good information about HTTP/2 on IIS 10.  As I read through the instructions, the only requirement for using the HTTP/2 protocol is that the request is made over HTTPS.  And of course, you need IIS 10…

I found no difference in the setup of ARR on IIS 10, so this article [here][LINK2] still seems to apply.  I prefer using the Web Platform Installer for adding IIS modules, but there are numerous techniques.

As shown in Figure 1, I created an ARR Server Farm called W2K16-IIS10

![Using ARR on IIS 10 with http protocol version 2][FIGURE1]
###### Figure 1, Using ARR on IIS 10 with http protocol version 2

On the Application Request Routing server (ARR), I added a binding for HTTPS on port 443 as shown in Figure 2.

![SSL required for http protocol version 2 on IIS 10][FIGURE2]
###### Figure 2, SSL required for http protocol version 2 on IIS 10

To test and confirm that the HTTP/2 protocol is used, I access the ARR server using Microsoft Edge on Windows 10 and captured the Network trace using the F12 Developer Tools.  As you can see in Figure 3, HTTP/2 is shown in the Protocol column and thus supports my initial claim that ARR works with HTTP/2 on IIS 10.

![Using the http/2 protocol over HTTPS from Microsoft Edge, to ARR on IIS 10][FIGURE3]
###### Figure 3, Using the http/2 protocol over HTTPS from Microsoft Edge, to ARR on IIS 10

[FIGURE1]: ../images/2016/msdn-0594.png "Figure 1, Using ARR on IIS 10 with http protocol version 2"
[FIGURE2]: ../images/2016/msdn-0595.png "Figure 2, SSL required for http protocol version 2 on IIS 10"
[FIGURE3]: ../images/2016/msdn-0596.png "Figure 3, Using the http/2 protocol over HTTPS from Microsoft Edge, to ARR on IIS 10"

[LINK1]: http://blogs.iis.net/davidso/http2
[LINK2]: http://www.iis.net/learn/extensions/planning-for-arr/using-the-application-request-routing-module
