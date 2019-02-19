# Configure Debug Diagnostic 2.0 to create a memory dump when a specific exception is thrown

Debugging a process crash is a challenging activity.  Although you know the exception type, the exceptions error message and you know the method that it is being thrown from, you still can't find out why the exception is being thrown.  It is especially complicated when the error is happening randomly and you cannot reproduce it.
A method for getting to the exceptions root cause is to use Debug Diagnostic to create a dump when that exception happens within a specific process.  I will show how to setup Debug Diagnostic to catch a Win32Exceptions from within a W3WP worker process.
Download the new 2.0 version of Debug Diagnostic here.  The team blog is here, just in case the download link does not work.
I wrote a simple website that throws a Win32Exception.  The code is shown in Figure 1.

![Throwing a Win32Excpetion from an ASPX page][FIGURE1]
###### Figure 1, Throwing a Win32Excpetion from an ASPX page

I then take this example program and place it on an IIS7/8 server.  I make sure that the program can run and creates the W3WP worker process that I want to configure Debug Diagnostic to monitor.  Figure 2 shows the Task Manager window containing the W3WP work process running the application pool Win32Exception.

![Task Manager showing W3WP worker process running the Win32Exception application pool.][FIGURE1]
###### Figure 1, Task Manager showing W3WP worker process running the Win32Exception application pool.

Next, open Debug Diagnostic and follow these steps:

1. Select Rule Type: Crash
2. Select Target Type: A specific IIS web application pool
3. Select Target: <TheApplicationPool>
4. Advanced Configuration -> Advanced Settings -> Exceptions… 
   1. First chance Exception Configuration -> Add Exception
   2. Exception Name: CLR (.NET) 4.0 Exception
   3. .NET Exception Type:  System.ComponentModel.Win32Exception
   4. Action Type: Full Userdump
5. Add Rule Name and Userdump location
6. Activate the rule

## Step 1

Select the Rule Type, as shown in Figure 3.

![Debug Diagnostic - Select Role Type][FIGURE3]
###### Figure 3, Debug Diagnostic - Select Role Type

## Step 2

Select "A specific IIS web application pool" as shown in Figure 4.

![Debug Diagnostic - Select a Target Type of A specific IIS web application pool][FIGURE4]
###### Figure 4, Debug Diagnostic - Select a Target Type of A specific IIS web application pool

## Step 3

Select the target application pool.  In the example, I have created a website and application pool called Win32Exception.  Figure 5 shows the selection.

![Debug Diagnostic - Select Target][FIGURE5]
###### Figure 5, Debug Diagnostic - Select Target

## Step 4
Configure the exception.  Leave all the default setting on the Advanced Configuration window as that are.  In the Advanced Settings group box select the Exceptions… button -> then the Add Exception… button.  Select the CLR version you are using, in this example I am using .NET 4.0 and therefore select "CLR (.NET) 4.0 Exception".  If you are using .NET 2.0 then you should select the 1.0 - 3.5 Exception value.

Once selected a ".NET Exception Type" text box is displayed for you to place the name of the exception you with to monitor for.  As you recall, I am trying to capture a Win32Exception that is occurring within my Win32Exception application pool.  Note that the value is
case sensitive.  To be sure I get it correct; I place my mouse over the exception in Visual Studio to get the description and case, as shown in Figure 6.

![System.ComponentModel.Win32Excpetion description and case][FIGURE6]
###### Figure 6, System.ComponentModel.Win32Excpetion description and case

You can also access the description on MSDN [here][LINK3].

This will show you the correct name, syntax and description.  Lastly, select "Full Userdump" from the Action Type dropdown and leave the Action Limit as 1.  The final result is shown in Figure 7.

![Debug Diagnostic - Configure the Exception][FIGURE7]
###### Figure 7, Debug Diagnostic - Configure the Exception

Lastly, name the rule, select where you want the memory dump to be stored and activate the rule.

When I ran the test application that throws the Win32Exception, the Debug Diagnostic Tool resembles Figure 8.

![Debug Diagnostic - Rules tab][FIGURE8]
###### Figure 8, Debug Diagnostic - Rules tab

Now you have a memory dump of the W3WP work process with the Win32Exception application pool.  You can now use either the Advanced Analysis tab within the Debug Diagnostic tool or WinDbg to search and find the cause of the exception.

The result will show you the specific method which is throwing the exception.  I would then proceed to look at the methods used within the failing method and find which one is capable of throwing the exception.

## Solution

Check the parameters of the method and make sure your logic prevents it from receiving values that cause the exception.

[FIGURE1]: ../images/2014/msdn-0351.png "Figure 1, Throwing a Win32Excpetion from an ASPX page"
[FIGURE2]: ../images/2014/msdn-0352.png "Figure 2, Task Manager showing W3WP worker process running the Win32Exception application pool."
[FIGURE3]: ../images/2014/msdn-0353.png "Figure 3, Debug Diagnostic - Select Role Type"
[FIGURE4]: ../images/2014/msdn-0354.png "Figure 4, Debug Diagnostic - Select a Target Type of A specific IIS web application pool"
[FIGURE5]: ../images/2014/msdn-0355.png "Figure 5, Debug Diagnostic - Select Target"
[FIGURE6]: ../images/2014/msdn-0356.png "Figure 6, System.ComponentModel.Win32Excpetion description and case"
[FIGURE7]: ../images/2014/msdn-0357.png "Figure 7, Debug Diagnostic - Configure the Exception"
[FIGURE8]: ../images/2014/msdn-0358.png "Figure 8, Install"


[LINK1]: https://www.microsoft.com/en-us/download/details.aspx?id=49924
[LINK2]: http://blogs.msdn.com/b/debugdiag/
[LINK3]: http://msdn.microsoft.com/en-us/library/system.componentmodel.win32exception.aspx
