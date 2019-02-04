# Displaying detailed error messages in IIS

One of the first questions I ask when troubleshooting an IIS case is "What HTTP Status code are you receiving?"  I ask this question because it sets the direction for further analysis.  In this example I want to discuss the 404 status code and how you might need to make some changes or do some further analysis when this status is rendered.  In most cases a 404 is pretty easy, it means that the resource you are looking for cannot be found.  Probably because the file was deleted or there is a typo in the name or path to the file.  After checking for the existence of the resource and confirming there are no typos, what is your next step?  Check the sub-status code.

There are more than 20 sub-status codes for 404.  You can check the IIS logs for the sub-status codes using [LogParser][LINK1] or you can open the IIS log and do a manual search.  However, for this example I want to show and discuss the configuration of local and remote error messages.

If you navigate to the Error Pages feature and then select Edit Feature Settings… from the Actions pane you will see the windows as shown in Figure 1.

![Edit Error Pages Setting - Default][FIGURE1]
###### Figure 1, Edit Error Pages Setting - Default

Notice that the "Detailed error for local requests and customer error pages for remote requests" is select by default.  This means that the error page will be different based on the location which you are accessing the resource.

## Remote Error Pages

In most cases, you do not want the client to receive too much information when an error happens on your website.  Therefore, when a client attempts the request shown in Figure 2, the associated remote error page is rendered.

![Remote Error Page][FIGURE2]
###### Figure 2, Remote Error Page

You may then receive reports from your clients that the file which they are trying to access is not found.  Also, when you (IT support) access the same URL, from your client, you will receive the same error message.  However, when you login to the server and check for the file, it DOES exist!!!

## Local Error Pages

When you have access to the web server which is returning the 404 status code, you can simply open a browser on the web server itself and attempt to access the "missing" file.  While on the web server you would receive the error page as shown in Figure 3.

![Local Error Page][FIGURE3]
###### Figure 3, Local Error Page

So there you have it, it is indeed a 404 error, but actually it is a 404.11 error which means something different.  This one means, as stated in the error page: "The request filtering module is configured to deny a request that contains a double escape sequence." 
You also notice that there is a "Things you can try" section with a tip on how to resolve the 404.11 error.  Making the recommended change to the requestFiltering@allowDoubleEscaping element, in this case, will result in the page being rendered as expected.

But what if you don't have access to the web server?  In some circumstances it may be acceptable to turn on detailed errors for remote clients.  To do this, select the Detailed errors radio button on the Edit Error Pages Settings window as shown in Figure 4.

![Edit Error Pages Setting - Detailed errors][FIGURE4]
###### Figure 4, Edit Error Pages Setting - Detailed errors

After selecting the OK button, you should read the message placed into the Alerts pane.  Then, when a client accesses the URL remotely, they are provided with the same detailed error page is rendered locally, as shown in Figure 5.

![Remote Error Page - Detailed errors][FIGURE5]
###### Figure 5, Remote Error Page - Detailed errors

Perhaps you can configure the error pages to return detailed errors when there are some problems and then turn it back off when you have resolved the problem.  Either way, I found this one a little tricky at first glance, but it was a relatively simple fix once you look at the sub-status codes.

[LINK1]: http://www.microsoft.com/en-us/download/details.aspx?id=24659

[FIGURE1]: ../images/2012/msdn-0132.png "Figure 1, Edit Error Pages Setting - Default"
[FIGURE2]: ../images/2012/msdn-0133.png "Figure 2, Remote Error Page"
[FIGURE3]: ../images/2012/msdn-0134.png "Figure 3, Local Error Page"
[FIGURE4]: ../images/2012/msdn-0135.png "Figure 4, Edit Error Pages Setting - Detailed errors"
[FIGURE5]: ../images/2012/msdn-0136.png "Figure 5, Remote Error Page - Detailed errors"

