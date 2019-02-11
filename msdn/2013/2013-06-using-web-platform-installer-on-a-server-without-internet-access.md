# Using Web Platform Installer on a server without internet access

I was trying to setup a WebDAV installation that mapped to a UNC share and used Windows Authentication.  I had to create a test domain with numerous servers in order to grant all the required permissions to get this setup to work.  I had some problem with the initial step of this project because the servers in my test domain did not have access to the internet and therefore could not download and install the Web Platform Installer.  Even if I could get it installed, the Web Platform Installer would need to access the internet again to get the WebDAV components.  What I did to solve the problem was to build the package on a computer with internet access and then copy and execute that package on the server without internet access.  The following are the steps I followed to do this.

I used a tool called WebpiCmd.  You can download it and get more information about it from [here][LINK1] or [here][LINK2].

I ran the command shown in Figure 1, from a machine with internet access to build an installation package.

![Using Webpicmd to build an offline WebDav installation package][FIGURE1]
###### Figure 1, Using Webpicmd to build an offline WebDav installation package

Notice the words written in green, they state that the installation is complete and provide the syntax for installing the package on the web server that does not have internet access.  The execution of the package creation process resulted in the creation of 3 directories shown in Figure 2.

![results of the offline WebpiCmd package][FIGURE2]
###### Figure 2, results of the offline WebpiCmd package

I should also mention that the WebpiCmd offline package process also includes any of the WebDAV dependencies.  For example, WebDAV needs some FTP binaries to work and they are therefore part of the installation package.  You can open the shadow-webproductlist.xml file in the feeds directory to view which dependency components will be installed.

I then copied those 3 directories to the IIS server in my test domain which does not have internet access.  I executed the command shown in Figure 3 to install the package.

![Installing WebDAV on a server without internet access using WebpiCmd][FIGURE3]
###### Figure 3, Installing WebDAV on a server without internet access using WebpiCmd

Then, when I opened IIS and looked in the features view of the management console, I see that it was installed as expected.

On IIS 7.5+ you might consider installing WebDAV Role from Server Manager.  However, for out of band components which can only be installed using the Web Platform Installer, this is a good option.

Once I get the WebDAV setup using the UNC Share and Windows Authentication I will write something about that.

[FIGURE1]: ../images/2013/msdn-0240.png "Figure 1, Using Webpicmd to build an offline WebDav installation package"
[FIGURE2]: ../images/2013/msdn-0241.png "Figure 2, IIS"
[FIGURE3]: ../images/2013/msdn-0242.png "Figure 3, IIS"

[LINK1]: http://msdn.microsoft.com/en-us/library/windowsazure/gg433092.aspx
[LINK2]: http://www.iis.net/learn/install/web-platform-installer/web-platform-installer-v4-command-line-webpicmdexe-rtw-release
