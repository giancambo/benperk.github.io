# Updating the configuration for the web site failed, Azure Websites

I was making a lot of changes to one of my Azure Websites the other day and received the messages within the Azure Management Console similar to those shown in Figure 1 and 2.

![Updating the configuration for web site failed][FIGURE1]
###### Figure 1, Updating the configuration for web site failed

![The deployment history is unavailable when the site is stopped][FIGURE2]
###### Figure 2, The deployment history is unavailable when the site is stopped

Azure Websites run on IIS and therefore when you make changes to your configuration, like an on premise installation, the app domain will likely recycle so that the configuration changes get applied and utilized when the pages are rendered.  My favorite article about this topic is [here][LINK1].

What I did to resolve the issue was to recycle the web site.  I pressed the Restart button as shown in Figure 3, the error went away and everything worked out just fine from that point.

![Restarting an Azure Web Site from via the Azure Management Console][FIGURE3]
###### Figure 3, Restarting an Azure Web Site from via the Azure Management Console

[FIGURE1]: ../images/2014/msdn-0449.png "Figure 1, Updating the configuration for web site failed"
[FIGURE2]: ../images/2014/msdn-0450.png "Figure 2, The deployment history is unavailable when the site is stopped"
[FIGURE3]: ../images/2014/msdn-0451.png "Figure 3, management"

[LINK1]: http://blogs.msdn.com/b/tess/archive/2006/08/02/686373.aspx
