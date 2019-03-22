# Lab 13: Using LogParser

## Setup

+ Install LogParser 2.2 from [here][LINK1].
+ Remember that IIS Log default to GMT and not local time, write queries accordingly
+ Review [this][LINK2] article which goes into great detail about LogParser
+ Review [this][LINK3] link which describes HTTP error codes
+ Review [this][LINK4] link which describes IIS error codes
+ You can get a copy of the queries, iis logs, http logs and event logs [here][LINK5].

## Find out what caused a performance issue and conclude why

1. Install and open LogParser in a command window

![Using LogParser][FIGURE1]
###### Figure 1, Using LogParser

2. Place the IIS logs into the same folder as LogParser and open Queries.txt for example queries.
3. Execute IIS query -----------1------------, what does it tell you?

![Using LogParser][FIGURE2]
###### Figure 2, Using LogParser

a. Q: What can you derive from a ratio between 200 and 304 status codes? A: A 200 means that the server provided the file to the client and it was a successful request.  A 304 means the file was retrieved from the client cache and no file was sent from the server.  This improves the performance of the application and reduces the load on the server.  The higher number of 304s is a good thing in relation to the 200s because it means there is less reliance on the server.

b. Use NET HELPMSG #, to find out what the sc-win32-status number means.

![MSDN][FIGURE3]
###### Figure 3, MSDN

c. Q:  Which sc-status codes do you want to analyze further?  A: The 500s would be of particular interest because that means there was some kind of error responding to that request.  The 64 sc-win32-status usually means that the IIS server can no longer determine where to send the response to,  This happens often when requests take a while to respond and the user closes or refreshes the browser.

4. Execute IIS query -----------2------------, what does it tell you?

![Using LogParser][FIGURE4]
###### Figure 4, Using LogParser

a. Q: Are those MAX values too big? A: The MAX time-taken looks long, 65 seconds, also the MAX cs-bytes and MAX sc-bytes look large as well.  Looking at each of those requests to see if it is expected would be a valid approach and worth the effort.

5. Execute IIS query -----------3------------, Q: What does it tell you? A: Now I can see which specific request took so long and when it happened, I.e. I can see the specific request that took the MAX time-taken.  I could then mention this to the developers and see if this is normal behavior..

![Using LogParser][FIGURE5]
###### Figure 5, Using LogParser

a. Q:  Any relation between request/response size and time-taken?  A: In this example, the size of the file is pretty small, so it does not look like the slowness was caused by the file size.

b. Q: What can you conclude when comparing the relationship between request/response size and time-taken?  A:  If the time-taken is large and the size of the file is large, the reason for the slowness is likely due to the file size.  If the file size is small then the issue may be caused by a network issue or a resource constraint, like 100% CPU consumption on the server.

6. Execute IIS query -----------4------------ to find the requests that had the largest sc-bytes size, this is the size of the file send from the server to the client, what does it tell you?   Recall question 5b, is there a relation between sc-bytes (file size) and the time it takes to complete a request, I.e. time-taken.  It looks like there is a relationship.

![Using LogParser][FIGURE6]
###### Figure 6, Using LogParser

a. Q: What file size is too big? A: The larger the file, if it is not cached on the client, the longer it takes to download.  So the smaller the better and do your best to cache them on the client.  Q: Should you reduce the sc-bytes value?  A: In this case the file size is ok.

7. Execute IIS query -----------5------------ to take a closer look at the requests with a large cs-bytes value, what does it tell you?   cs-bytes is the size of the data sent from the client to the server.  Again, I see a relationship between size and time-taken, do you?  Also,notice that the cs-method, or verb is POST, which means there is a form on the page that a user is likely entering data and sending it to the server.  At this level you cannot really go deeper, but you can ask the developer if this is expected behavior.  It is a clue that may lead to finding out why the web site is not running as expected.

![Using LogParser][FIGURE7]
###### Figure 7, Using LogParser

a. Q:  Why would a client side file be big? A: Perhaps the client is uploading a file to the server or the client is posting contents of 

a form to the server. Q: What is too big?  A: there is really no hard rule on what is too big, if a file needs to the uploaded that is large you should consider streaming it using an asynchronous process.

8. Execute IIS query -----------6------------ to find out what requests resulted in a 500 IIS error, what does it tell you?  It tells me that one of the pages renders a 500 much more often than the others. Therefore, I would look into that file and the code to see if it does anything different from the other that may result in a higher probability of error.

![Using LogParser][FIGURE8]
###### Figure 8, Using LogParser

a. Q: What causes 500’s… See Lab 25.  A: These errors are typically caused by server side code.  Look at the sub-status code for more tips.

b. Q: What actions can you take in IIS to prevent or troubleshoot them? A: exceptions do happen, but through logging and analysis of those logs you can pinpoint where the error is coming from and repair it.  With logging, you would never find it.

9. Q: What is your overall conclusion and course of action? A: Overall the site is healthy excluding a few large XML files, a form that allows for the posting of a large amount of data and a few 500s.

10. Execute HTTP query -----------------1--------------- to see all the errors reported at the HTTP.sys level, what does it tell you?

a. Don’t forget to change the Input Log Format…

![Using LogParser][FIGURE9]
###### Figure 9, Using LogParser

b. Q: What do each of these s-reason values mean? A: Descriptions are discussed here.

c. Q: What conclusions can you make, how should/can you analyze further?  A: after reviewing what these s-reason codes mean, I’d looked at the ones which mean the most to me.  Also, the one which is occurring the most seems like a good place to start.

11. Execute HTTP query -----------------2--------------- to look at the specific request which has a s-reason of URL, what does it tell you?  Looks like somebody is trying to execute some javascript in one of my forms.  Perhaps send this on to the security team or make sure I have not enabled unvalidated requests and I have some understanding of the AntiXSS library, what it does and how to implement it.

![Using LogParser][FIGURE10]
###### Figure 10, Using LogParser

a. Search using date/time… perhaps this activity happened one a specific day from a specific IP address…

12. What are your conclusions?

13. Execute Event query -----------------1--------------- to get a view of all the System events that happened on the server, what does it tell you?

![Using LogParser][FIGURE11]
###### Figure 11, Using LogParser

a. Q:  Which Event Types are important?  A:  EventTypes = 1 are errors and I would look at those in more detail using a query that gets the detail of the specific log.

b. Open the Event File in Event Viewer and view the details of events.

c. Q: How do you troubleshoot them further?  A:  Search online for the EventId or if there is a stack in the details of the event you can look if there are any known issues and perhaps there is a hotfix for it.

14. Execute Event query -----------------2--------------- to get a view of the application events that happened on the server, what does it tell you?

![Using LogParser][FIGURE12]
###### Figure 12, Using LogParser

a. Same as previous…

[FIGURE1]: ../images/2016/msdn-0816.png "Figure 1, Using LogParser"
[FIGURE2]: ../images/2016/msdn-0817.png "Figure 2, Using LogParser"
[FIGURE3]: ../images/2016/msdn-0818.png "Figure 3, Using LogParser"
[FIGURE4]: ../images/2016/msdn-0819.png "Figure 4, Using LogParser"
[FIGURE5]: ../images/2016/msdn-0820.png "Figure 5, Using LogParser"
[FIGURE6]: ../images/2016/msdn-0821.png "Figure 6, Using LogParser"
[FIGURE7]: ../images/2016/msdn-0822.png "Figure 7, Using LogParser"
[FIGURE8]: ../images/2016/msdn-0823.png "Figure 8, Using LogParser"
[FIGURE9]: ../images/2016/msdn-0824.png "Figure 9, Using LogParser"
[FIGURE10]: ../images/2016/msdn-0825.png "Figure 10, Using LogParser "
[FIGURE11]: ../images/2016/msdn-0826.png "Figure 11, Using LogParser"
[FIGURE12]: ../images/2016/msdn-0827.png "Figure 12, Using LogParser"

[LINK1]: https://www.microsoft.com/en-us/download/details.aspx?id=24659
[LINK2]: http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser
[LINK3]: http://support.microsoft.com/?id=820729
[LINK4]: https://support.microsoft.com/en-us/kb/943891
[LINK5]: https://benperk.github.io/
