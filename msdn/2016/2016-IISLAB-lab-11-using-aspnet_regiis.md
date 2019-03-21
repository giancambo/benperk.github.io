# Lab 11: Using aspnet_regiis

## General information

+ The description of the aspnet_regiis tool can be found [here][LINK1]

## Lab 11-1 Setup

+ 2 IIS servers are needed for this lab, install IIS as per these instructions [Lab 1][LINK2], but the CSharpGuitarBUgs web site is not required
+ Place a copy of each of the c:\windows\system32\inetsrv\config\applicationHost.config files in a temporary location, mark them so you know which server they came from
+ Copy c:\windows\system32\inetsrv\config\applicationHost.config from (IIS Server 1) and to the same location (IIS Server 2)
+ ASP.NET should initially be installed on both (IIS Server 1) and (IIS Server 2) server, ASP.NET 4.5 was used in this lab

![Using aspnet_regiis][FIGURE1]
###### Figure 1, Using aspnet_regiis

## Lab 11-1 – Error when changing application pool identity

Issue when an applicationHost.config is copied to another IIS server

+ It is usually better to use ShareConfiguration than to copy the configuration file
+ It is also possible to use WebDeploy to synchronize web sites in a web-farm

1. Click on Application Pools in the IIS Management Console

2. Click on DefaultAppPool and select the Advanced Settings… link from the Actions pane

3. Change the Identity to a custom account and press the OK button, the above window is rendered. NOTE: in production, never use an administrator account as the identity of a worker process, for simplicity only is this done here.

![Using aspnet_regiis][FIGURE2]
###### Figure 2, Using aspnet_regiis

When attempting to change the identity of an application pool to a custom identity the following error is rendered: “Value does not fall within the expected range.”

![Using aspnet_regiis][FIGURE3]
###### Figure 3, Using aspnet_regiis

4. Q: In which context does the application run in? I.e. which bit mode and .NET version does it run in. You need to know this to find which version of the aspnet_regiis to execute… A: by default on an IIS 8.5 server the worker is running in 64 bit mode and .NET Framework 4 is present.

![Using aspnet_regiis][FIGURE4]
###### Figure 4, Using aspnet_regiis

5. From the location where the applicationHost.config file came from (IIS server 1) and execute the following commands to export the sessionKeys:  NOTE:  ASP.NET must be installed on this server.

a. ```aspnet_regiis -px "iisConfigurationKey" "c:\temp\iisConfigurationKey.xml" -pri```

b. ```aspnet_regiis -px "iisWasKey" "c:\temp\iisWasKey.xml" –pri```

![Using aspnet_regiis][FIGURE5]
###### Figure 5, Using aspnet_regiis

![Using aspnet_regiis][FIGURE6]
###### Figure 6, Using aspnet_regiis

6. Copy the applicationHost.config and the 2 XML files created in step 5a and 5b from (IIS server 1) to (IIS Server 2)

7. Place the (IIS server 1) applicationHost.config (same as you did in the setup of this Lab) into the c:\windows\system32\inetsrv\config directory and reproduce the error

8. Import the sessionKeys to server 2 by executing the following commands:

a. ```aspnet_regiis -pi "iisConfigurationKey" "D:\iisConfigurationKey.xml"```

b. ```aspnet_regiis -pi "iisWasKey" "D:\iisWasKey.xml"```

![Using aspnet_regiis][FIGURE7]
###### Figure 7, Using aspnet_regiis

9. The issue no longer happens and you can enter a custom identity for the application pool

## Lab 11-2 Setup

+ Replace the current c:\windows\system32\inetsrv\config\applicationHost.config with the original on (IIS Server 2)
+ Place a deafult.aspx file into the c:\intetpub\wwwroot directory

## Lab 11-2

1. Open IIS manager and navigate to the default.aspx file using a browser

![Using aspnet_regiis][FIGURE8]
###### Figure 8, Using aspnet_regiis

2. Open the Handler Mappings feature and find the handler for the requested file type, it is missing

![Using aspnet_regiis][FIGURE9]
###### Figure 9, Using aspnet_regiis

3. Q: What is the requested file type? A: ASPX

4. Find out which context the application pool is running in, I.e. bit mode and .NET version, then navigate to that directory via a command prompt, for example, c:\windows\Microsoft.NET\Framework64\v4.0.30319

5. Prior to IIS 8 you could use aspnet_regiis –i to reinstall/reset ASP.NET as you can see from the above image that the handlers are not present.  Use WPI as discussed in [Lab 2][LINK3], but it is always more important to take a backup of your configuration as discussed in Lab 9 and here, which is a lot easier and les risky than a complete reinstall.

![Using aspnet_regiis][FIGURE10]
###### Figure 10, Using aspnet_regiis

6.  Once installed, reopen the Handler mapping feature and you will find the ASPX handler.  Refresh the default.aspx page and it renders as expected

## Lab 11-3 Setup

Create a web.config file that includes a <connectionString> like below and place it into the c:\inetpub\wwwroot directory

```
<?xml version="1.0"?>
 <configuration>
   <connectionStrings>
    <add name="TopSecretConnectionString"
         connectionString="Initial Catalog=aspnetdb;data source=localhost;Integrated Security=SSPI"
         providerName="System.Data.SqlClient" />
   </connectionStrings>
 </configuration>
```

## Lab 11-3

1. View the contents of the web.config file, pay special attention to the ConnectionStrings section.

![Using aspnet_regiis][FIGURE11]
###### Figure 11, Using aspnet_regiis

2. Confirm the .NET version and bit-ness in which the application pool is running under and navigate to the correct version of the aspnt_regiis.

![Using aspnet_regiis][FIGURE12]
###### Figure 12, Using aspnet_regiis

3. Execute the following command: aspnet_regiis –pef “connectionStrings” C:\inetpub\wwwroot –prov “DataProtectionConfigurationProvider”

![Using aspnet_regiis][FIGURE13]
###### Figure 13, Using aspnet_regiis

4. Open the c:\inetpub\wwwroot\web.config file and you will see that the content is encrypted.

![Using aspnet_regiis][FIGURE14]
###### Figure1 4, Using aspnet_regiis

5. No code changes required to encrypt the connection string

[FIGURE1]: ../images/2016/msdn-0797.png "Figure 1, Using aspnet_regiis"
[FIGURE2]: ../images/2016/msdn-0798.png "Figure 2, Using aspnet_regiis"
[FIGURE3]: ../images/2016/msdn-0799.png "Figure 3, Using aspnet_regiis"
[FIGURE4]: ../images/2016/msdn-0800.png "Figure 4, Using aspnet_regiis"
[FIGURE5]: ../images/2016/msdn-0801.png "Figure 5, Using aspnet_regiis"
[FIGURE6]: ../images/2016/msdn-0802.png "Figure 6, Using aspnet_regiis"
[FIGURE7]: ../images/2016/msdn-0803.png "Figure 7, Using aspnet_regiis"
[FIGURE8]: ../images/2016/msdn-0804.png "Figure 8, Using aspnet_regiis"
[FIGURE9]: ../images/2016/msdn-0805.png "Figure 9, Using aspnet_regiis"
[FIGURE10]: ../images/2016/msdn-0806.png "Figure 10, Using aspnet_regiis"
[FIGURE11]: ../images/2016/msdn-0807.png "Figure 11, Using aspnet_regiis"
[FIGURE12]: ../images/2016/msdn-0808.png "Figure 12, Using aspnet_regiis"
[FIGURE13]: ../images/2016/msdn-0809.png "Figure 13, Using aspnet_regiis"
[FIGURE14]: ../images/2016/msdn-0810.png "Figure 14, Using aspnet_regiis"

[LINK1]: http://msdn.microsoft.com/en-us/library/k6h9cz8h(v=vs.80).aspx
[LINK2]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK3]: 2016-IISLAB-lab-2-install-the-web-platform-installer.md
