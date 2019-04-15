# Create a VNET and access an Azure VM hosted within it from an App Services Web App

There are 2 kinds of VNETs at the moment, they are VNETv1 (ASM), also referred to as ‘Classic virtual networks’ and VNETv2 (ARM), aka ‘Resource Manager virtual networks’.  The binding between a VNETv1 and a Web App can be achieved within the AUX portal here, while the same needs to be performed using PowerShell for VNETv2, shown later.

In this article I discuss how I created a Point-to-Site connection between a Azure App Services Web App and an Azure VM that is within a VNET.  The Point-2-Site scenario is where you want your Web App to access a database or web API hosted on a Azure VM running within a VNET.  As you know, when you create a Web App that endpoint is accessible on the internet, the same is true when you create a SQL Azure database.  However, if you do not want your Web API or database endpoint exposed you can place them into a VNET and make them less accessible and perhaps more secure.  I say ‘perhaps’ because I am not a security expert and security is a big deal, ask a security expert for confirmation.

The steps I took to create the VNET and the connection to a VM within it were:

+ Create a Resource Group
+ Create an App Services Web App
+ Configure the Networking from the Web App
+ Create an Azure VM within the VNET
+ Deploy an ASP.NET Core Web API to the Azure VM within the VNET
+ Create the Point-to-Site (P2S) connection
+ Test it Out and review

## Create a Resource Group

What I have learned from my numerous years working with Azure resources is that I need to give some thought in advance around how I group them together.  What I do these days is group all the resources that provide a specific solution into a single resource group.  Resources within a resource group do not have to be in the same region, a Resource Group is only a logical grouping of Azure or 3rd party marketplace features.  Create a new Resource Group by clicking on the + NEW link at the top of the Azure management portal here and select Resource Group, as shown in Figure 1.

![create a resource group in Azure][FIGURE1]
###### Figure 1, create a resource group in Azure

Now Add a Web App into the Resource Group.

## Create an App Services Web App

Open the Resource Group and click on the +Add link as shown in Figure 2.

![create an Azure App Services Web App][FIGURE2]
###### Figure 2, create an Azure App Services Web App

Next, select the Web App from the blade which renders, then the Create button.  Give the Web App a name, select the subscription, the Resource Group you created in the previous step and create a new App Service Plan with with a plan of S1 or greater.  VNET requires Standard or Premium plans.  See Figure 3 as an example.

![create an Azure App Service Web App and an App Service Plan][FIGURE3]
###### Figure 3, create an Azure App Service Web App and an App Service Plan

Once the deployment is successful, configure the Networking and the VNET.

## Configure the Networking from the Web App

Open the Web App created in the last section and select the Networking feature which opens the blade show in Figure 4.

![configure the networking for an Azure App Services Web App][FIGURE4]
###### Figure 4, configure the networking for an Azure App Services Web App

Click on the Setup link to begin the configuration and the creation of the new Virtual Network.  As illustrated in Figure 5, enter the name of your VNET and click the OK button.

![create the VNET for an Azure App Services Web App][FIGURE5]
###### Figure 5, create the VNET for an Azure App Services Web App

For this example I have simply accepted the defaults address blocks and names.  If you need different for your solution, make the appropriate changes.  Note to self:  learn more about this and provide some examples of why these would need to be changed.  After clicking on the OK button it may take some time to perform all the steps required to create the VNET like the gateway, public IP address and the VNET, so check back by looking in the notifications window, as shown, Figure 6.

![taking time to create a VNET in Azure][FIGURE6]
###### Figure 6, taking time to create a VNET in Azure

Once completed, check out the configuration and then, let’s add an Azure VM to the VNET.

## Create an Azure VM within the VNET

The newly created VNET is empty, so let’s add an Azure VM into it.  Similar to that shown in Figure 2, from the Resource Group blade select + Add and add the Azure VM.  I won’t go into much detail here because creating a VM is pretty straight forward.  I chose to install an Azure VM running Windows Server 2016 Datacenter.  The important point is that you select the VNET you created in the previous section for the Network into which the VM will be added.  Similar to that shown in Figure 7.

![Create an Azure VM and add it to a VNET for P2S or S2S connectivity from an Azure App Service Web App][FIGURE7]
###### Figure 7, Create an Azure VM and add it to a VNET for P2S or S2S connectivity from an Azure App Service Web App

This also can take some minutes, so let it complete and return once completed.

## Deploy an ASP.NET Core Web API to the Azure VM within the VNET

I wrote another post here that explains how to create and deploy the ASP.NET Core Web API to the Azure VM.  You can also download the code here.

The code deployed to the Azure App Services Web App which you created in the “Create an App Services Web App” section previously, also refer to Figure 2 and Figure 3 is the consumer of the ASP.NET Core Web API hosted on the Azure VM which sits inside the VNET (VPN-AM2-VNET).  The code to consume the ASP.NET Core Web API is shown below.

```
protected void Page_Load(object sender, EventArgs e)
{
   try
   {
      System.Net.WebRequest request = System.Net.WebRequest.Create("http://***/api/Sleepy");
      System.Net.WebResponse response = request.GetResponse();
      Stream dataStream = response.GetResponseStream();
      StreamReader reader = new StreamReader(dataStream);
      labelAPI.Text = reader.ReadToEnd() + " -> this came from the Web API.";
      reader.Close();
      response.Close();
   }
   catch (Exception ex)
   {
      labelAPI.Text = ex.Message;
   }
}
```

## Create the Point-to-Site (P2S) connection

My VNET is version 2 and therefore I need to create the P2S connection using a PowerShell command.  I wrote an article here that shows the PowerShell commands required to get logged in to Azure from the PowerShell IDE and how to set the context to the correct Azure Subscription.  It is also important to have the most current version of the Microsoft Azure PowerShell cmdlets, I explain how to determine that here.  First I logged in.

```Login-AzureRmAccount -SubscriptionId "********-****-****-****-************"```

After logging in successfully, if the output received shows a Subscription other than the one containing the VNET, you will need to change the context.  I did this by executing the following PowerShell cmdlets.

```
Get-AzurePublishSettingsFile
Import-AzurePublishSettingsFile "C:\Temp\***??***.publishsettings"
Set-AzureSubscription -SubscriptionId "*****-****-****-****-******"
Get-AzureSubscription
```

Then execute these 2 PowerShell commands to create the Point-to-Site (P2S) connection between the Azure App Services Web App and the VNET.

```
$gw = Get-AzureRmVirtualNetworkGateway -Name "VPN-AM2-VNET-gateway" -ResourceGroupName "VPN-AM2-RG"
Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gw -VpnClientAddressPool "192.168.0.0/16"
```

Replace the –Name with your Gateway and the –ResourceGroupName with your Resource Group name.  As Shown in Figure 8, the execution of the command complete without any problem.  NOTE to self –> why use 192.168.0.0/16?  Why not 172.16.0.0/16 or 10.0.0.0/16?  I think the 10.0.0.0/16 configuration would mess up or conflict with the subnet, but can’t say the same in regards to the 172.16.** range…need to do more learnings in this area, but in the end I have successfully created the VNET and Web App that consumes an ASP.NET Core Web API running on an Azure VM within the VNET.

![create a Point-to-Site (P2S) between an Azure App Services Web App and an Azure VNET][FIGURE8]
###### Figure 8, create a Point-to-Site (P2S) between an Azure App Services Web App and an Azure VNET

I then took a look at the Virtual Network Integration configuration as shown in Figure 9.  I see that the POINT-TO-SITE contains the address space which was created via the PowerShell command.

![Azure App Services Web App consumes an ASP.NET Core Web API hosted on an Azure VM within a VNET][FIGURE9]
###### Figure 9, Azure App Services Web App consumes an ASP.NET Core Web API hosted on an Azure VM within a VNET

## Test it Out

That was actually much easier than expected.  I think my lack of Networking experience made me a little apprehensive and expected to encounter some problems which I had no clear path to resolve.  That didn’t happen and all is well.

Simply, when I accessed the Web App I got the response from the Azure VM as expected.  However, how can I tell and be for sure that it really did it?  And also, let’s look at each of the components of the VNET and see what kind of information is in each one to get a more clear understanding of what we just did.

+ Look at the Azure VM to confirm the request from the Web App was received
+ Look at the Network Interface
+ Look at the Public IP Address
+ Look at the Network Security Group
+ Look at the Virtual Network
+ Look at the Virtual Network Gateway

Basically, I want to look at all the contents in my Resource Group which I specifically created for this VNET, as shown in Figure 10.

![all the parts of a VNET, an Azure VM and a Web App, so cool][FIGURE10]
###### Figure 10, all the parts of a VNET, an Azure VM and a Web App, so cool

## Look at the Azure VM

The information I found useful when looking at the Azure VM in the portal were the Diagram, Figure 11 and the Network Interfaces.

![An Azure VM in a VNET with a P2S from an Azure App Services Web App][FIGURE11]
###### Figure 11, An Azure VM in a VNET with a P2S from an Azure App Services Web App

I also wanted to Remote Desktop connect to the Azure VM and check the IIS logs to confirm that a request came from my Web App to the Azure VM at the time I performed the test.  Indeed it did happen, as shown in Figure 12.

![successful P2S request from a Web App to an Azure VM in a VNET][FIGURE12]
###### Figure 12, successful P2S request from a Web App to an Azure VM in a VNET

Interesting in Figure 12 that the request seems to take 6.9 seconds, I will need to find out why it is so slow Smile .  However, I won’t troubleshoot it on the Azure VM, I will move it to a Web App as we can have more fun troubleshooting the performance issue there.

## Look at the Public IP Address

There are 2 of these in the Resource Group, one is the Public IP Address of the Azure VM which you can see already in Figure 11 and in Figure 13.

![an Azure VM IP address][FIGURE13]
###### Figure 13, an Azure VM IP address

And the other Public IP Address of the VNET.

![an Azure VNET IP address][FIGURE14]
###### Figure 14, an Azure VNET IP address

Nothing really jumps out at me here, I would just like to know more about why they are called Public, most likely because if I wanted to setup a Site-to-Site or a Hybrid connection I would probably need these.  They are not ping-able.

## Look at the Network Security Group

I hear a lot about Network Security Group rules, especially in regards to App Service Environments.  Usually around the configuration of Inbound and Outbound rules. The initial NSG blade looks like that shown in Figure 15.

![an Azure VNET network security group][FIGURE15]
###### Figure 15, an Azure VNET network security group

You can add many inbound and outbound connection rules from within the portal, see Figure 16.

![configure inbound or outbound network security group (NSG) within an Azure VNET][FIGURE16]
###### Figure 16, configure inbound or outbound network security group (NSG) within an Azure VNET

I selected HTTP from the Service drop-down list, but there are many numbers of protocols/services for which you can create a rule, for example:  FTP, SSH, SMTP, DNS(TCP), DNS(UDP), POP3, MS SQL, My SQL, Redis, and about 10-20 others.  This is cool stuff.

## Look at the Virtual Network

The good information in this one is on the Diagram blade where I can see my Azure VM and the GatewaySubnet.  If you recall from way earlier where I showed the code for the consumer of the ASP.NET Core Web API where I had left the HTTP URL of the request blocked out, well, here is where I found what to add for that value.

```System.Net.WebRequest request = System.Net.WebRequest.Create("http://10.1.0.4/api/Sleepy");```

The IP address of the Azure VM shown in Figure 17 is that value.  I am certain you can give the Azure VM a name and then use the name instead of the IP, but this out of scope for now.

![the diagram of an Azure Virtual Network][FIGURE17]
###### Figure 17, the diagram of an Azure Virtual Network

## Look at the VNET Gateway

The Gateway acts like, well, a gateway into and out of the VNET.  It is kind of like the box you have in your house that connects all your devices to the internet provider.  The interesting pieces of information I see, Figure 18, are the Point-to-site configuration blade where I can see the AppServiceCertificate.cer certificate that was created when I executed the PowerShell cmdlets previously in Figure 8.  As well, I see the Allocated IP addresses.

![an Azure VNET gateway to be the gateway][FIGURE18]
###### Figure 18, an Azure VNET gateway to be the gateway

That’s it, thanks for reading and I hope you found this useful.

Check out some of my other articles I wrote in regards to ASP.NET Core and .Net Core

+ How to call an async method from a console app main method
+ How to deploy a .NET Core console application to Azure, WebJob
+ .NET Core application, where is my EXE, how to publish
+ Create a VNET and access an Azure VM hosted within it from an App Services Web App
+ Create and deploy an ASP.NET Core Web API to Azure Windows

[FIGURE1]: ../images/2017/msdn-1117.png "Figure 1, create a resource group in Azure"
[FIGURE2]: ../images/2017/msdn-1118.png "Figure 2, create an Azure App Services Web App"
[FIGURE3]: ../images/2017/msdn-1119.png "Figure 3, create an Azure App Service Web App and an App Service Plan"
[FIGURE4]: ../images/2017/msdn-1120.png "Figure 4, configure the networking for an Azure App Services Web App"
[FIGURE5]: ../images/2017/msdn-1121.png "Figure 5, create the VNET for an Azure App Services Web App"
[FIGURE6]: ../images/2017/msdn-1122.png "Figure 6, taking time to create a VNET in Azure"
[FIGURE7]: ../images/2017/msdn-1123.png "Figure 7, Create an Azure VM and add it to a VNET for P2S or S2S connectivity from an Azure App Service Web App"
[FIGURE8]: ../images/2017/msdn-1124.png "Figure 8, create a Point-to-Site (P2S) between an Azure App Services Web App and an Azure VNET"
[FIGURE9]: ../images/2017/msdn-1125.png "Figure 9, Azure App Services Web App consumes an ASP.NET Core Web API hosted on an Azure VM within a VNET"
[FIGURE10]: ../images/2017/msdn-1126.png "Figure 10, all the parts of a VNET, an Azure VM and a Web App, so cool"
[FIGURE11]: ../images/2017/msdn-1127.png "Figure 11, An Azure VM in a VNET with a P2S from an Azure App Services Web App"
[FIGURE12]: ../images/2017/msdn-1128.png "Figure 12, successful P2S request from a Web App to an Azure VM in a VNET"
[FIGURE13]: ../images/2017/msdn-1129.png "Figure 13, an Azure VM IP address"
[FIGURE14]: ../images/2017/msdn-1130.png "Figure 14, an Azure VNET IP address"
[FIGURE15]: ../images/2017/msdn-1131.png "Figure 15, an Azure VNET network security group"
[FIGURE16]: ../images/2017/msdn-1132.png "Figure 16, configure inbound or outbound network security group (NSG) within an Azure VNET"
[FIGURE17]: ../images/2017/msdn-1133.png "Figure 17, the diagram of an Azure Virtual Network"
[FIGURE18]: ../images/2017/msdn-1134.png "Figure 18, an Azure VNET gateway to be the gateway"
