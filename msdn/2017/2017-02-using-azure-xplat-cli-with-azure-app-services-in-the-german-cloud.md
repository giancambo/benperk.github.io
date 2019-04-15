# Using Azure Xplat-CLI with Azure App Services in the German Cloud

Although you can use CLI with the public cloud as well and you can use it to configure many of the Azure features, this article is a bit focused on the Azure German Cloud #AzureGermanCloud #GermanCloud and the Azure App Services #Azure #AppServices running within it.

You can ‘get started’ with Microsoft Azure Deutschland here.  You can also check out the feature available on the Europe tab located here.

The Azure Xplat-CLI is open source and is hosted on GitHub here. #AzureCLI

The steps required to configure an Azure App Service running in the German Cloud are:

Install Node.js package (for npm) and the Azure CLI
Login to the German Cloud using Azure CLI
Execute some Azure App Service commands
Let’s get started.

Install Node.js package (for npm) and the Azure CLI
To install the Azure Command-Line Interface (CLI) you must first install NPM.  This package manager is part of the Node.js package.  I discuss how I used this previously when debugging a #chatbot here, see from Figure 5.  Or you can directly download the Node.js package from here.  Once installed, the NPM #NPM package will be installed as well and you can use it to install the Azure Xplat CLI and then configure some Azure App Services with it.

There are some very good instructions on how to Install the Azure CLI here, so I will speed through this part.  Enter the following command to install the Azure CLI, also shown in Figure 1.

```npm install -g azure-cli```

![install the Azure CLI for Azure App Service on the Azure German Cloud][FIGURE1]
###### Figure 1, install the Azure CLI for Azure App Service on the Azure German Cloud

After it installed, I ran the following command to see which version I just installed, shown in Figure 2.

```azure --version```

![install the Azure CLI for Azure App Service on the Azure German Cloud][FIGURE2]
###### Figure 2, install the Azure CLI for Azure App Service on the Azure German Cloud

Now that it’s all installed, let’s login.

Login to the German Cloud using Azure CLI
To login to the Azure German Cloud, execute the following command, also shown in Figure 3.

```azure login -e AzureGermanCloud -u <yourGermanCloudUserId>```

![login using Azure CLI for Azure App Service on the Azure German Cloud][FIGURE3]
###### Figure 3, login using Azure CLI for Azure App Service on the Azure German Cloud

Now, let’s get to work.

## Execute some Azure App Service commands

There already exists some really good documentation for administrating Azure App Services with CLI here.  But let’s do a little anyway.

## List Existing App Service Plan

```azure appserviceplan list --resource-group <yourResourceGroupName>```

![List app service plans using Azure CLI for Azure App Service on the Azure German Cloud][FIGURE4]
###### Figure 4, List app service plans using Azure CLI for Azure App Service on the Azure German Cloud

## Scale up an App Service Plan

As you can see in Figure 4, the SKU is S1 which means it is a SMALL instance.  Let’s scale it to and S3, LARGE.

```azure appserviceplan set --name <ASPname> --resource-group <RGname> --sku <plan>```

![scale up an app service plan using Azure CLI for Azure App Service on the Azure German Cloud][FIGURE5]
###### Figure 5, scale up an app service plan using Azure CLI for Azure App Service on the Azure German Cloud

When I ran this one originally, I got an error “error: ‘config’ is not an azure command.  See ‘azure help’”.  It looks like the ‘config’ command has been replaced with ‘set’.  To see how I figured that out, check out this post here.

## Scale out an App Service Plan

As you can see in Figure 4, the ‘Number Of Workers’ is 1.  Let’s change this to 5.

```azure appserviceplan set --name <ASPname> --resource-group <RGname> --sku S3 --instances 5```

![scale out an app service plan using Azure CLI for Azure App Service on the Azure German Cloud][FIGURE6]
###### Figure 6, scale out an app service plan using Azure CLI for Azure App Service on the Azure German Cloud

Run another list command to confirm the actions where successful.

![List app service plans using Azure CLI for Azure App Service on the Azure German Cloud][FIGURE8]
###### Figure 7, List app service plans using Azure CLI for Azure App Service on the Azure German Cloud

The same can be confirmed by looking in the portal, accessible here.

![Confirm changes made Azure CLI for Azure App Service on the Azure German Cloud in the Azure Portal][FIGURE9]
###### Figure 8, Confirm changes made Azure CLI for Azure App Service on the Azure German Cloud in the Azure Portal


[FIGURE1]: ../images/2017/msdn-1146.png "Figure 1, install the Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE2]: ../images/2017/msdn-1147.png "Figure 2, install the Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE3]: ../images/2017/msdn-1148.png "Figure 3, login using Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE4]: ../images/2017/msdn-1149.png "Figure 4, List app service plans using Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE5]: ../images/2017/msdn-1150.png "Figure 5, scale out an app service plan using Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE6]: ../images/2017/msdn-1151.png "Figure 6, scale out an app service plan using Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE7]: ../images/2017/msdn-1152.png "Figure 7, List app service plans using Azure CLI for Azure App Service on the Azure German Cloud"
[FIGURE8]: ../images/2017/msdn-1153.png "Figure 8, Confirm changes made Azure CLI for Azure App Service on the Azure German Cloud in the Azure Portal"
