# Debug = True

If you have ever performed website optimization then it is likely you have read this article [here][LINK1].  This article discusses the impact of having the debug set to true in your production application.  In another one of my blogs on C#, I made the comment once about "what is a technical blog without a comparison of String vs. Stringbuilder" so I thought that same here on my IIS blog that "what is a technical blog without a debug=true entry".  Therefore, here goes!  Be warned, that this is an article about how to find the value in your configuration and not a discussion about the impact of it.

One important concept to understand is the Configuration Hierarchy and Effective Configuration.  In summary, it means that any setting at a lower level override that same setting made at a higher level.  So if you set debug=false in the machine.config file for a given .NET version, but the debug value is set to true in a web.config of the website, then for that website, debug=true.  Figure 1 shows the configuration hierarchy for IIS 7+.

![IIS 7, IIS 7.5 and IIS 8 configuration hierarchy][FIGURE1]
###### Figure 1, IIS 7, IIS 7.5 and IIS 8 configuration hierarchy

There are a number of places where you can set the value of debug.  Review this article which describes the compilation element and the location in which it can be set.  [Here][LINK2] is the link.  Figure 2 shows how the setting exists in a websites web.config file.

![setting the debug=true in the web.config file][FIGURE2]
###### Figure 2, setting the debug=true in the web.config file

You can also check the value of this setting in the Configuration Editor on the specific website, as shown in Figure 3.  Notice the 'Section' and 'From' values.

![Configuration Editor showing the compilation element for viewing the debug value][FIGURE3]
###### Figure 3, Configuration Editor showing the compilation element for viewing the debug value

If you have some memory dumps of this process you can also review them in WinDbg.  In WinDbg, a command I always execute when I review a memory dump is !findDebugTrue.  Figure 4 shows the output of the command when the value is true.

![WinDbg output for !findDebugTrue when value is true][FIGURE4]
###### Figure 4, WinDbg output for !findDebugTrue when value is true

Finally, Figure 5 shows the output from the same command when the value is false.

![WinDbg output for !findDebugTrue when value is false][FIGURE5]
###### Figure 5, WinDbg output for !findDebugTrue when value is false

There you have it, the debug=true blog.  But really, use this only for your testing environments and change the value when migrating it to production.  It does have some performance impact.

***NOTE:***  When debug=true, asp.net requests will not time out.  httpRuntime settings do not apply to asynchronous handlers, only synchronous ones.  ASP.NET is synchronous while MVC and Web APIs are asynchronous.

[LINK1]: http://blogs.msdn.com/b/tess/archive/2006/04/13/575364.aspx
[LINK2]: http://msdn.microsoft.com/en-us/library/s10awwz0(v=vs.90).aspx

[FIGURE1]: ../images/2012/msdn-0159.png "Figure 1, IIS 7, IIS 7.5 and IIS 8 configuration hierarchy"
[FIGURE2]: ../images/2012/msdn-0160.png "Figure 2, setting the debug=true in the web.config file"
[FIGURE3]: ../images/2012/msdn-0161.png "Figure 3, Configuration Editor showing the compilation element for viewing the debug value"
[FIGURE4]: ../images/2012/msdn-0162.png "Figure 4, WinDbg output for !findDebugTrue when value is true"
[FIGURE5]: ../images/2012/msdn-0163.png "Figure 5, WinDbg output for !findDebugTrue when value is false"
