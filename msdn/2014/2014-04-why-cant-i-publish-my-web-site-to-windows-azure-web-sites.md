# Why can’t I publish my web site to Microsoft Azure Web App

There are numerous ways to publish your web sites to a Microsoft Azure App Service Web App.  Click on one of your App Service Web App and on the Quick Start page you see a link called ‘Set up deployment from source control’, as shown in Figure 1.

![Set up deployment from source control in Microsoft Azure App Service Web App][FIGURE1]
###### Figure 1, Set up deployment from source control in Microsoft Azure App Service Web App

Clicking on that link will provide a list of current possible source control repositories, as shown in Figure 2.  You can also use a tool like FileZilla to publish your web site directly using FTP.

![Source code repository Microsoft Azure App Service Web App options][FIGURE2]
###### Figure 2, Source code repository Microsoft Azure App Service Web App options

The current deployment options are included in the following Tabel 1.

| Deployment Location | Description |
| ------------------- | ----------- |
| Visual Studio Online  | Visual Studio Online provides a complete solution for software management, source control, issue   tracking, build and test automation, and more.  |
| Local Git Repository  | Do you already have a Git repository on your local computer? Simply publish your local Git   repository to a remote repository in Windows Azure, an your site will go  live quickly and automatically.  |
| GitHub  | 
GitHub is the best place to share code with friends, co-workers, classmates, and complete strangers. Over three million people use GitHub to build amazing things together.  |
|  Dropbox | Dropbox is a free service that lets you bring all your photos, documents, and videos anywhere and share them easily. Use Windows Azure to quickly synchronize and deploy your code from your Dropbox folder.  |
| Bitbucket  | Bitbucket is a hosting site for the Git and Mercurial distributed version control systems. Bitbucket includes an issue tracker, a wiki, and integration with popular  services such as Basecamp, Flowdock, and Twitter.  |
| CodePlex  | 
CodePlex is Microsoft’s open source project hosting web site. You can create public projects for free, sharing your code with the world.  |
|  External Repository | 
Type the URL of any public external repository to automatically deploy from that location. Both Git and Mercurial repositories are supported.  |

###### Table 1, Microsoft Azure App Service Web App Deployment Options

There are many options as you can see each of which have their own level of complexity and troubleshooting tools and techniques.  If you encounter errors while publishing from any of these, here are a few tips on what I look at when working with customer on deployment issues.

## Using the Kudu Service

You can access some deployment details by entering https://***.scm.azurewebsites.net (where standard is the name of your Web App).  Using your Deployment credentials to login, you can click on the Deployments links, as shown in Figure 3, to download a JSON file containing your deployment logs.  If the issue is happening on the App Service Web App side, then the error is likely logged here which might lead you to a solution.  If there is no logged error, then the issue may be happening before the request reaches the Windows Azure platform.  In this case you want to focus you effort on the deployment platform.

![Viewing Microsoft Azure App Service Web App deployment logs using Kudu ][FIGURE3]
###### Figure 3, Viewing Microsoft Azure App Service Web App deployment logs using Kudu 

## Using WebDeploy to test connectivity and publishing

If the deployment option you chose is not working and you might want to try using WebDeploy just as a quick check to see if this method is working.  You will first need to download you publish profile from the DASHBOARD page, for the given web site within the Microsoft Azure Management console.  Click on the “Download the publish profile” link, as shown in Figure 4 and open the file.

NOTE: For this example, I installed Web Deploy 3.5 from the IIS Management Console using the Web Platform Installer.

![Download the Windows Azure Web Site publish profile ][FIGURE4]
###### Figure 4, Download the Windows Azure Web Site publish profile 

When the file is opened you will see something similar to that in Figure 5.  The ***publishUrl***, ***userName*** and ***userPWD*** are the attributes you need for the Web Deploy command.

![Web Deploy values from the Microsoft Azure App Service Web App publish profile ][FIGURE5]
###### Figure 5, Web Deploy values from the Microsoft Azure App Service Web App publish profile 

Enter the Web Deploy command, shown below and in Figure 6, from an IIS Server where the website is hosted.

```
C:\>msdeploy –verb:dump –source:iisapp=standard,computername=https://***publishUrl***:443/msdeploy.axd?site=standard,***username***=userName,***password***=userPWD,authtype=basic –alloweuntrusted –debug -verbose
```

![Web Deploy command for testing deployment functionality ][FIGURE6]
###### Figure 6, Web Deploy command for testing deployment functionality 

If the command returns without an error you can be confident that a deployment using Web Deploy is available and working.  With this information you can refocus your investigation on the deployment platform and not the Microsoft Azure platform. For more information about the Web Deploy dump verb, see [here][LINK1].

## Using an FTP application

Make sure you can make a connection to your Microsoft Azure App Service Web App using an FTP tool.  Navigate to the LogFiles directory as shown in Figure 6 and see if there are any deployment folders that contain some deployment logs.  Figure 7 is taken from FileZilla.

![Using FileZilla to view any deployment log files ][FIGURE7]
###### Figure 7, Using FileZilla to view any deployment log files 

By using these tips you might be able to find some errors that lead you research further.

UPDATE: Here is another nice tool you can use to deploy and test deployments, it is called [WSDeploy][LINK2].

[FIGURE1]: ../images/2014/msdn-0359.png "Figure 1, Set up deployment from source control in Microsoft Azure App Service Web App"
[FIGURE2]: ../images/2014/msdn-0360.png "Figure 2, Source code repository Microsoft Azure App Service Web App options"
[FIGURE3]: ../images/2014/msdn-0361.png "Figure 3, Viewing Microsoft Azure App Service Web App deployment logs using Kudu "
[FIGURE4]: ../images/2014/msdn-0362.png "Figure 4, Download the Windows Azure Web Site publish profile "
[FIGURE5]: ../images/2014/msdn-0363.png "Figure 5, Web Deploy values from the Microsoft Azure App Service Web App publish profile "
[FIGURE6]: ../images/2014/msdn-0364.png "Figure 6, Web Deploy command for testing deployment functionality "
[FIGURE7]: ../images/2014/msdn-0365.png "Figure 7, Using FileZilla to view any deployment log files "

[LINK1]: http://technet.microsoft.com/en-us/library/dd569101(v=WS.10).aspx
[LINK2]: http://blog.davidebbo.com/2014/03/WAWSDeploy.html
