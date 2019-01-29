# How to configure GitHub or VSTS for use with an Azure Function

I have written a few post about setting up source code repositories for App Services and now, while I am getting up to speed with Azure Functions I wanted to do the same.  Here are some other GitHub related articles I have written: #AzureFunctions #Azure #GitHub

+ [Setting up and using GitHub in Visual Studio 2017][LINK1]
+ [Setting up and using GitHub in Visual Studio 2015][LINK2]
+ [Deploy GitHub source code repositories to an Azure App Service][LINK3]
+ [How to deploy from GitHub using someone else’s repository][LINK4]
+ [How to configure GitHub or VSTS for use with an Azure Function][LINK5]
+ [Failed to deploy from GitHub to Azure App Service Web App, missing project reference][LINK6]
+ [GitHub articles by Benjamin][LINK7]

And here are some Azure Function articles which might also be helpful to have a read through:

+ [How to add assembly references to an Azure Function App][LINK8]
+ [Testing/Consuming an Azure Function][LINK9]
+ [Visual Studio Tool for Azure Functions][LINK10]
+ [Debugging Azure Functions in Visual Studio][LINK11]

The ability to code and test locally instead of via the Azure Portal is supported now by the Visual Studio Tool for Azure Functions, so have a read of that one and the Test/Consuming an Azure Function as well.

See also [Continuous deployment for Azure Functions][LINK12].

Setting up the deployment from a source code repository is very similar to doing it for an App Service or an App Service Environment.  The steps are:

1. Create the Azure Function and the Git repository
2. Sync the local Git with GitHub
3. Configure the Deployment Options for the Azure Function

## Create the Azure Function and the Git repository

As seen in Figure 1, create the Azure Function.

![create an Azure Function in with Visual Studio][FIGURE1]
###### Figure 1, create an Azure Function in with Visual Studio

## Sync the local Git with GitHub

Then on the Team Explorer tab submit and synchronize the changes from the Local Git to GitHub, similar to that in Figure 2.

![sync local Git with GitHub an Azure Function in with Visual Studio][FIGURE2]
###### Figure 2, sync local Git with GitHub an Azure Function in with Visual Studio

Then check your GitHub repository to confirm it was published, as seen in Figure 3.

![GitHub repository for an Azure Function created with Visual Studio][FIGURE3]
###### Figure 3, GitHub repository for an Azure Function created with Visual Studio

## Configure the Deployment Options for the Azure Function

Then access the Azure Function via the Azure Portal and configure the Deployment options by clicking the Deployment option link, Figure 4.

![Configure an Azure Function deployment link ][FIGURE4]
###### Figure 4, Configure an Azure Function deployment link 

Select the GitHub repository source, or any of the others which are supported, as seen in Figure 5.

![choose a deployment source for an Azure Function, Visual Studio Team Services (VSTS), OneDrive, Local Git Repository, GitHub, Bitbucket, Dropbox][FIGURE5]
###### Figure 5, choose a deployment source for an Azure Function, Visual Studio Team Services (VSTS), OneDrive, Local Git Repository, GitHub, Bitbucket, Dropbox

Then choose the project which you synced to GitHub via the Visual Studio Team Explorer, as seen in Figure 6.

![Configure an Azure Function deployment link, source, project and branch][FIGURE6]
###### Figure 6, Configure an Azure Function deployment link, source, project and branch

Once you select the OK button, the code will be pulled from the GitHub and places onto the Azure Function.

![deployments][FIGURE7]
###### Figure 6a, Configure an Azure Function deployment link, source, project and branch

Finally, in Figure 7, you will see the Azure Function in the console as expected.

![GitHub repository for an Azure Function created with Visual Studio][FIGURE8]
###### Figure 7, GitHub repository for an Azure Function created with Visual Studio


[FIGURE1]: ../images/2017/msdn-0100.png "Figure 1, create an Azure Function in with Visual Studio"
[FIGURE2]: ../images/2017/msdn-0101.png "Figure 2, sync local Git with GitHub an Azure Function in with Visual Studio"
[FIGURE3]: ../images/2017/msdn-0102.png "Figure 3, GitHub repository for an Azure Function created with Visual Studio"
[FIGURE4]: ../images/2017/msdn-0103.png "Figure 4, Configure an Azure Function deployment link"
[FIGURE5]: ../images/2017/msdn-0104.png "Figure 5, choose a deployment source for an Azure Function, Visual Studio Team Services (VSTS), OneDrive, Local Git Repository, GitHub, Bitbucket, Dropbox"
[FIGURE6]: ../images/2017/msdn-0105.png "Figure 6, Configure an Azure Function deployment link, source, project and branch"
[FIGURE7]: ../images/2017/msdn-0106.png "Figure 6a, deployments"
[FIGURE8]: ../images/2017/msdn-0107.png "Figure 7, GitHub repository for an Azure Function created with Visual Studio"

[LINK1]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/04/04/setting-up-and-using-github-in-visual-studio-2017/
[LINK2]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/04/01/setting-up-and-using-github-in-visual-studio-2015/
[LINK3]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/05/10/deploy-github-source-code-repositories-to-an-azure-app-service/
[LINK4]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/05/10/how-to-deploy-from-github-using-someone-elses-repository/
[LINK5]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/05/10/how-to-configure-github-or-vsts-for-use-with-an-azure-function/
[LINK6]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/05/10/failed-to-deploy-from-github-to-azure-app-service-web-app-missing-project-reference/
[LINK7]: https://blogs.msdn.microsoft.com/benjaminperkins/tag/github/
[LINK8]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/04/13/how-to-add-assembly-references-to-an-azure-function-app/
[LINK9]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/04/06/testingconsuming-an-azure-function/
[LINK10]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/02/01/visual-studio-tool-for-azure-functions/
[LINK11]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/01/19/debugging-azure-funtions-in-visual-studio/
[LINK12]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-continuous-deployment
