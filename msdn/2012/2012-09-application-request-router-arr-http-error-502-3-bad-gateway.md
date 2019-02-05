# Application Request Router (ARR) – HTTP Error 502.3 – Bad Gateway

I was setting up a web farm with Web Farm Framework (WFF) and Application Request Router (ARR) and received the error shown in Figure 1/2.

![HTTP Error 502.3 - Bad Gateway][FIGURE1]
###### Figure 1, HTTP Error 502.3 - Bad Gateway

![HTTP Error 502.3 - Bad Gateway][FIGURE2]
###### Figure 2, HTTP Error 502.3 - Bad Gateway

I checked the Monitoring and Management window to see if the Health Status of the server as shown in Figure 3.  Take note, that if you have not configured a Health Test, then they will have a 'Healthy' status.  So, don't get confused on this one.

![Health Status][FIGURE3]
###### Figure 3, Health Status

I thought I was having an infinite Loop back problem, because it was taking some time to respond and the ARR/WFF Server was itself part of the farm.  Therefore, I modified the default URL Rewrite an added a condition, shown in Figure 3, that would stop any looping from happening.

![URL Rewrite stop looping][FIGURE4]
###### Figure 4, URL Rewrite stop looping

That didn't fix the problem.

I logged into each of the servers and checked that the Default Web Site was running and sure enough, one of them was not.  I started the server and then it worked as expected.

![IIS 8 Splash][FIGURE5]
###### Figure 5, IIS 8 Splash

Point is, it is a good idea to have a Health Test setup in your environment.  This would have saved me some time because I would have seen right off in the Monitoring and Management feature which server was Unhealthy.  Instead, I had to login to each server in the farm and check manually.

This was an easy one to solve, but thought I write a post just in case it helps someone…somewhere.

UPDATE: Note that by adding unsafe for caching server variables to your URL Rewrite Rule results in some caching to be disabled.  For example the HTTP_REFERER.  Read more about this [here][LINK1].

[LINK1]: http://www.iis.net/learn/extensions/url-rewrite-module/url-rewrite-module-configuration-reference#Interaction_with_IIS_Output_Caching

[FIGURE1]: ../images/2012/msdn-0164.png "Figure 1, HTTP Error 502.3 - Bad Gateway"
[FIGURE2]: ../images/2012/msdn-0165.png "Figure 2, HTTP Error 502.3 - Bad Gateway"
[FIGURE3]: ../images/2012/msdn-0166.png "Figure 3, Health Status"
[FIGURE4]: ../images/2012/msdn-0168.png "Figure 4, URL Rewrite stop looping"
[FIGURE5]: ../images/2012/msdn-0167.png "Figure 5, IIS 8 Splash"
