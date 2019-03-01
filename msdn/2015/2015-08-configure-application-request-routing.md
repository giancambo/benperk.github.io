# Configure Application Request Routing

There are many documents that cover the configuration of ARR, but this one’s my own.  I write this post to set the base line for another article I wrote here, about configuring ARR to work with Windows Authentication, specifically Kerberos.  After taking the actions in this article and making sure all is running as expected, then you can move onto the next article [here][LINK1], to setup ARR with Windows Authentication.

Here are the steps required to configure Application Request Routing:

+ Install Application Request Routing (use WPI)
+ Configure the Server Farm
+ Test it

## Install Application Request Routing

Install ARR using the Web Platform Installer, for this example I used Application Request Routing 3.0 module, as shown in Figure 1.  *NOTE: IIS must already be installed on the server.  Using the WPI all the dependencies for ARR are installed as well, for example URL Rewrite.  

![Install Application Request Routing module][FIGURE1]
###### Figure 1, Install Application Request Routing module

Once ARR is installed, open the IIS management console and you will see the Server Farms folder as shown in Figure 2.

## Configure the Server Farm
Right-click on the Server Farm folder and select the Create Server Farm… link in the Actions pane.

![the Server Farms folder and Create Server Farm Action link][FIGURE2]
###### Figure 2, the Server Farms folder and Create Server Farm Action link

Enter the name of the Server Farm, as shown in Figure 3 and click the Next button.

![add server farm name to ARR reverse proxy][FIGURE3]
###### Figure 3, add server farm name to ARR reverse proxy

Add the server(s) which ARR will route the traffic to (reverse proxy to).  And click on the Next button, as show in Figure 4.  If you are routing traffic back the same IIS server (looping it back or self-referencing, for some reason), check out [this][LINK2] article.

![add server(s) which ARR will route traffic to, reverse proxy to][FIGURE4]
###### Figure 4, add server(s) which ARR will route traffic to, reverse proxy to

Click the Finish button and then Yes, as shown in Figure 5, to create the Server Level URL Rewrite rule that will reverse proxy / route the request to.

![select the Yes button and complete the ARR Server Farm configuration][FIGURE5]
###### Figure 5, select the Yes button and complete the ARR Server Farm configuration

I recommend that you configure the application pool to be always running and disable the idle timeout setting by giving it the value of 0, as shown in Figure 6.

![ARR application pool recommendations][FIGURE6]
###### Figure 6, ARR application pool recommendations

## Test it

From a client machine (not an IIS server), open a browser and enter the server name of the ARR server and you should see the response from the server(s) configured in the Server Farm and not the actual ARR server.

This example uses the default authentication provider, anonymous.  If you want to learn how to configure ARR to use Windows Authentication, read this [article][LINK1].

[FIGURE1]: ../images/2015/msdn-0541.png "Figure 1, Install Application Request Routing module"
[FIGURE2]: ../images/2015/msdn-0542.png "Figure 2, the Server Farms folder and Create Server Farm Action link"
[FIGURE3]: ../images/2015/msdn-0543.png "Figure 3, add server farm name to ARR reverse proxy"
[FIGURE4]: ../images/2015/msdn-0544.png "Figure 4, add server(s) which ARR will route traffic to, reverse proxy to"
[FIGURE5]: ../images/2015/msdn-0545.png "Figure 5, select the Yes button and complete the ARR Server Farm configuration"
[FIGURE6]: ../images/2015/msdn-0546.png "Figure 6, ARR application pool recommendations"

[LINK1]: 2015-08-configure-application-request-routing-with-windows-authentication-kerberos.md
[LINK2]: 2015-03-application-request-routing-arr-self-referencing-itself-400-or-502-3.md
