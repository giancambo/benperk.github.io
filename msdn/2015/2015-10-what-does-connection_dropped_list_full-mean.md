# What does Connection_Dropped_List_Full mean

When you look in the HTTP Error log (c:\Windows\System32\LogFiles\HTTPERR\) and see a lot of Connection_Dropped_List_Full error reasons logged, it means that the client has dropped the connection so many times, in a short period, that the log in which the dropped connections are stored has become full.  You might also see numerous 200.0.64 HTTP Status codes in the IIS logs.  A 200.0.64 means the client has closed the connection before IIS had a chance to send or completely send the response back to the client.

The description of Connection_Dropped_List_Full [here][LINK1] itself seems clear enough, however, examples of when this is experienced may be helpful.

The situation where I experienced this was when there was a hardware load balancer in front of a web farm and the load balancer had taken the webservers out of service (marked them as unhealthy).  For some reason the LB still sent the request to IIS, but then immediately closed the connection, so IIS had no place to send the response back to. 

I took a Failed Request Trace, as I describe [here][LINK2] and found in the GENERAL_REQUEST_HEADERS that, the Headers attribute was being set to ***Connection: Close***, I wouldn’t normally expect to see that, expecting instead ***Connection: Keep-Alive***.

If you get this error, check to make sure that there are no rules on the load balancer that remove servers based on response time.  During a burst of users, like when everyone logs in, in the morning for example, there could be a large increase of traffic and the removal of servers from the farm could happen, because the servers' response may slow due to this burst.  As the other remaining, 'healthy' servers attempt to manage the additional load, they too could ultimately get removed, as they would likely take longer than normal to respond to the burst of requests (domino effect).  Ultimately, all the web servers in farm get removed from the LB based on the rule and your site would then be unavailable via the LB...accessing the web servers directly would likely still work.

[LINK1]: http://support.microsoft.com/kb/820729
[LINK2]: http://blogs.msdn.com/b/benjaminperkins/archive/2012/01/02/enable-and-activate-failed-request-tracing-rules.aspx
