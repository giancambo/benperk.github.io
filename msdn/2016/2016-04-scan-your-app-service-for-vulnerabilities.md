# Scan your App Service for vulnerabilities

A recent feature deployed for App Services (Web App, Mobile App, API App or Logic App) is a tool that can scan your App Service for vulnerabilities, announced [here][LINK1].

Once you have signed up, you will see a link to your management console similar to that shown in Figure 1.

![check your App Service for vulnerabilities][FIGURE1]
###### Figure 1, check your App Service for vulnerabilities

It did take some minutes for the extension to be installed, be a little patient, but in the end it worked just fine and clicking on the link navigated me to the extension within my KUDU console, Figure 2. I discuss what KUDU is [here][LINK2].

![scanning an App Service (Web App, Mobile App, API App and Logic App) for security][FIGURE2]
###### Figure 2, scanning an App Service (Web App, Mobile App, API App and Logic App) for security

You can also see the installed extension in KUDU by clicking on the Site Extensions link, similar to that shown in Figure 3.

![security App Service, secure cloud, secure Web App][FIGURE3]
###### Figure 3, security App Service, secure cloud, secure Web App

Once complete you can view the results, Figure 4, and if there is anything found, you can take an action on them. I found none when I scanned my Web App.

![scan results for security scan of an App Service (Web App, Mobile App, API App and Logic App)][FIGURE4]
###### Figure 4, scan results for security scan of an App Service (Web App, Mobile App, API App and Logic App)

[FIGURE1]: ../images/2016/msdn-0613.png "Figure 1, check your App Service for vulnerabilities"
[FIGURE2]: ../images/2016/msdn-0614.png "Figure 2, scanning an App Service (Web App, Mobile App, API App and Logic App) for security"
[FIGURE3]: ../images/2016/msdn-0615.png "Figure 3, security App Service, secure cloud, secure Web App"
[FIGURE4]: ../images/2016/msdn-0616.png "Figure 4, scan results for security scan of an App Service (Web App, Mobile App, API App and Logic App)"

[LINK1]: https://azure.microsoft.com/en-gb/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/
[LINK2]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
