# Azure Management Certificates (German Cloud, Azure Deutschland)

This article has to do mostly with the German Cloud, #GermanCloud #AzureGermany because it is most probable that there will not be anything similar to manage.windowsazure.com deployed to this solution.  Why is that important?  It is important because this is historically how many people configured thier PowerShell or other clients to make deployments/configurations onto Azure.  For example, this is how it used to be done (still works, but it's 'classic'):

```
Get-AzurePublishSettingsFile
Import-AzurePublishSettingsFile .\Your-Publish-Settings-credentials.publishsettings
Get-AzureSubscription
Select-AzureSubscription -SubscriptionName “The Subscription Name containing the slot”
Set-AzureSubscription -SubscriptionId “ID of subscription”
```

Where the Get-AzurePublishSettingsFile cmdlet opens a browser session pointing to https://manage.windowsazure.com/publishsettings/index?client=powershell .  Unfortunately, using the cmdlet with the –Environment parameter like here:

```Get-AzurePublishSettingsFile -Environment AzureGermanCloud```

Attempts to open a web address that doesn’t and likely will never exist (https://manage.microsoftazure.de/publishsettings/index).

Here are some reasons to change your method away from this approach:

1. ASM, or features which can only be created and administered from the old portal (manage.windowsazure.com) which are not accessible in the new portal (portal.azure.com) cannot be used in the German Cloud until they are migrated to ARM or supported in the new portal.
2. At some point in the future, the AUX portal will be depricated and you will have to migrate to this new authenitcation model,so you might as well do it now

# Solution

As seen in Figure 1, when you want to get a Management Certificate you are presented with some information, the specific comment is:  “Management certificates are only applicable to the classic service management APIs, whereas service principles are required for the new Resource Manager APIs”

![Management Certificate for German Cloud][FIGURE1]
###### Figure 1, Management Certificate for German Cloud

Clicking on the link “Learn more about using service principles with Resource Manager” is where you can begin the process of learning and implementing the new authentication model.

Below are the links which you are currently redirected to from the Management Certificate blade in the portal.

+ PowerShell
+ Azure CLI
+ Portal

To set this up, I did the following from the Azure Management Portal:

+ Checked the Active Directory permissions
+ Checked the Subscription permissions
+ Captured the Tenant ID

I did the following from PowerShell:

+ Created the Active Directory application
+ Captured the Application ID (the user id)
+ Created the Active Directory application password
+ Created the Authentication Key
+ Grant the Active Directory application access to the Subscription

The PowerShell cmdlets where:

```
$app = New-AzureRmADApplication -DisplayName "*" -HomePage "*" -IdentifierUris "*" -Password "*"
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
Start-Sleep 15
New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId
```

You can then use the Application Id (userName) and the Application Password with either PowerShell or the Azure CLI to perform permitted configurations to the resources within the Subscription.  I used the PowerShell documented here, starting at section ”Provide Credentials through PowerShell”, to access the German Cloud I added the –EnvironmentName parameter.

```
$creds = Get-Credential
$tenant = (Get-AzureRmSubscription).TenantId
Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant -EnvironmentName AzureGermanCloud
```

Then, anyone with those credentials can access and perform actions allowed by the permissions.

## Closing thought

What I am thinking is that the above is what I would do if I was an administrator and wanted to give others the ability to make changes or view features within a subscription.  If I already have my own credentials that were created at the time the Subscription was registered, then I could just simply use them.

I can login to a PowerShell session automatically using this:

```
$UserName = "uid@tenantName.onmicrosoft.de"
$Password = ConvertTo-SecureString '{password}' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential($UserName, $Password)
Login-AzureRmAccount -Credential $creds -EnvironmentName AzureGermanCloud
```

You can do the same with the either your individual login or the Application Id login used when running the Get-Credential cmdlet.  You can also Save and Select the access tokens to/from a JSON file.  See section “Save access token to simplify log in” here.

I also found here, this comment:  "In some cases, you want to run code or script that accesses resources, but you do not want to run it under a user’s credentials. Instead, you want to create an identity called a service principal for the application and assign the appropriate role for the service principal. Resource Manager enables you to create credentials for the application and programmatically authenticate the application. "  This seems to confirm my thought.

```
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

[FIGURE1]: ../images/2017/msdn-1162.png "Figure 1, Management Certificate for German Cloud"
