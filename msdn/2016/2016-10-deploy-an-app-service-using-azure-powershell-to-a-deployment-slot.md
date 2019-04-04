# Deploy an App Service using Azure PowerShell to a Deployment Slot

I wrote an article [here][LINK1] that explains how to install Azure PowerShell and to check what version it is.

A trend that I am seeing is the we tend to create the capabilities to make modifications to an Azure feature and exposed the interface. Then we create a PowerShell cmdlet and then later it gets built into the management console. This makes some sense and there are a lot more complexities when it comes to placing a GUI around an API. I mention that because if you want to early adopt, you might get a few weeks or months jump on the competition by keeping up with the Azure PowerShell cmdlets…at least that is how it is at the moment.

The above paragraph really has nothing to do with the article, I just had that thought and wanted to share it.

After creating a Deployment Slot in the management portal, shown in Figure 1, yes, you can create a Deployment Slot from Azure PowerShell, but I am ramping up slowly, ‘cause I’m a GUI guy.

![create a deployment slot for an App Service][FIGURE1]
###### Figure 1, create a deployment slot for an App Service

Below, shown in Listing 1, is the sequence of cmdlets I used to deploy an App Service Web App to the deployment slot.  Note: the name of the Web App is STANDARD. you need to replace it with your own App Service name.

```
Get-AzurePublishSettingsFile
Import-AzurePublishSettingsFile .\Your-Publish-Settings-credentials.publishsettings
Get-AzureSubscription
Select-AzureSubscription -SubscriptionName "The Subscription Name containing the slot"
Set-AzureSubscription -SubscriptionId "ID of subscription"
#Remove-AzureAccount -WhatIf #if you cannot find the slot, remove and add accounts
$WebAppName = "standard(staging)"
Get-AzureWebsite -Name $WebAppName
Publish-AzureWebsiteProject -Name "standard" -Package "C:\PowerShell\standard.zip" -Slot "staging"
``` 

Once the script completes without error, the deployment is complete. If you do get errors, you might take a look at [this][LINK2] article that includes some tips on how to troubleshoot deployment errors. It is focused on GIT but the discussion about downloading the Diagnostic Dump and looking at the IIS logs can be helpful. Perhaps this article is helpful too.

[FIGURE1]: ../images/2016/msdn-0974.png "Figure 1, create a deployment slot for an App Service"

[LINK1]: 2016-04-how-to-tell-which-azure-powershell-version-you-are-running.md
[LINK2]: ../../waws/2015/2015-08-troubleshooting-web-app-deployment-issues-when-using-git-github.md
