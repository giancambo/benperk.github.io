# Troubleshooting an IIS Add Roles and Features error

It is common to install or uninstall Web Server features.  You need these features to execute ASP.NET pages, perform logging, authenticate user requests, etc….  Figure 1 shows a sub-set list of features which you may choose to install.

![Add Roles and Features list, small example][FIGURE1]
###### Figure 1, Add Roles and Features list, small example

Try walking through the 'Add Roles and Features' wizard to see the list of all possible features.

## Issue and resolution 

I recently worked on case where an error was being rendered during the installation of a feature.  Figure 2 shows the error.

![An error while installing an IIS feature][FIGURE2]
###### Figure 2, An error while installing an IIS feature

The error message is: "Attempt to install <feature name> failed with error code 0x80070643.  Fatal error during installation"  Although this example is referring to the IIS 6 Management Console, the installation what in fact failing for any of the IIS features.

To solve this specific case, the following actions where performed:

+ Take a backup of the configuration using AppCmd, example [here][LINK1].
+ Disables Shared Configuration
+ Attempted to reinstall the feature, success as shown in Figure 3
+ Restored the configuration using APpCmd, example [here][LINK1].

![Successful installation of the IIS feature][FIGURE3]
###### Figure 3, Successful installation of the IIS feature

## Other Troubleshooting tips

If you are receiving the same error, but are not using shared configuration or you are experiencing a different error there are 2 places I recommend looking for more information.

First, when you install or remove an IIS feature the steps taken are written to an IIS log located in the C:\windows directory.  In IIS 7, the file is named IIS7.log.  In IIS 8, the file is currently named IIS.log.  An example of its contents is shown in Figure 4.

![IIS7.log and IIS.log file examples][FIGURE4]
###### Figure 4, IIS7.log and IIS.log file examples

The second place to look is in the Event Viewer.  Look at the Setup events.  For example, I installed Windows Authentication and the following entry (Figure 5) was added to the Setup event log.

![Setup event log][FIGURE5]
###### Figure 5, Setup event log

You may want to look into the System event log to for any additional errors.  These error descriptions and event id may lead to the solution to the problem.

[LINK1]: ../2011/2011-08-create-an-iis-configuration-backup.md

[FIGURE1]: ../images/2012/msdn-0197.png "Figure 1, Add Roles and Features list, small example"
[FIGURE2]: ../images/2012/msdn-0198.png "Figure 2, An error while installing an IIS feature"
[FIGURE3]: ../images/2012/msdn-0199.png "Figure 3, Successful installation of the IIS feature"
[FIGURE4]: ../images/2012/msdn-0200.png "Figure 4, IIS7.log and IIS.log file examples"
[FIGURE5]: ../images/2012/msdn-0201.png "Figure 5, Setup event log"
