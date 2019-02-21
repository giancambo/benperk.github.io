# Configure Remote IIS Administration for IIS Web Sites

It becomes unexciting when I need to access multiple Internet Information Services (IIS) servers to check configurations and such. Sure, you can use PowerShell, but I am a GUI guy and prefer the nice icons and text boxes over command line. I get a new GUI when a new version of the OS is released and new icons and stuff when new features are installed and/or released, and that adds excitement to my job. The Remote IIS Administration role is something I like to use to administer IIS servers remotely instead of logging into each, one at a time. It is accessible from within the IIS management console by selecting the File menu item and then the item you would like to manage, I.e. a Server, Site or Application, as shown in Figure 1.

![Adding a Server, Site or Application for remote IIS management][FIGURE1]
###### Figure 1, Adding a Server, Site or Application for remote IIS management

Not so fast, before you can add them you need to configure the remote management role.  [Here][LINK1] are some instructions, but, like always I like to walk through them and document my learnings and experiences.

The first step is to add the Management Service Role as shown in Figure 2.

![add the Management Service role for Remote Administration][FIGURE2]
###### Figure 2, add the Management Service role for Remote Administration

Once installed, you need to start the Web Management Service.  It is set to Manual, as shown in Figure 3.  Change it to Automatic if you plan on using this feature often.

![Start the Web Management Service service to enable Remote Management][FIGURE3]
###### Figure 3, Start the Web Management Service service to enable Remote Management

Next, set the EnableRemoteManagement attribute in the Registry to 1, as shown in Figure 4.  Please be careful making changes to the registry, some changes can cause serious negative impact.  Consider taking a backup of it prior to making any changes, JIC.

![Set EnableRemoteManagement to 1][FIGURE4]
###### Figure 4, Set EnableRemoteManagement to 1

Once the Management Services role is installed, you will notice the Management Service icon within the IIS management console at the server level as shown in Figure 5.

![the Management Service role in the IIS management console][FIGURE5]
###### Figure 5, the Management Service role in the IIS management console

Open the role and take note of the PORT configuration as it is needed when you make the connection from another IIS management console, as shown in Figure 6.

![the Management Service role in IIS][FIGURE6]
###### Figure 6, the Management Service role in IIS

Next, login to the server where you want to remote manage your IIS servers from.  Open the IIS management console and click File -> Connect to a Server, as shown previously in Figure 1.  Enter the name of the server as show in Figure 7 and click the Next button.

![Connect to a Server from IIS management console for remote management][FIGURE7]
###### Figure 7, Connect to a Server from IIS management console for remote management

Add the credentials for connecting to the remote IIS server, as shown in Figure 8 and select the Next button.

![enter credentials for IIS remote management][FIGURE8]
###### Figure 8, enter credentials for IIS remote management

You might get a warning that mentions ‘This certificate was issued to a different server.”.  You can either ignore the message and select the Connect button, as shown in Figure 9, or you can configure the certificate to be used to make the connection via the instructions provided previously.

![Certificate alert when configuring IIS Remote Management][FIGURE9]
###### Figure 9, Certificate alert when configuring IIS Remote Management

After clicking on the Connect button, give the connection a name and press the Finish button as shown in Figure 10.

![naming your remote IIS server connection][FIGURE10]
###### Figure 10, naming your remote IIS server connection

If you are prompted to install some new features, as shown in Figure 11, go ahead and do that.

![installing new features to enable IIS Remote Management][FIGURE11]
###### Figure 11, installing new features to enable IIS Remote Management

Once complete you can manage the configured server remotely without having to login, as illustrated in Figure 12.

![An IIS server which is remotely manageable][FIGURE12]
###### Figure 12, An IIS server which is remotely manageable

I like this feature and I hope you find it useful.

It is also possible to map a Microsoft Azure Web Site using the same feature.  Instructions are available [here][LINK2].

[FIGURE1]: ../images/2014/msdn-0413.png "Figure 1, Adding a Server, Site or Application for remote IIS management"
[FIGURE2]: ../images/2014/msdn-0414.png "Figure 2, add the Management Service role for Remote Administration"
[FIGURE3]: ../images/2014/msdn-0415.png "Figure 3, Start the Web Management Service service to enable Remote Management"
[FIGURE4]: ../images/2014/msdn-0416.png "Figure 4, Set EnableRemoteManagement to 1"
[FIGURE5]: ../images/2014/msdn-0417.png "Figure 5, the Management Service role in the IIS management console"
[FIGURE6]: ../images/2014/msdn-0418.png "Figure 6, the Management Service role in IIS"
[FIGURE7]: ../images/2014/msdn-0419.png "Figure 7, Connect to a Server from IIS management console for remote management"
[FIGURE8]: ../images/2014/msdn-0420.png "Figure 8, enter credentials for IIS remote management"
[FIGURE9]: ../images/2014/msdn-0421.png "Figure 9, Certificate alert when configuring IIS Remote Management"
[FIGURE10]: ../images/2014/msdn-0422.png "Figure 10, naming your remote IIS server connection"
[FIGURE11]: ../images/2014/msdn-0423.png "Figure 11, installing new features to enable IIS Remote Management"
[FIGURE12]: ../images/2014/msdn-0424.png "Figure 12, An IIS server which is remotely manageable"

[LINK1]: http://www.iis.net/learn/manage/remote-administration/remote-administration-for-iis-manager
[LINK2]: 2014-08-configure-remote-iis-administration-for-microsoft-azure-web-sites.md
