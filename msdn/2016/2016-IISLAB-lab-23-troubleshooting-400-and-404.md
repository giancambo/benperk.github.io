# Lab 23: Troubleshooting 400 and 404

## Prerequisites

+ Review the requestFiltering attribute [here][LINK1]
+ Read information about HTTP 400 – Bad Request [here][LINK2]
+ Check out the new Microsoft Message Analyzer [here][LINK3]
+ Check out Network Monitor [here][LINK4]

## Setup

+ Install and Configure CSharpGuitarBugs on an IIS server as described in [Lab 1][LINK5]

## Lab 23-1

1. Access the CSharpGuitarBugs website from a client machine, not from the IIS server, click on Full Catalog – ASPX, press the Next Page button and the following error is rendered, why? Q: Does the file being requested exist? A: Yes

![Troubleshooting 400 and 404][FIGURE1]
###### Figure 1, Troubleshooting 400 and 404

1. If it exists, then why are you getting a 404? 404’s have a lot of sub-status codes… Q: Where do you find the sub-status code and what is it?  A: See options a. – d.

a. Access the website while on the server.

b. Look in the IIS logs

c. Failed Request Tracing logs

d. Turn on remote error rendering, by modifying the web.config file like the following

![Troubleshooting 400 and 404][FIGURE2]
###### Figure 2, Troubleshooting 400 and 404

2. Access the same file from a browser while on the server and you see this error

![Troubleshooting 400 and 404][FIGURE3]
###### Figure 3, Troubleshooting 400 and 404

3. To resolve this issue set allowDoubleEscaping to true

![Troubleshooting 400 and 404][FIGURE4]
###### Figure 4, Troubleshooting 400 and 404

4. Refresh the website from a client machine, 404.11 is gone, continue to next lab, because now you get another error, right?

## Lab 23-2

1. Access the CSharpGuitarBugs website from a client machine, not from the IIS server, click on Full Catalog – ASPX, press the Next Page button and the following error is rendered when selecting the following button, why?

2. What tools can you use to get more information from the client side? If you already configured customErrors in Lab 23-1, 1d, then you will not see the above, instead you will see the below. You can also use Fiddler or

![Troubleshooting 400 and 404][FIGURE6]
###### Figure 6, Troubleshooting 400 and 404

![Troubleshooting 400 and 404][FIGURE7]
###### Figure 7, Troubleshooting 400 and 404

3. Access the same from the IIS server.

![Troubleshooting 400 and 404][FIGURE8]
###### Figure 8, Troubleshooting 400 and 404

4. How do you fix this? Q: Can it be fixed on the IIS side? A: No.  Colons are only allowed in the query string after a ?, you cannot have one anyplace in the URL

## Lab 23-3

## Setup

+ Make sure customErrors mode = ON, this is the most likely configuration so need to know how to troubleshoot without it
+ Check out [Lab 10][LINK6] for a similar lab.  Lab 10 also for the solution to this lab, I.e. change the MaxFieldLength and MaxRequestBytes both discussed further [here][LINK7]

1. Access the CSharpGuitarBugs website from a client machine, not from the IIS server, click on Full Catalog – ASPX, press the Refresh button and the following error is rendered when selecting the following button, why?

2. What error do you get, how can you trouble shoot it?

a. Look in the IIS logs, what Status Code is being returned?

![Troubleshooting 400 and 404][FIGURE9]
###### Figure 9, Troubleshooting 400 and 404

b. Look in the HTTP Error log, what is being returned there?

![Troubleshooting 400 and 404][FIGURE10]
###### Figure 10, Troubleshooting 400 and 404

c. Try Network Monitor…

d. Try Fiddler

![Troubleshooting 400 and 404][FIGURE11]
###### Figure 11, Troubleshooting 400 and 404

e. Last, set customError=Off

![Troubleshooting 400 and 404][FIGURE12]
###### Figure 12, Troubleshooting 400 and 404

![Troubleshooting 400 and 404][FIGURE13]
###### Figure 13, Troubleshooting 400 and 404

## Lab 23-4

Take a Network Monitor Trace, reproduce the 400 and analyze it

1. Make sure customErrors mode=”Off” for the 400 web site, what happens when it is On?

2. Install Network Monitor on a remote machine, not on IIS

3. Start a Capture and reproduce the 400.

![Troubleshooting 400 and 404][FIGURE14]
###### Figure 14, Troubleshooting 400 and 404

4. Stop the Capture

![Troubleshooting 400 and 404][FIGURE15]
###### Figure 15, Troubleshooting 400 and 404

5. Enter HTTP into the Filter box and Apply

![Troubleshooting 400 and 404][FIGURE16]
###### Figure 16, Troubleshooting 400 and 404

6. Click the process where the error was capture within

![Troubleshooting 400 and 404][FIGURE17]
###### Figure 17, Troubleshooting 400 and 404

7. Find and review the error

![Troubleshooting 400 and 404][FIGURE18]
###### Figure 18, Troubleshooting 400 and 404

8. Open the same in Microsoft Message Analyzer

![Troubleshooting 400 and 404][FIGURE19]
###### Figure 19, Troubleshooting 400 and 404

![Troubleshooting 400 and 404][FIGURE20]
###### Figure 20, Troubleshooting 400 and 404

![Troubleshooting 400 and 404][FIGURE21]
###### Figure 21, Troubleshooting 400 and 404

[FIGURE1]: ../images/2016/msdn-0904.png "Figure 1, Troubleshooting 400 and 404"
[FIGURE2]: ../images/2016/msdn-0905.png "Figure 2, Troubleshooting 400 and 404"
[FIGURE3]: ../images/2016/msdn-0906.png "Figure 3, Troubleshooting 400 and 404"
[FIGURE4]: ../images/2016/msdn-0907.png "Figure 4, Troubleshooting 400 and 404"
[FIGURE6]: ../images/2016/msdn-0909.png "Figure 6, Troubleshooting 400 and 404"
[FIGURE7]: ../images/2016/msdn-0910.png "Figure 7, Troubleshooting 400 and 404"
[FIGURE8]: ../images/2016/msdn-0911.png "Figure 8, Troubleshooting 400 and 404"
[FIGURE9]: ../images/2016/msdn-0912.png "Figure 9, Troubleshooting 400 and 404"
[FIGURE10]: ../images/2016/msdn-0913.png "Figure 10, Troubleshooting 400 and 404"
[FIGURE11]: ../images/2016/msdn-0914.png "Figure 11, Troubleshooting 400 and 404"
[FIGURE12]: ../images/2016/msdn-0915.png "Figure 12, Troubleshooting 400 and 404"
[FIGURE13]: ../images/2016/msdn-0916.png "Figure 13, Troubleshooting 400 and 404"
[FIGURE14]: ../images/2016/msdn-0917.png "Figure 14, Troubleshooting 400 and 404"
[FIGURE15]: ../images/2016/msdn-0918.png "Figure 15, Troubleshooting 400 and 404"
[FIGURE16]: ../images/2016/msdn-0919.png "Figure 16, Troubleshooting 400 and 404"
[FIGURE17]: ../images/2016/msdn-0920.png "Figure 17, Troubleshooting 400 and 404"
[FIGURE18]: ../images/2016/msdn-0921.png "Figure 18, Troubleshooting 400 and 404"
[FIGURE19]: ../images/2016/msdn-0922.png "Figure 19, Troubleshooting 400 and 404"
[FIGURE20]: ../images/2016/msdn-0923.png "Figure 20, Troubleshooting 400 and 404"
[FIGURE21]: ../images/2016/msdn-0924.png "Figure 21, Troubleshooting 400 and 404"

[LINK1]: http://www.iis.net/configreference/system.webserver/security/requestfiltering
[LINK2]: http://support.microsoft.com/kb/2020943
[LINK3]: http://blogs.technet.com/b/messageanalyzer/
[LINK4]: http://www.microsoft.com/en-us/download/details.aspx?id=4865
[LINK5]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK6]: 2016-IISLAB-lab-10-using-asp-net-tracing-tracetrue.md
[LINK7]: https://support.microsoft.com/en-us/kb/820129
