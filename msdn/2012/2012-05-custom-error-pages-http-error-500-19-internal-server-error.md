# Custom Error Pages – HTTP Error 500.19 – Internal Server Error

Rendering custom error pages can be helpful when, for example, the custom error page provides some information on how to resolve the error.  Perhaps if a client attempts to access a file which does not exist on the web site, meaning they receive a 404 HTTP status code, you can render a page which has existing similar named files, provide a site search capability or redirect the request to the web sites default page.
Before you can do that, you need to configure the Error Pages on IIS for this.  On my IIS 8 instance, the default server level settings can be viewed by clicking the icon as Figure 1 shows.

![IIS 8 Custom Error Pages default][FIGURE1]
###### Figure 1, IIS 8 Custom Error Pages default

Without modification when a client receives a 404 HTTP response, they will receive the message shown in Figure 2.

![IIS 8 default 404 error page][FIGURE2]
###### Figure 2, IIS 8 default 404 error page

Let's say instead of rendering the default 404 page, I want to modify it and make it match the style of my website.  To do this, navigate to the "Default Web Site" node, for this example, and double-click on the Error Pages icon in the Features pane.  Then click on the 404 found in the Status Code column and then ***Edit…*** on the Actions pane as shown in Figure 3.

![Edit a Site level Error Page][FIGURE3]
###### Figure 3, Edit a Site level Error Page

Next, within the Edit Custom Error Page window, deselect the "Try to return the error file in the client language" check box, set the File path as shown in Figure 4.

![Edit Customer Error Page and set the File path][FIGURE4]
###### Figure 4, Edit Customer Error Page and set the File path

Then select the ***Edit Feature Settings…*** from the Actions pane and select the Custom error pages radio button, as shown in Figure 5.

![Configure Error Responses][FIGURE5]
###### Figure 5, Configure Error Responses

Then browse to the Default Web Site and enter a file that does not exist.  When I do this it results in the following:

***HTTP Error 500.19 - Internal Server Error
Absolute physical path "C:\inetpub\wwwroot\ErrorPages" is not allowed in system.webServer/httpErrors section in web.config file. Use relative path instead.***

That did not seem to work out as expected. 

To resolve this error, select the server node within IIS  (ex. SERVERNAME\Administrator) and within the Features pane double-click the Configuration Editor icon.  Navigate to the system.webServer/httpErrors element, as shown in Figure 6 and change allowAbsolutePathsWhenDelegated to true, then Apply….

![Configuration Editor setting allowAbsolutePathsWHenDelegated][FIGURE6]
###### Figure 6, Configuration Editor setting allowAbsolutePathsWHenDelegated

Return to the "Default Web Site" and navigate to a file which does not exist.  You will see the custom error page as shown in Figure 7.

![a custom, more friendly error page][FIGURE7]
###### Figure 7, a custom, more friendly error page




[FIGURE1]: ../images/2012/msdn-0122.png "Figure 1, IIS 8 Custom Error Pages default"
[FIGURE2]: ../images/2012/msdn-0123.png "Figure 2, IIS 8 default 404 error page"
[FIGURE3]: ../images/2012/msdn-0124.png "Figure 3, Edit a Site level Error Page"
[FIGURE4]: ../images/2012/msdn-0125.png "Figure 4, Edit Customer Error Page and set the File path"
[FIGURE5]: ../images/2012/msdn-0126.png "Figure 5, Configure Error Responses"
[FIGURE6]: ../images/2012/msdn-0127.png "Figure 6, Configuration Editor setting allowAbsolutePathsWHenDelegated"
[FIGURE7]: ../images/2012/msdn-0128.png "Figure 7, a custom, more friendly error page"
