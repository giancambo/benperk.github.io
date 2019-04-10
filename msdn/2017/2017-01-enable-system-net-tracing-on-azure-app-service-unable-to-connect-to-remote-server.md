# Enable System.Net tracing on Azure App Service

It is becoming a common scenario that customers of Azure App Services Web Apps are making requests to services hosted on other Azure IaaS or PaaS platforms, services not hosted on the Azure platform (on premises) and which use the System.Net class.  For example, making a request from your code that uses either of the following code snippets:

```
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(URL);
HttpWebResponse response = (HttpWebResponse)request.GetResponse();
```

NOTE:  Both the HttpWebRequest and HttpWebResponse are classes derived from System.Net.

In cases where a call to either HttpWebRequest or HttpWebResponse methods result in a System.Net.WebException, System.Net Tracing is one possible option for troubleshooting and finding the root cause.  Some common forms of an System.Net.WebException are, for example:

```System.Net.WebException: Unable to connect to the remote server ---> System.Net.Sockets.SocketException:  A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond <IP Address/Server Name:PORT> at …```

```Microsoft.WindowsAzure.Storage.StorageException: The remote name could not be resolved: '*******' ---> System.Net.WebException: The remote name could not be resolved: '********' at System.Net.HttpWebRequest.GetResponse()```

```0x8009030D SEC_E_UNKNOWN_CREDENTIALS (HR) The credentials supplied to the package were not recognized```

***NOTE***:  I wrote a blog here a few months ago which provide instructions on how to implement the same on an on premise server.

To implement System.Net tracing on your Azure Web App (Website), perform the following:

+ Add the configuration to your websites web.config file
+ Set the path for the log file to be written to
+ Reproduce the problem
+ Download and review the logs
+ Disable the logging

## Add the System.Net configuration to your web.config file

Add the <system.diagnostics> configuration documented here to your web.config file for the Azure Web App that is having the connection issue.

## Define the location where your trace file should be written to

I logged into KUDU as discussed here and navigated to the CMD Debug Console and created a directory called NetTracing in the D:\home\LogFiles directory, as shown in Figure 1.

![create directory to log System.Net traces on Azure Web App][FIGURE1]
###### Figure 1, create directory to log System.Net traces on Azure Web App

Then modify the path in the web.config file so that the trace is logged into that directory.  The location where you modify the path is within the <sharedListeners> tag and is the value of the initializeData property.  For example, as shown in Figure 2.

![example of setting the path for System.Net trace in the web.config file][FIGURE2]
###### Figure 2, example of setting the path for System.Net trace in the web.config file

Once the configuration is completed, publish the web.config file to your website and reproduce the problem.

NOTE (1):  If logging does not start or function as expected, you might consider recycling the website.

NOTE (2):  Be sure to add the traceOutputOptions attribute with the values of ProcessId, DateTime as shown in Figure 2

***You can download a sample of the web.config here.

## Download and review the logs

Once you have reproduced the issue one or more times, download the trace file locally and review the files for any error that can move your investigation forward.

I was able to download the trace file using an FTP tool, FileZilla for example, as shown in Figure 3.

![downloading the System.Net trace file][FIGURE3]
###### Figure 3, downloading the System.Net trace file

While trying to download the file I received the following error, shown in Figure 4, from within the FTP client, (The process cannot access the file because it is being used by another process).

![file used by another process, Azure Web App][FIGURE4]
###### Figure 4, file used by another process, Azure Web App

To resolve this I used the KUDU Process Explorer and checked for an existing handle to the network.log file.  I right clicked on the W3WP process, then properties, as shown in Figure 5.

![checking the handles of your Azure Web Apps worker process][FIGURE5]
###### Figure 5, checking the handles of your Azure Web Apps worker process

Then click on the Handles tab as shown in Figure 6, and scroll down looking for a handle to the file which your System.Net trace is being written to.  Which was found in my example and is shown in Figure 7.

![looking for open handles of you Azure Web App worker process][FIGURE6]
###### Figure 6, looking for open handles of you Azure Web App worker process

![the handle to a file which cannot be downloaded from Azure Web App][FIGURE7]
###### Figure 7, the handle to a file which cannot be downloaded from Azure Web App

Read the next section to see how I released the handle.

## Disable the System.Net tracing

Once you have captured the System.Net trace logs during a reproduction of the issue, you should disable the logging.  In just a short amount of time, the log file can grow very large.  When you comment out the configuration and redeploy the web.config to your Azure Web App, it will trigger a recycle of the website which releases the handle on the System.Net trace file.


[FIGURE1]: ../images/2017/msdn-1086.png "Figure 1, create directory to log System.Net traces on Azure Web App"
[FIGURE2]: ../images/2017/msdn-1087.png "Figure 2, example of setting the path for System.Net trace in the web.config file"
[FIGURE3]: ../images/2017/msdn-1088.png "Figure 3, downloading the System.Net trace file"
[FIGURE4]: ../images/2017/msdn-1089.png "Figure 4, file used by another process, Azure Web App"
[FIGURE5]: ../images/2017/msdn-1090.png "Figure 5, checking the handles of your Azure Web Apps worker process"
[FIGURE6]: ../images/2017/msdn-1091.png "Figure 6, looking for open handles of you Azure Web App worker process"
[FIGURE7]: ../images/2017/msdn-1092.png "Figure 7, the handle to a file which cannot be downloaded from Azure Web App"
