# Lab 15: Debug Diagnostics

## Setup

Setup the CSharpGuitarBugs web site as described in [Lab 1][LINK1] and [Lab 2][LINK2].

Download and install most recent version of Debug Diagnostics,from [here][LINK3].  Also check debugdiag.com

## Instructions

Debugging a process crash is a challenging activity. Although you know the exception type, the exceptions error message and you know the method that it is being thrown from, you still can't find out why the exception is being thrown. It is especially complicated when the error is happening randomly and you cannot reproduce it.

Sometimes you can find the specific exception in the Event Logs when a stack is present, or you can use for example procdump to see what exceptions are happening, as explained [here][LINK4].

A method for getting to the exceptions root cause is to use Debug Diagnostic to create a dump when that exception happens within a specific process. I will show how to setup Debug Diagnostic to catch a Win32Exceptions from within a W3WP worker process.

Once the CSharpGuitarBugs web site is installed, click on the “View By Color” link and you will notice a Win32Exception is thrown.  Make sure that the web site has run and a W3WP worker process that I want to configure Debug Diagnostic to monitor is created. Figure 1 shows the Task Manager window containing the W3WP work process running the application pool where the Win32Exception just happened.  NOTE, as this is a 1st chance exception, I.e. it was handled in the code via a try{}…catch{} code block, the process does not terminate and restart.

![Task Manager showing W3WP worker process running the Win32Exception application pool.][FIGURE1]
###### Figure 1, Task Manager showing W3WP worker process running the Win32Exception application pool.

Next, open Debug Diagnostic and follow these steps:

1. Select Rule Type: Crash
2. Select Target Type: A specific IIS web application pool
3. Select Target: <TheApplicationPool>
4. Advanced Configuration -> Advanced Settings -> Exceptions…
  a. First chance Exception Configuration -> Add Exception
  b. Exception Name: CLR (.NET) 4.0 Exception
  c. .NET Exception Type: System.ComponentModel.Win32Exception
  d. Action Type: Full Userdump
5. Add Rule Name and Userdump location
6. Activate the rule

## Step 1:

Select the Rule Type, as shown in Figure 2.

![Debug Diagnostic - Select Role Type][FIGURE2]
###### Figure 2, Debug Diagnostic - Select Role Type

## Step 2:

Select "A specific IIS web application pool" as shown in Figure 3.

![Debug Diagnostic - Select a Target Type of A specific IIS web application pool][FIGURE3]
###### Figure 3, Debug Diagnostic - Select a Target Type of A specific IIS web application pool

## Step 3:

Select the target application pool. In the example, I have created a website and application pool called Win32Exception. Figure 4 shows the selection.

![Debug Diagnostic - Select Target][FIGURE4]
###### Figure 4, Debug Diagnostic - Select Target

## Step 4:

Configure the exception. Leave all the default setting on the Advanced Configuration window as that are. In the Advanced Settings group box select the Exceptions… button -> then the Add Exception… button. Select the CLR version you are using, The CSharpGuitarBugs web site uses .NET 4.5.2 and therefore select "CLR (.NET) 4.0 Exception". If you are using .NET 2.0 then you should select the 1.0 - 3.5 Exception value.

Once selected a ".NET Exception Type" text box is displayed for you to place the name of the exception you with to monitor for. As you recall, I am trying to capture a Win32Exception that is occurring within my CSharpGuitarBugs application pool. Note that the value is case sensitive. To be sure I get it correct; I place my mouse over an exception in Visual Studio to get the exact description and case, as shown in Figure 5.

![System.ComponentModel.Win32Excpetion description and case][FIGURE5]
###### Figure 5, System.ComponentModel.Win32Excpetion description and case

You can also access the description on MSDN [here][LINK5], which will show you the correct name, syntax and description. Lastly, select "Full Userdump" from the Action Type dropdown and leave the Action Limit as 1. The final result is shown in Figure 6.

![Debug Diagnostic - Configure the Exception][FIGURE6]
###### Figure 6, Debug Diagnostic - Configure the Exception

Lastly, name the rule, select where you want the memory dump to be stored and activate the rule.

Run the CSharpGuitarBugs web site and click on “View By Color”.  This will throw a Win32Exception, the Debug Diagnostic Tool resembles Figure 7.

![Debug Diagnostic - Rules tab][FIGURE7]
###### Figure 7, Debug Diagnostic - Rules tab

Now you have a memory dump of the W3WP worker process for the CSharpGuitarBugs application pool. You can now use either the Advanced Analysis tab within the Debug Diagnostic tool or WinDbg to search and find the cause of the exception.

The result will show you the specific method which is throwing the exception. I would then proceed to look at the methods used within the failing method and find which one is capable of throwing the exception.

Try to configure an Access Denied rule, then click on the “Add items to your Shopping Cart” and try to add the item to your shopping cart.  The rule would look something like Figure 8.

![Debug Diagnostic - Configure the Exception][FIGURE8]
###### Figure 8, Debug Diagnostic - Configure the Exception

[FIGURE1]: ../images/2016/msdn-0828.png "Figure 1, Task Manager showing W3WP worker process running the Win32Exception application pool."
[FIGURE2]: ../images/2016/msdn-0829.png "Figure 2, Debug Diagnostic - Select Role Type"
[FIGURE3]: ../images/2016/msdn-0830.png "Figure 3, Debug Diagnostic - Select a Target Type of A specific IIS web application pool"
[FIGURE4]: ../images/2016/msdn-0831.png "Figure 4, Debug Diagnostic - Select Target"
[FIGURE5]: ../images/2016/msdn-0832.png "Figure 5, System.ComponentModel.Win32Excpetion description and case"
[FIGURE6]: ../images/2016/msdn-0833.png "Figure 6, Debug Diagnostic - Configure the Exception"
[FIGURE7]: ../images/2016/msdn-0834.png "Figure 7, Debug Diagnostic - Rules tab"
[FIGURE8]: ../images/2016/msdn-0835.png "Figure 8, Debug Diagnostic - Configure the Exception"

[LINK1]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK2]: 2016-IISLAB-lab-2-install-the-web-platform-installer.md
[LINK3]: http://www.microsoft.com/en-us/download/details.aspx?id=40336
[LINK4]: ../2012/2012-10-new-procdump-version-5-0-released-using-the-f-parameter.md
[LINK5]: http://msdn.microsoft.com/en-us/library/system.componentmodel.win32exception.aspx
