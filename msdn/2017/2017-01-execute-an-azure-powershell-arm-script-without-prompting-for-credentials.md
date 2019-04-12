# Execute an Azure PowerShell (ARM) script without prompting for credentials

Something I have noticed and seems to be a path being set forward is that ASM capabilities are not going to be available in an Azure Sovereign clouds.  By ASM, I mean anything that is dependent on this URL manage.windowsazure.com.  Therefore, if you have any dependency on that and plan to move into an Azure Sovereign cloud, you will need to change what you are doing or how you are doing it.  I have written a few posts here about the German Cloud, which is a Sovereign cloud.

How (I) connected a console application to a Web API protected by an Azure Active Directory
How (I) configured Azure Active Directory into my ASP.NET MVC OWIN web application
Some other of my security related articles are here
Anyway, one of the more common ways to begin an Azure PowerShell session is to execute the Login-AzureRmAccount cmdlet.

```Login-AzureRmAccount```

However, when you do this you are prompted each time to enter your credentials, as shown in Figure 1.

![stop credential pop up, challenge response when accessing Azure PowerShell][FIGURE1]
###### Figure 1, stop credential pop up, challenge response when accessing Azure PowerShell

There are 2 ways I have seen to get logged into an Azure PowerShell session so that you can run a script without being prompted.

+ Temporary automatic login using a authentication Token
+ Using a certificate for longer term authentication
+ Creating a service principle with password

Before you can use either of those you must “Create a service principle with password”.  Review the following topics for more information on how to achieve this.

## Creating a service principle with password

Execute these cmdlets.

```
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId 25ec5bae**********
$app = New-AzureRmADApplication -DisplayName "PowerShellLogin" -HomePage "{URL}/PowerShellLogin" -IdentifierUris "{URL}/PowerShellLogin" -Password "{Pass}"
$app
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId
```

Once executed, you will see the application in the portal, like Figure 2.

![stop credential pop up, challenge response when accessing Azure PowerShell][FIGURE2]
###### Figure 2, stop credential pop up, challenge response when accessing Azure PowerShell

## Temporary automatic login using a authentication Token

In order to make this happen, you can execute these Azure PowerShell cmdlets.  The same and more can be reviewed here.

Keep 2 points in mind.  1. The credentials you enter are those you created using these instructions here.  2. The token will expire (usually after 2 hours, but it can expire for numerous other reasons faster).  Keep in mind that the token must be refreshed once it expires and the profile saved into the JSON must be updated.

```
$creds = Get-Credential
$tenant = (Get-AzureRmSubscription).TenantId
$tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
Save-AzureRmProfile -Path c:\Temp\token.json
Select-AzureRmProfile -Path c:\Temp\token.json

## Using a certificate for longer term authentication

Check out section “Provide certificate through automated PowerShell script” here for some additional information.

To create a self-signed certificate, I wrote how to do this here, here and there is also this tool here.

Here are the cmdlets required to login with a certificate.

```
$tenant = (Get-AzureRmSubscription).TenantId
$tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant
```

## List of helpful links

+ Create service principle with portal
+ Create service principle with PowerShell – Create service principle with password
+ Create service principle with PowerShell – provide credentials through PowerShell
+ Using Azure PowerShell to create a service principle to access resources
+ Use portal to create Active Directory application and service principle that can access resources

[FIGURE1]: ../images/2017/msdn-1107.png "Figure 1, stop credential pop up, challenge response when accessing Azure PowerShell"
[FIGURE2]: ../images/2017/msdn-1108.png "Figure 2, Feature"
