# Why does my Azure App Service (Web, Mobile, API, Logic App) consume so much storage space

I worked on a case where the customer stated that their Web App was pretty small but when looking at the File System Storage metric in the Azure portal, they saw that is consumed almost 30 GB.

The way I recommended they check it out is to use the KUDU console.  I wrote an article about KUDU [here][LINK1], if you want to learn how to access it. 

## Using a PowerShell Script

Log into KUDU and click on Debug console -> PowerShell, as shown in Figure 1.

![the PowerShell console in KUDU][FIGURE1]
###### Figure 1, the PowerShell console in KUDU

Create a PS1 file that contains the code shown in Listing 1 and upload it to the D:\home directory.

###### Listing 1, Get-Size.ps1 example

```
function Get-Size
{  param([string]$pth)
  "Directory " + [string]$pth + " consumes " + "{0:n2}" -f ((gci -path $pth -recurse | measure-object -property length -sum).sum /1mb) + " mb" }
Get-Size data
Get-Size LogFiles
Get-Size site 
```

Once the PS1 is uploaded, exec the command as shown in Figure 2.

![the PowerShell console in KUDU to show App Service (Web App, Mobile App, Logic App, API App) space/size consumption][FIGURE2]
###### Figure 2, the PowerShell console in KUDU to show App Service (Web App, Mobile App, Logic App, API App) space/size consumption

## Using a the DIR command

Log into KUDU and click on Debug console -> CMD, as shown in Figure 1 previously.

Then navigate to the D:\home\sites or maybe d:\home\LogFiles directory and execute this command, as shown in Figure 3.

*** dir /s > directory.txt ***

![list the contents of a directory][FIGURE3]
###### Figure 3, list the contents of a directory

Executing this command will dump the contents and the file size of the SITES directory and sub directories into a file named directory.txt.  Once executed, as seen fing Figure 4, a file is created and placed into the directory.

![contents of your Azure Web App][FIGURE4]
###### Figure 4, contents of your Azure Web App

Click on the pen or edit link and you can view the contents to see if there are any large files or a number of ‚larger‘ files.  You should see something similar to the illustration in Figure 5.

![contents of you Web App, what is consuming space on my Web App?][FIGURE5]
###### Figure 5, contents of you Web App, what is consuming space on my Web App?

### Using the Azure Resource Explorer

There is also a cool tool [here][LINK2] called Azure Resources.  Within the Azure Resources tool, navigate to the usages resource of the Web App and view the Bytes unit as described in the Embedded Video 1.  Something similar to this:

/subscriptions/{subscriptionId}/resourceGroups/{rg}/providers/Microsoft.Web/sites/{site}/usages

-----------------

The 10GB "limit" for Basic is bound to the App Service Plan (ASP) and the "currentValue" is for the Web App.

## Using the DU tool command

You can use the du.exe sysinternals tool to output the content site of each directory or sub-directory into a log file, as shown in Figure 6.  The command is:

*** d:\home\du -mh > d:\home\LogFiles\logName.txt ***

![using du to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)][FIGURE6]
###### Figure 6, using du to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)

## Using the Azure Web Apps Disk Usage Site Extension

There is also an extension which you can install via KUDU, as shown in Figure 7 that will show the consumption as well.

![using the Azure Web App Disk Usage extension to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)][FIGURE7]
###### Figure 7, using the Azure Web App Disk Usage extension to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)

After you install the extension, you will need to restart the App Service.  The output is friendly, as shown in Figure 8.

![output of the Azure Web App Disk Usage extension to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)][FIGURE8]
###### Figure 8, output of the Azure Web App Disk Usage extension to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)

You can also try out these PowerShell cmdlets:

```
Get-AzureRmResource -ResourceGroupName YourResourceGroupName -ResourceType Microsoft.Web/sites/usages -ResourceName YourAppServiceName -ApiVersion 2016-03-01
Get-AzureRmResource -ResourceGroupName YourResourceGroupName -ResourceType Microsoft.Web/serverfarms/usages -ResourceName YourAppServiceName -ApiVersion 2016-03-01
```

[FIGURE1]: ../images/2016/msdn-0586.png "Figure 1, the PowerShell console in KUDU"
[FIGURE2]: ../images/2016/msdn-0587.png "Figure 2, the PowerShell console in KUDU to show App Service (Web App, Mobile App, Logic App, API App) space/size consumption"
[FIGURE3]: ../images/2016/msdn-0588.png "Figure 3, list the contents of a directory"
[FIGURE4]: ../images/2016/msdn-0589.png "Figure 4, contents of your Azure Web App"
[FIGURE5]: ../images/2016/msdn-0590.png "Figure 5, contents of you Web App, what is consuming space on my Web App?"
[FIGURE6]: ../images/2016/msdn-0591.png "Figure 6, using du to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)"
[FIGURE7]: ../images/2016/msdn-0591.png "Figure 7, using the Azure Web App Disk Usage extension to find content size of your Azure App Service (Web App, Mobile App, API App, Logic App)"
[FIGURE8]: ../images/2016/msdn-0593.png "Figure 8, Feature"

[LINK1]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK2]: https://resources.azure.com/
