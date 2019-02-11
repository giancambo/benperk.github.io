# Changes made to one web site are applied to another

I was working on an issue some months ago where changes being made to a single web site were being applied to another web site on the server.  I found that strange because that should not happen, I mean, making a change to a web site should be specific to that web site and those changes should not be globally applied.

To make a long story short, there were multiple web sites pointing to the same physical path that included a web.config.  Therefore, changes to the web.config file used for the multiple web sites were applied to all the web sites.  It took some time to find that but I knew there was something fishy.

This was not the behavior that was needed so we went about finding the solution.  We wanted to have those web sites pointing to the same directory and at the same time be able to make configuration changes to the specific web site that apply only to that web site.

The solution is a concept called “Locking” which was introduced in IIS 7 and still valid in current versions of IIS.  This feature is most useful when an IIS administrator wants certain configurations to be configurable deeper into the web sites or the IIS Administrator wants to delegate control of the web sites configuration to the developers of the web site.  For example, let assume the IIS Administrator wants to allow Authentication to be enabled or disabled at the web site level.  To allow this, when in the Configuration Editor, navigate to the system.webServer/security/authentication/(any) as shown in Figure 1.

![Configuration Editor in IIS][FIGURE1]
###### Figure 1, Configuration Editor in IIS

Once selected, a link named “Unlock Section” will show on the right-hand side action pane as shown in Figure 2.  When selected, all authentication providers can be configured at the web site level.

![Unlocking a section in the Configuration Editor][FIGURE2]
###### Figure 2, Unlocking a section in the Configuration Editor

Before this section is unlocked, changes are made to the applicationHost.config file within the <location> tag for the given web site, as shown in Figure 3.

![the location path in the applicationHost.config file][FIGURE3]
###### Figure 3, the location path in the applicationHost.config file

After the unlocking of the section, any changes made to the configurations of that section are applied to the web.config file for that specific web site.

The solution to the original problem was to clean up the existing web.config file for the web site.  This required the removal of existing configurations that were to be locked.  For example, we wanted to allow Anonymous Authentication in one web site and not another.  So removing the <authentication> section from the web.config, locking that section and then applying the configuration resulted in the desired result.

[FIGURE1]: ../images/2013/msdn-0248.png "Figure 1, Configuration Editor in IIS"
[FIGURE2]: ../images/2013/msdn-0249.png "Figure 2, Unlocking a section in the Configuration Editor"
[FIGURE3]: ../images/2013/msdn-0250.png "Figure 3, the location path in the applicationHost.config file"
