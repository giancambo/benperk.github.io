# Source Code plug in for WordPress blogs using Open Live Writer

Read the comment section.

I like to write.  I use WordPress at the moment for my blogs, I think as the blogs are stored in a database instead of being static text when I want to move to another platform or technologiy, I will only need to export the data and format it to match the new platform requirements.  Also, I can change templates and see how my blog looks.  So I can chage how my blog looks almost as often as I would like.  I also host my WordPress blog on Azure, and there are many options, see Figure 1.

![wordpress on Azure, so easy][FIGURE1]
###### Figure 1, wordpress on Azure, so easy

I also use Open Live Writer which you can get from here.  This makes it so easy to write and then post a blog.

As I find myself working on many computers, I need also to configure the Open Live Writer to format my source code example, I can always find the plug-in, but sometimes it give me some troubles during the configuration.  I am going to document it here for future reference.  You can download and see the already provided configuration instructions here.

Here are the actions I take to configure this plug-in.

+ Download the Version 2.0.0 BETA (recommended) –> SyntaxHighlight_WordPressCom_OLWPlugIn_V2.0.0.zip
+ Make sure I have at least .NET 4.5.2 installed.  Using Web Platform Installer to find out which version of .NET you are running is a good approach.  I mention this in a number of my posts here, here, here and here. As shown in Figure 2, .NET 4.5.2 is not installed.  Therefore it needs to be installed.  YOu might consider installing the most current version of Visual Studio as this would usually include the most current version of the .NET Framework.
+ Create a directory named Plugins here:  C:\Users\<username>\AppData\Local\OpenLiveWriter\app-*.*.*.
+ Place all the contents of the downloaded ZIP file into the Plugins directory.
+ Restart Open Live Wrtier and check if the plugin is present, see Figure 3, it is not there
+ Use Process Monitor to see what is going on….  What I find is that:
  + There is a log being written, see Figure 4.
  + The process is looking into the Plugins dirctory I created in step 4 and it finds it
+ Look into the log file and see if there are any tips, BINGO…
+ The link in the exception mentions that I need to add an entry into the configuration file:
  + The configuration file is located here:  C:\Users\<username>\AppData\Local\OpenLiveWriter\app-0.6.0.0
  + It’s called OpenLiveWriter.exe.config
+ Add/enable the loadFromRemoteSource attribute in the file
+ Restart the editor, and you should see the Plugin, Figure 6

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <runtime>
    <loadFromRemoteSources enabled="true" />
  </runtime>
</configuration>
```

```
OpenLiveWriter,1.8332,None,00008,19-Jan-2017 13:52:10.007,"System.IO.FileLoadException: Could not load file or assembly 
'file:///C:\Users\benperk\AppData\Local\OpenLiveWriter\app-0.6.0.0\Plugins\SyntaxHighlight_WordPressCom_OLWPlugIn.dll' or one of its 
dependencies. Operation is not supported. (Exception from HRESULT: 0x80131515)File name: 
'file:///C:\Users\benperk\AppData\Local\OpenLiveWriter\app-0.6.0.0\Plugins\SyntaxHighlight_WordPressCom_OLWPlugIn.dll' ---> 
System.NotSupportedException: An attempt was made to load an assembly from a network location which would have caused the assembly to be
sandboxed in previous versions of the .NET Framework. This release of the .NET Framework does not enable CAS policy by default, so this
load may be dangerous. If this load is not intended to sandbox the assembly, please enable the loadFromRemoteSources switch. See
http://go.microsoft.com/fwlink/?LinkId=155569 for more information.
``` 

![how to check the version of .NET installed on your work station][FIGURE2]
###### Figure 2, how to check the version of .NET installed on your work station

![source code plugin for WordPress blog and Open Live Writer][FIGURE3]
###### Figure 3, source code plugin for WordPress blog and Open Live Writer

![Open Live Writer log file, source code plugin][FIGURE4]
###### Figure 4, Open Live Writer log file, source code plugin

![Open Live Writer log file, source code plugin][FIGURE5]
###### Figure 5, Open Live Writer log file, source code plugin

![Open Live Writer source code plugin][FIGURE6]
###### Figure 6, Open Live Writer source code plugin

[FIGURE1]: ../images/2017/msdn-1109.png "Figure 1, wordpress on Azure, so easy"
[FIGURE2]: ../images/2017/msdn-1110.png "Figure 2, how to check the version of .NET installed on your work station"
[FIGURE3]: ../images/2017/msdn-1111.png "Figure 3, source code plugin for WordPress blog and Open Live Writer"
[FIGURE4]: ../images/2017/msdn-1122.png "Figure 4, Open Live Writer log file, source code plugin"
[FIGURE5]: ../images/2017/msdn-1133.png "Figure 5, Open Live Writer log file, source code plugin"
[FIGURE6]: ../images/2017/msdn-1144.png "Figure 6, Open Live Writer source code plugin"
