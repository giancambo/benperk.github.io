# Using Cloud Explorer with a Sovereign Cloud

I was reading this article where it discusses some of the different kinds of Clouds that Azure supports.  For example:

+ Public Cloud
+ China Cloud
+ German Cloud
+ US Government Cloud

There may be more, but I was working on setting up my Cloud Explorer in Visual Studio 2015 with a subscription I have in the German Cloud and wasn’t able to make that work ‘out of the box’.  #GermanCloud #AzureGermanCloud This is how I made it work.

First install Cloud Explorer via the Extensions and Updates menu option inside the Tools menu, something similar to Figure 1.

![install Cloud Explorer for German Cloud][FIGURE1]
###### Figure 1, install Cloud Explorer for German Cloud

Once installed open the Cloud Explorer panel from the View menu item and click on the ?Accounts? icon, as seen in Figure 2.

![Add a German Cloud account to the Cloud Explorer][FIGURE2]
###### Figure 2, Add a German Cloud account to the Cloud Explorer

If you click on the ‘Add an account…’ link at the moment it will not work.  You need to configure some Registry settings before it will work.  NOTE:  making changes to the registry is dangerous, back it up before you make changes and even if you have a back up you can mess stuff up.  You need to add a the following to the registry for the computer you want to add the German Cloud account into.

Windows Registry Editor Version 5.00

```
[HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
"AadInstance"="https://login.microsoftonline.de"
"adaluri"="https://management.core.cloudapi.de"
"AzureRMEndpoint"="https://management.microsoftazure.de/"
"AzureRMAudienceEndpoint"="https://management.microsoftazure.de/"
"EnableAzureRMIdentity"="true"
"GraphUrl"="graph.cloudapi.de"
```

By default, at least this is how it was on my PC, there are no settings in that location, as seen in Figure 3.

![registry settings for the German Cloud][FIGURE3]
###### Figure 3, registry settings for the German Cloud

Once the above registry settings are added (just cut and paste it into NOTEPAD and save it as *.reg) or add them manually, you will see the following, Figure 4.

![Registry settings for the German Cloud and Cloud Explorer][FIGURE4]
###### Figure 4, Registry settings for the German Cloud and Cloud Explorer

NOTE: I had to remove current, pre-existing accounts, shown in Figure 2 in order to add my German Cloud subscription.

Restart Visual Studio 2015 and add the account, like shown in Figure 5.

![German Cloud account in Visual Studio Cloud Explorer][FIGURE5]
###### Figure 5, German Cloud account in Visual Studio Cloud Explorer

Then I can use many of the features available from the Cloud Explorer.

If you need to get back to your Public Cloud accounts, you can update the registry with the following and restart Visual Studio 2015.  You will need to re-add the accounts.  You might consider using 2 PCs, once for Public and one for Sovereign…at least this is how it is now.

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
"AadInstance"=-
"adaluri"=-
"AzureRMEndpoint"=-
"AzureRMAudienceEndpoint"=-
"EnableAzureRMIdentity"=-
"GraphUrl"=-
```

I am sure improvements are being made, this is just in case you experience this before then.

Die deutsche Cloud, Visual Studio, Subscription im Visual Studio bekanntmachen,  den Cloud-/Serverexplorer ein Publish-Profil laden.

[FIGURE1]: ../images/2017/msdn-1100.png "Figure 1, install Cloud Explorer for German Cloud"
[FIGURE2]: ../images/2017/msdn-1101.png "Figure 2, Add a German Cloud account to the Cloud Explorer"
[FIGURE3]: ../images/2017/msdn-1102.png "Figure 3, registry settings for the German Cloud"
[FIGURE4]: ../images/2017/msdn-1103.png "Figure 4, Registry settings for the German Cloud and Cloud Explorer"
[FIGURE5]: ../images/2017/msdn-1104.png "Figure 5, German Cloud account in Visual Studio Cloud Explorer"
