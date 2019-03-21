# Lab 10: Using ASP.NET Tracing (Trace=“True“)

## Prerequisites

+ Review this post [here][LINK1]
+ Review or complete [Lab 4][LINK2] which provides similar performance information

## Setup

+ Install and Configure CSharpGuitarBugs on an IIS server as described in [Lab 1][LINK3]

## Lab 10-1

1. Click on the the FullCatalog –ASPX page located within the CSharpGuitarBugs website. About how long did it take? The goal is to find out in which ASP.NET pipeline event is the code executing the longest.

2. Open the WebForms/Fullcatalog.aspx file and add Trace=”true” to the page directives.

![Using ASP.NET Tracing (Trace=“True“)][FIGURE1]
###### Figure 1, Using ASP.NET Tracing (Trace=“True“)

3. You can also turn on Tracing at the Application level so that every ASPX page has the performance report.

![Using ASP.NET Tracing (Trace=“True“)][FIGURE2]
###### Figure 2, Using ASP.NET Tracing (Trace=“True“)

4. Access the page again and scroll to the bottom. Where is the code executing? This information is valuable for the developer team so they can look at that method to see why it is taking so long.

![Using ASP.NET Tracing (Trace=“True“)][FIGURE3]
###### Figure 3, Using ASP.NET Tracing (Trace=“True“)

5. Open the FullCatalog.aspx.cs file and look at the method, what is the problem?

## Lab 10-2

1. Click the Refresh button on the FullCatalog.aspx page.  If you get an error “A connection attempt failed because the connected party…” the open the FullCatalog.aspx.cs file and set the URL of the HttpWebRequest to direct to the LOCALHOST:PORT for your server.

2. If you cannot get the change to apply, check our [Lab 28][LINK4] and deleted the Temporary ASP.NET files.

3. Press the Refresh button, if the Trace=”true” is not already added to the FullCatalog.aspx page, then please add it as per Lab 10-1.

4. Scroll to the bottom and find the useful piece of information.

![Using ASP.NET Tracing (Trace=“True“)][FIGURE4]
###### Figure 4, Using ASP.NET Tracing (Trace=“True“)

![Using ASP.NET Tracing (Trace=“True“)][FIGURE5]
###### Figure 5, Using ASP.NET Tracing (Trace=“True“)

5. Look at that method in the FullCatalog.aspx.cs code and find what the problem is.

6. If you know how to use Fiddler, you can check and see a little more about what is happening.

![Using ASP.NET Tracing (Trace=“True“)][FIGURE6]
###### Figure 6, Using ASP.NET Tracing (Trace=“True“)

![Using ASP.NET Tracing (Trace=“True“)][FIGURE7]
###### Figure 7, Using ASP.NET Tracing (Trace=“True“)

***TIP***:  To find the size of a request header, you can copy the contents of the Request Header seen in Fiddler into a Notepad, then save it.  Right-click on the txt file and view the properties.

8. Recommend to the developer of the code to place the HttpWebRequest into a try{}catch{} block and write the exception to the page instead of letting it crash.

9. What is the issue?  Yes, the request header is too big.  You will need to adjust the MaxFieldLength and MaxRequestBytes as described in more detail [here][LINK5].  Also, as per [Lab 6][LINK6] when you make a change to the registry you must restart HTTP.  Remember that NET STOP HTTP stops W3SVC but NET START HTTP does not start W3SVC, so you will need to also run NET START W3SVC for the web site to respond after running a NET STOP HTTP.

![Using ASP.NET Tracing (Trace=“True“)][FIGURE8]
###### Figure 8, Using ASP.NET Tracing (Trace=“True“)

10. Try to reproduce the issue again and it no longer happens.

[FIGURE1]: ../images/2016/msdn-0789.png "Figure 1, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE2]: ../images/2016/msdn-0790.png "Figure 2, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE3]: ../images/2016/msdn-0791.png "Figure 3, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE4]: ../images/2016/msdn-0792.png "Figure 4, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE5]: ../images/2016/msdn-0793.png "Figure 5, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE6]: ../images/2016/msdn-0794.png "Figure 6, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE7]: ../images/2016/msdn-0795.png "Figure 7, Using ASP.NET Tracing (Trace=“True“)"
[FIGURE8]: ../images/2016/msdn-0796.png "Figure 8, Using ASP.NET Tracing (Trace=“True“)"

[LINK1]: http://msdn.microsoft.com/en-us/library/94c55d08(v=vs.71).aspx
[LINK2]: 2016-IISLAB-lab-4-install-and-configure-failed-request-tracing.md
[LINK3]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK4]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/06/20/lab-28-the-impact-of-debugtrue/
[LINK5]: https://support.microsoft.com/en-us/kb/820129
[LINK6]: 2016-IISLAB-lab-6-using-http-w3svc-was-and-w3wp.md
