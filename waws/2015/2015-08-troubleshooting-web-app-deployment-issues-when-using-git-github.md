# Troubleshooting Web App deployment issues when using Git / GitHub

I setup a Web App to test deployments from GitHub.  Once the Web App was created I linked it to GitHub by selecting the “Integrate source control” link on the main page of for the website and selected GitHub.  Following the wizard I was able to make the link between the 2.  The details are shown in Figure 1.

![linking GitHub to a Web App][FIGURE1]
###### Figure 1, linking GitHub to a Web App

After making some changes to the MASTER branch via the online GitHub, the files are deployed to the linked Web App after comitting the changes.  Additionally, after synching the files to my local repository via the GitHub Windows client application, I get a summary of the changes as shown in Figure 2.

![GitHub client status summary][FIGURE2]
###### Figure 2, GitHub client status summary

There are some useful logs in the Azure portal for checking the status of the deployment as well, as shown in Figure 3.

![checking a Web App deployment status via the portal][FIGURE3]
###### Figure 3, checking a Web App deployment status via the portal

However, what happens if you experience some errors during the deployment and you cannot find any issues or error messages within the portal or on the deployment repository side?  The remainder of this article describes some ideas on how to proceed with your research and hopefully resolve the deployment issue.  The options include:

+ Review the IIS logs for errors, confirm your deployment is making a connection
+ Make sure everything works outside of KUDU and Web Apps
+ Diagnostic dump via KUDU and review the deployment logs
+ How big is your deployment?
+ Deploy using the shared Publisher Role instead of the dedicated W3WP SCM process
+ View the EVENTLOG.xml file for exceptions
+ Kill the SCM process (possible hung file)
+ Real-time log stream analysis with CURL

A recommendation I can make quickly, is to increase your SCM_TRACE_LEVEL so that you get more details about the deployment as described [here][LINK1].  As always, once you complete your troubleshooting, lower it back down so less data is logged, based on your requirements.  

## Review the IIS logs

After making sure I had enabled IIS logging for my website that uses GIT, as shown in Figure 4, where Web server logging is On, I can download and analyze the logs using, for example LogParser.  I wrote another article which explains how to use LogParser [here][LINK2].

![how to enable IIS logs on Web Apps][FIGURE4]
###### Figure 4, how to enable IIS logs on Web Apps

As shown later in Figure 9, once enabled, the Web App IIS logs are written to the LogFiles\http\RawLogs directory.  An example of a LogParser query is shown in Figure 5.

![IIS Web App GIT deployment logs][FIGURE5]
###### Figure 5, IIS Web App GIT deployment logs

What you would look for is, #1, do you see any POST or GET requests being sent to your Web App?  If not, then your deployment is failing before it even gets to the Web App platform.  You should check your network connection and specifically check if you have a proxy or firewall which is blocking the connection for some reason.  #2, check the sc-status just make sure that you are getting a 200, if not, identify what the status code and sub-status code is and take appropriate actions.

## Does everything work outside of KUDU and Web Apps

The point of looking at the IIS logs, discussed previously, is to confirm that your deployment is making it to the Azure platform.  Here are some links to other helpful instructions for confirming things are working as they should be outside of KUDU and Web Apps.

+ [Making sure things work outside of Kudu and Web Apps][LINK3]
+ [Make sure all your files are committed][LINK4]
+ [Make sure site correctly deploys locally][LINK5]
+ [Check if the MSDEPLOY, discussed here works][LINK6]

If everything checks out, then continue reading the article for more tips and suggestions on how to troubleshoot and analyze further.

## Create an Web App Diagnostic dump 

I wrote a brief article about [KUDU/SCM][LINK7] here, explaining a little about SCM / KUDU.  To download the Diagnostic dump which will include many log files for the site, including the deployment logs, click the Diagnostic dump menu item, within KUDU, as shown in Figure 6.

![capture a Web App Diagnostic dump][FIGURE6]
###### Figure 6, capture a Web App Diagnostic dump

Once the process is completed, you are prompted to save or open the ZIP file.  The contents of the diagnostic dump are shown via Figure 7.  The deployments directory contains folders with a deployment GUID as their name, for example 789aece1f3ac34df3e7448d499b8f2a54aa45e29, each deployment will receive its own folder with a different deployment GUID.  Within the deployment GUID folder, there are 2 XML files, one is the STATUS.xml file which contains information about the deployment like author, who deployed it, the start and end date, basically an overview of the deployment.  Of course the status of the deployment exists as well.

![Web App deployment logs][FIGURE7]
###### Figure 7, Web App deployment logs

The other XML file in the deployment GUID folder is the LOG.xml file as shown in Figure 8.  Notice that the deployment GUID is the same, circled in blue in many locations, see Figure 3, Figure 7 and Figure 8, all circled in blue.  The LOG.xml provides more details about the deployment and may include errors or exceptions that lead to a solution. 

![a successful deployment log file][FIGURE8]
###### Figure 8, a successful deployment log file

Additionally, you’ll find the deployment log, as shown in Figure 9 using FTP.

![get the deployment log via FTP][FIGURE9]
###### Figure 9, get the deployment log via FTP

Another place to check for exceptions and errors, if you can’t find any log of your deployment in the \deployments directory, is in the LogFiles\kudu\trace directory as shown previously, circled in red in Figure 7 and also seen in Figure 9 via FTP.
I recommend that you perform a search for keywords like ‘error’ or ‘exception’ then review each of the files that contain these words.  For example, I found this error in a trace file, shown in Figure 10.  It was very cool indeed to actually find a recommended solution with the logged exception.

![Web App GIT deployment error][FIGURE10]
###### Figure 10, Web App GIT deployment error

Performing a BING search for SCM_COMMAND_IDLE_TIMEOUT, lead me to this [page][LINK8] that describes the configurable repository and deployment settings.  If this happens to you, you might consider raising the timeout value over the 60 second default limit.
To set these values, access the Azure portal and make the change to the Application settings for the given git linked Web App, as shown in Figure 11a (new portal) and 11b (current portal).  Additionally, you can add a .deployment file to your Git repository and make the changes there.  This is discussed at the end of the articles in the ‘HOW TO create a .deployment file’ section.

![How to change verbose logging for Web App deployments in the new preview portal][FIGURE11]
###### Figure 11a, How to change verbose logging for Web App deployments in the new preview portal

![How to change verbose logging for Web App deployments in the current portal][FIGURE12]
###### Figure 11b, How to change verbose logging for Web App deployments in the current portal

## How big is your deployment?

Another valid focus point, if you get time outs or failures is the size of your deployment.  Is the deployment 4 or 5 files with a total size of 20KB or is the deployment 4GB with hundreds or thousands of files?  If you have a very large deployment and are getting some issues, consider increasing the http.postBuffer to a larger size, greater than the default size of 1MB.  [Here][LINK9] is a nice description of the configuration you can make to a GIT configuration.  Additionally, the Web App forums is a great resource to search for other customer experiences, [here][LINK10] is where I found the tip about increasing the http.postBuffer.  Other newer or existing posts may help you find a solution. 

# Deploy using the shared Publisher Role

When you make a deployment to your Web App the process that accepts and executes the deployment request is the dedicated W3WP process running your SCM website.  Review this article [here][LINK11], and you can see that the WEBSITE_WEBDEPLOY_USE_SCM app setting is set to TRUE by default.  When set to TRUE, your deployment is managed through the SCM process.  If you set the WEBSITE_WEBDEPLOY_USE_SCM to FALSE, then the shared Web App Publisher Role performs your deployment as shown [here][LINK12] (expand “Overview of Web Sites Roles”).

You can give this a try, but it is recommended to use the dedicated process versus the shared one, however, in a serious situations where you need the code deployed fast, this might be an option for getting it done.

##View the EVENTLOG.xml file for exceptions

The EVENTLOG.XML which I discuss [here][LINK13], is a very good source of information when exceptions occur during a deployment or within your Web App in general.  As seen in Figure 12, the data is in an XML format which you can search through in, for example Visual Studio, or parse it using LogPaser as previously mentioned.

![viewing the RAW EVENTLOG.XML data of a Web App event][FIGURE13]
###### Figure 12, viewing the RAW EVENTLOG.XML data of a Web App event

You can also access the contents of the EVENTLOG.XML file via the Event Viewer support feature via your KUDU console, as shown in Figure 13.

![viewing event viewer logs for your Web App via KUDU][FIGURE14]
###### Figure 13, viewing event viewer logs for your Web App via KUDU

## Kill the SCM process

There may be a situation where a file you are trying to update is currently being used by a process running on your VM.  This may cause your deployment to hang and then possibly fail.  You can check this by selecting the Handle tab within the properties of the processes shown via the Process Explorer, shown in Figure 14.  If you find a file you are attempting to modify in the list, then you may want to kill the W3WP SCM or W3WP process.  Or, consider killing them both regardless, if there is minimal impact to your users, just to make sure you didn’t miss anything.  

![kill, kill, kill the process that is stopping my Web App deployment][FIGURE15]
###### Figure 14, kill, kill, kill the process that is stopping my Web App deployment

The processes will be restarted again once you make a request to them.

## Real time log stream analysis with CURL

Performing the deployment, then capturing and analyzing the logs is a valid way to find the reason for the deployment failure.  Especially so when you need to send them to someone else for analysis.  However, you can also setup a tool called CURL on your client machine and watch the logs being written as the deployment takes place.  Perform the following to view the Diagnostic Log Stream as discussed in some detail [here][LINK14].

1. Download and unzip CURL from [here][LINK15].
2. Place the CURL executable and the .crt file from the /bin directory into a directory on your client
3. Enter ‘curl - u USERNAME https://??*??.scm.azurewebsites.net/logstream -k’
4. Perform a deployment and watch the logs

#### Download, unzip and place CURL onto your client machine 

As far as I read, the official CURL comes via the source code only, which means you can compile it to get your own version of the executable.  The URL I provide above is to download the EXE.  The link is a wizard that walks you through some questions so you get the correct version and bit size for your needs.

Next, I opened the ZIP file and found the curl.exe and curl-ca-bundle.crt in the \bin directory.  I placed them into a directory which existed in my PATH environment variable.  This makes it easier to run as I can enter the command into a command prompt and the system finds it without me needing to navigate to the directory in which it exists.

#### Enter CURL command to start the Diagnostic Log Stream monitoring

Enter the command -> “***curl -u USERNAME URL -k***”  where:

+ USERNAME - your Web App Deployment Credential, it is not the publishing credential nor your live id used to access the Azure Management Portal.  It is, for example, the USERNAME you use to access your Web App via FTP
+ URL - is the URL to your SCM / KUDU site for the Web App to which you are deploying

Figure 15 represents how this might look.

![connecting SCM / KUDU via CURL for live log streaming using Diagnostic Log Stream][FIGURE16]
###### Figure 15, connecting SCM / KUDU via CURL for live log streaming using Diagnostic Log Stream

After you enter the command, you are prompted for your password and then welcomed into the log-streaming service.

#### Perform a deployment

I then make a modification to a file within my local GitHub repository and commit my changes as shown in Figure 16.

![ommit changes to repository for publication to a Web App][FIGURE17]
Create###### Figure 16, ommit changes to repository for publication to a Web App

Then, synchronize the local GitHub repository with the Web App, as shown in Figure 17.

![synchronize your local GitHub repository with your Web App][FIGURE18]
###### Figure 17, synchronize your local GitHub repository with your Web App

Shortly after you select the Sync button, you can watch the output of the log stream and check for any issue or exception.  Figure 18 and Figure 19 illustrate a successful deployment.  Notice that in Figure 18 that the before and after deployment GUIDs are displayed.  This GUID is the same you would find when accessing via FTP, as seen previously in Figure 9.  Additionally, you can check which files have been added, removed and modified.

![checking the log stream for deployments to a Web App via a code repository][FIGURE19]
###### Figure 18, checking the log stream for deployments to a Web App via a code repository

Continue watching the log stream until the end of the deployment, looking for terms such as ‘Deployment successful’ or ‘statusCode: 200, statusText: OK’, similar to that here.

![ a successful deployment to a Web App using a source code repository and CURL][FIGURE20]
###### Figure 19,  a successful deployment to a Web App using a source code repository and CURL

If all goes well, you are done, however, if you see some exceptions or reports of ‘error’, ‘failure’, ‘statusCode’ not equal to 200, then at a minimum, you have something to begin your troubleshooting efforts and/or to provide to a support contact.

## HOW TO create a .deployment file

While writing this article and making a suggestion to set the SCM_TRACE_LEVEL=4, I had some difficulty finding out how to create the file and where to put it.  Here is how I did it.  At the top of the page [here][LINK16], which I have linked to many times in this article there is a link describing the .deployment file and links to [here][LINK17].  It mentions that the .deployment file needs to be in the root of the repository.  Looking at my repository, I did not have that file, as shown in Figure 20.

![it-deploy GitHub content][FIGURE21]
###### Figure 20, it-deploy GitHub content

I then places a file named .deployment into my local GitHub repository.  I created the TXT file, but had to RENAME the file via a command prompt as shown in Figure 21.

![rename .deployment.txt to .deployment][FIGURE22]
###### Figure 21, rename .deployment.txt to .deployment

When I refresh my online GitHub, I then see the file as expected.  I can make the changes to the .deployment file in my local repository or on the online portal.  Once the changes are committed, they are applied to any future deployment.


[FIGURE1]: ../images/2015/waws-0064.png "Figure 1, linking GitHub to a Web App"
[FIGURE2]: ../images/2015/waws-0065.png "Figure 2, GitHub client status summary"
[FIGURE3]: ../images/2015/waws-0066.png "Figure 3, checking a Web App deployment status via the portal"
[FIGURE4]: ../images/2015/waws-0067.png "Figure 4, how to enable IIS logs on Web Apps"
[FIGURE5]: ../images/2015/waws-0068.png "Figure 5, IIS Web App GIT deployment logs"
[FIGURE6]: ../images/2015/waws-0069.png "Figure 6, capture a Web App Diagnostic dump"
[FIGURE7]: ../images/2015/waws-0070.png "Figure 7, Web App deployment logs"
[FIGURE8]: ../images/2015/waws-0071.png "Figure 8, a successful deployment log file"
[FIGURE9]: ../images/2015/waws-0072.png "Figure 9, get the deployment log via FTP"
[FIGURE10]: ../images/2015/waws-0073.png "Figure 10, Web App GIT deployment error"
[FIGURE11]: ../images/2015/waws-0074.png "Figure 11a, How to change verbose logging for Web App deployments in the new preview portal"
[FIGURE12]: ../images/2015/waws-0075.png "Figure 11b, How to change verbose logging for Web App deployments in the current portal"
[FIGURE13]: ../images/2015/waws-0076.png "Figure 12, viewing the RAW EVENTLOG.XML data of a Web App event"
[FIGURE14]: ../images/2015/waws-0077.png "Figure 13, viewing event viewer logs for your Web App via KUDU"
[FIGURE15]: ../images/2015/waws-0078.png "Figure 14, kill, kill, kill the process that is stopping my Web App deployment"
[FIGURE16]: ../images/2015/waws-0079.png "Figure 15, connecting SCM / KUDU via CURL for live log streaming using Diagnostic Log Stream"
[FIGURE17]: ../images/2015/waws-0080.png "Figure 16, ommit changes to repository for publication to a Web App"
[FIGURE18]: ../images/2015/waws-0081.png "Figure 17, synchronize your local GitHub repository with your Web App"
[FIGURE19]: ../images/2015/waws-0082.png "Figure 18, checking the log stream for deployments to a Web App via a code repository"
[FIGURE20]: ../images/2015/waws-0083.png "Figure 19,  a successful deployment to a Web App using a source code repository and CURL"
[FIGURE21]: ../images/2015/waws-0084.png "Figure 20, it-deploy GitHub content"
[FIGURE22]: ../images/2015/waws-0085.png "Figure 21,  publish my .deployment file"

[LINK1]: https://github.com/projectkudu/kudu/wiki/Configurable-settings
[LINK2]: http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser
[LINK3]: https://github.com/projectkudu/kudu/wiki/Investigating-issues
[LINK4]: https://github.com/projectkudu/kudu/wiki/Make-sure-all-your-files-are-committed
[LINK5]: https://github.com/projectkudu/kudu/wiki/Make-sure-site-correctly-deploys-locally
[LINK6]: http://blogs.msdn.com/b/benjaminperkins/archive/2014/04/01/why-can-t-i-publish-my-web-site-to-windows-azure-web-sites.aspx
[LINK7]: ../../msdn/2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK8]: https://github.com/projectkudu/kudu/wiki/Configurable-settings
[LINK9]: http://git-scm.com/docs/git-config
[LINK10]: https://social.msdn.microsoft.com/Forums/azure/en-US/fbede8b2-5a06-46c8-8f39-af269a1ace07/problems-pushing-large-changesets-with-git-to-windows-azure?forum=windowsazurewebsitespreview
[LINK11]: http://azure.microsoft.com/blog/2014/06/30/web-deploy-as-a-site-extension/
[LINK12]: https://technet.microsoft.com/en-us/library/dn457747.aspx
[LINK13]: http://blogs.msdn.com/b/benjaminperkins/archive/2014/09/01/using-logparser-to-analyze-the-eventlog-xml-azure-websites.aspx
[LINK14]: https://github.com/projectkudu/kudu/wiki/Diagnostic-Log-Stream
[LINK15]: http://curl.haxx.se/dlwiz/
[LINK16]: https://github.com/projectkudu/kudu/wiki/Configurable-settings
[LINK17]: https://github.com/projectkudu/kudu/wiki/Customizing-deployments
