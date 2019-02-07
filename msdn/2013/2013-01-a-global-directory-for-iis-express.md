# A global directory for IIS Express

I know of 3 different Microsoft web server versions that can be used to host and test your web applications.  They are:

+ Internet Information Services (IIS)
+ Cassini Web Server
+ IIS Express

IIS is the full version with many features and can be enhanced and extended in many, many ways.  It can be installed on your client machine, but is mostly used for businesses or individuals to host their web based intranet or internet applications.

The Cassini Web Server is typically used from within Visual Studio and is what hosts your web sites and renders your web page when you select F5 or Ctrl+F5.

Lastly, there is IIS Express that has more capabilities than Cassini but less than IIS.  IIS Express is integrated with WebMatrix, although it can be utilized within Visual Studio too.

One of the main differences between the 3 web servers is the concept of a global directory.

For example in IIS, an example of a global directory is: (where **#_#_##### is the .NET version your application pool is configured to use**)

***C:\>InetPub\wwwroot\aspnet_client\system_web\#_#_#####***

There is a similar global directory for the Cassini Web Server, as shown below:

***C:\>Windows\Microsoft.NET\Framework\v#.#.#####\ASP.NETClientFiles***

The purpose of these directories are to store files that need to be retrieved from the application hosted on the web server.  Files like JavaScripts, ActiveX Controls, Cascading Style Sheets, Images and any other static content.

But where is the global directory for IIS Express.  Quick answer is that there is not one.  IIS Express is very much directory based.  That being said, the 2 solutions I have been able to work out are:

1. Add the group of files to your root path of your web site
2. Create a virtual directory in IIS Express’ applicationHost.config file for the location of your global files

The first one really isn’t a global solution because you would need to copy the files into the root directory of each website you create.  For example, if you create a web site in Visual Studio called WebSite1, then you would need to place your files into the following (default) directory:

***C:\Users\<UserId>\Documents\Visual Studio 2012\WebSites\WebSite1***

The second is to create a virtual directory that points to the files you need your application to access.  Again, this is web site specific, but you need only store the files in a single location on your computer.  To do this perform the following steps.

1. Copy your global files to a single directory, for this example:  C:\>Temp
2. Open the applicationHost.config file located in the C:\Users\<UserId>\Documents\IISExpress\config directory.
3. Add an additional application entry as shown in Figure 1 that points to the global files

![IIS Express global directory work around #2][FIGURE1]
###### Figure 1, IIS Express global directory work around #2

4. Save the applicationHost.config file

Please note that manually modifying the applicationHost.config file may cause problems if you make a typo.  These problems may be very difficult to recover from.  Either take a backup prior to making these manual changes or use AppCmd or PowerShell to make these changes.

***UPDATE APR-25-2013***

There is also another option for getting IIS Express to look at the ASP.NETClientFiles directory, or any directory for that matter.  There is a command called MKLINK which creates a symbolic link with the files in a given directory.

For example:  ***MKLINK /d virtual_path "%systemdrive%\directory\subdirectory"***

Where virtual_path is the beginning of the path to the location of your global files.  I.e. the directory directly below ASP.NETClientFiles.  **%systemdrive%\directory\subdirectory** is the physical location of the global files.  If you had a file called "GlobaStyle.css" stored in a physical directory called **"%systemdrive%\GlobalFiles"** and wanted to access it from within your code using **"MyCSSFiles/GlobalStyle.css"** you would execute the following MKLINK command:

***MKLINK /d MyCSSFiles "%systemdrive%\GlobalFiles"***

NOTE: When you make this symbolic link and delete the linked directories at a later time, it does not delete the symbolic link between the folders.  If you manually create these symbolic links, remove them again if you remove/delete that path.  You do not want unused symbolic links laying around.  Use ***RMDIR symbolic_link_name*** to remove the symbolic link, where symbolic_link_name is the name of the link, for example virtual_path used above.

[FIGURE1]: ../images/2013/msdn-0202.png "Figure 1, IIS Express global directory work around #2"

[LINK1]: ../2011/2011-08-create-an-iis-configuration-backup.md
[LINK2]: http://technet.microsoft.com/en-us/library/cc753194(v=ws.10).aspx
