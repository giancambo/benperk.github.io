# Could not load type “Namespace.Class” from assembly “Namespace”

I have been working hard learning the new features of ASP.NET 4.5, specifically using the following methods:
+ HttpResponse.BeginFlush
+ HttpResponse.EndFlush
+ Stream.ReadAsync
+ HttpRequest.GetBufferedInputStream
+ HttpRequest.GetBefferlessInputStream
+ Stream.BeginRead
+ Stream.EndRead

All of which need to be implemented using either a handler or module, if you want to use them in IIS.  Can’t say that I did this a lot in the past, but this error took me a lot of time to resolve and searching for a solution was equally time consuming.  Therefore, it is worthy of this blog.

The default properties for a class in Visual Studio 2012 are shown in Figure 1.

![efault class properties][FIGURE1]
###### Figure 1, efault class properties

After configuring the module in my web.config file, when I ran the ASP.NET 4.5 web site, the following as shown in Figure 2 was rendered.

![Could not load type ‘Namespace.Class’ from assembly ‘Namespace’][FIGURE2]
###### Figure 2, Could not load type ‘Namespace.Class’ from assembly ‘Namespace’

The solution was to change the Build Action property from Content to Compile.  And bingo, the error went away.

![Solution: change Build Action from Content to Compile][FIGURE3]
###### Figure 3, Solution: change Build Action from Content to Compile


[FIGURE1]: ../images/2013/msdn-0305.png "Figure 1, efault class properties"
[FIGURE2]: ../images/2013/msdn-0306.png "Figure 2, Could not load type ‘Namespace.Class’ from assembly ‘Namespace’"
[FIGURE3]: ../images/2013/msdn-0307.png "Figure 3, Solution: change Build Action from Content to Compile"
