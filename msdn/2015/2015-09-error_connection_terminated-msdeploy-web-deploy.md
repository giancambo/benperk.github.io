# ERROR_CONNECTION_TERMINATED msdeploy web deploy

****NOTE****:  This exception happens most often when your corporate IT department has a firewall rule or proxy which is preventing the deployment to be made.  The first step should be that you attempt a deployment outside of your corporate network, perhaps from home when connected on your private ISP.

I was doing some deployments to my Azure Web App one Saturday night and started getting this error show in Figure 1.

![ERROR_CONNECTION_TERMINATED Azure Web App, web deploy][FIGURE1]
###### Figure 1, ERROR_CONNECTION_TERMINATED Azure Web App, web deploy

Here is the message specifically:

Web deployment task failed. (Web Deploy experienced a connection problem with the server and had to terminate the connection.  Contact your server administrator if the problem persists.  Learn more at: http://go.microsoft.com/fwlink/?LinkId=221672#ERROR_CONNECTION_TERMINATED.)     

I did some checks to make sure the platform was ok and that the SCM W3WP process was up and running as expected, while I was checking, I noticed how slow my internet connection was.  Accessing any web site was very slow.  Therefore, I concluded this issue has to do with the network speed and ability to stay connected and send the content to the Azure Web App.

Just to be certain, I attempted the same in the office.  I used a tool called CLUMSY 0.2 to simulate a slow network and performed a similar kind of deployment, and bang, the same issue.

I performed the same using the Publish-AzureWebsiteProject PowerShell command, and had the same results as shown in Figure 2.

![RROR_CONNECTION_TERMINATED PowerShell deployment][FIGURE2]
###### Figure 2, RROR_CONNECTION_TERMINATED PowerShell deployment

The first 2 attempts worked just fine, but as soon as the network stated being slow or packets being dropped, exceptions started happening.

I looked around for some kind of explanation for this exception and it was not easy to find any feedback on it.  This is just my experience with it and because I could reproduce it, there is a high probability that the issue is with slow networks connectivity.

Additionally, perhaps the size of deployment is very big and times out.  You might consider using FTP to deploy you Azure Web App initially and then deploy the changes using web deploy, PowerShell or the Publish… utility in Visual Studio.

***NOTE***:  This wouldn’t only apply to Azure Web App, I’d expect the same to be true when using Web Deploy to deploy to any web site where the network is having some latency problems.

***NOTE 2***:  Check if Fiddler is running, if it is, stop it.  See comment section, thanks.

[FIGURE1]: ../images/2015/msdn-0560.png "Figure 1, ERROR_CONNECTION_TERMINATED Azure Web App, web deploy"
[FIGURE2]: ../images/2015/msdn-0561.png "Figure 2, RROR_CONNECTION_TERMINATED PowerShell deployment"
