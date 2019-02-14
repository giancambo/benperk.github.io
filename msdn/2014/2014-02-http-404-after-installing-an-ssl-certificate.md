# HTTP 404 after installing an SSL Certificate

I was configuring an SSL certificate and once complete, when I attempted to access the website using https://localhost I started receiving a 404 error as shown in Figure 1.

![404 HTTP Status after installing an SSL Certificate][FIGURE1]
###### Figure 1, 404 HTTP Status after installing an SSL Certificate

I tried some shots at troubleshooting the issue using [Failed Request Tracing][LINK1] and [LogParser][LINK2] to view the IIS logs, but didn’t get a Failed Request Trace and I found no entry in the IIS logs.  I confirmed that there was a default document in the websites directory and even browsed right to it and still got a 404.  Obviously, this is not a File Not Found issue.

My bindings were similar to that shown in Figure 2.

![SSL/443 binding causing a 404][FIGURE2]
###### Figure 2, SSL/443 binding causing a 404

As the issue was not being logged into the IIS logs, I concluded that the issue must be happening at the HTTP level.  So I looked in the HTTP Error logs located by default at c:\windows\system32\logfiles\HTTPERR and did find the error being logged, a subset of that is shown below.

```
2013-11-12 13:15:58 ::1%0 61522 ::1%0 443 HTTP/1.1 GET / 404 - NotFound -
2013-11-12 13:16:23 ::1%0 61526 ::1%0 443 HTTP/1.1 GET / 404 - NotFound -
2013-11-12 13:17:08 ::1%0 61533 ::1%0 443 HTTP/1.1 GET / 404 - NotFound -
```

After some effort and just looking at the bindings again, I determined that the reason for the error was due to the fact that I was accessing the website using https://localhost for which there was no binding.  I.e. there was no binding for *:443 or localhost:443.

I should note that I added the following entry to the HOSTS file so that https://invalidhostname did indeed render as expected.  Ex. ***“127.0.0.1 invalidhostname”***

I updated the bindings so they reflected that shown in Figure 3, so that when I now made the request, the 404 was gone.

![SSL/443 binding causing a 404, solution][FIGURE3]
###### Figure 3, SSL/443 binding causing a 404, solution

This was a problem that took me some time to resolve, so just sharing in case it saves others some time.

[FIGURE1]: ../images/2014/msdn-0325.png "Figure 1, 404 HTTP Status after installing an SSL Certificate"
[FIGURE2]: ../images/2014/msdn-0326.png "Figure 2, SSL/443 binding causing a 404"
[FIGURE3]: ../images/2014/msdn-0327.png "Figure 3, SSL/443 binding causing a 404, solution"

[LINK1]: ../2012/2012-01-enable-and-activate-failed-request-tracing-rules.md
[LINK2]: http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser
