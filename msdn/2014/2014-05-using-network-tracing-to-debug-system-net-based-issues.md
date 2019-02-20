# Using Network Tracing to debug System.Net based issues

I have worked on a number of cases where web services or URLs are called using a code segment like the below code snippet.

```
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(URL);
HttpWebResponse response = (HttpWebResponse)request.GetResponse();
```

The result of the GetResponse() method is some kind of error, for example a Timeout Exception, which is not always brought up to the application level or rendered to the client so that a support engineer can understand, troubleshoot and ultimately resolve the issue.  Often times, there is an error message sent to the client but it is too generic to add any value.

If by looking at the source code you determine that the error is coming from the methods shown in the previous code snippet or from the System.Net namespace, you can enable Network Tracing to capture better logs and information to troubleshoot and fix the problem.

[Here][LINK1] is an article that discusses how to enable Network Tracing.

It is straight forward to do, simply add the provided system.diagnostics configuration to the machine.config file for the .NET version in which your application runs in.  For example, if your application uses the .NET 4 Framework then modify the machine.config file
located in the C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config directory.

NOTE:  When I did this I was running on a 64bit machine and initially made the change to the machine.config file in the Framework64 directory, this had not impact.  Only when I made the change to the machine.config in the Framework directory did this work.

I wrote a small console application that asked for a URL and then used the code in the previous snippet to make the call, as shown in Figure 1.

![calling a URL using HttpWebRequest and HttpWebResponse, System.Net Tracing][FIGURE1]
###### Figure 1, calling a URL using HttpWebRequest and HttpWebResponse, System.Net Tracing

I did place my code in a try{} catch{} block so I see the error, but sometimes, in complex applications, it is not so aparent which URL is called, as it is dynamically built or what the exception is because it happens in a place without solid exception handling.  Nonetheless, after running the command I went looking for the log file….where is the log file that is written when using System.Net tracing?

Well, I looked at the configuration provided in the previous link to ‘How to: Configure Network Tracing’ and I noticed that there was a file within the configuration called network.log.  So I searched, using the search capabilities in Windows Explorer and found that it was written in the C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config.  I was also able to find it using Process Monitor when I filtered on the process name of my console application as shown in Figure 2.  There are many uses for Process Monitor and it can solve so many problems.

![using process monitor to solve any problem, once again][FIGURE2]
###### Figure 2, using process monitor to solve any problem, once again

I then opened the network.log file and was provided with a lot of details about the request which could help is resolving this simple example and even a very complicated security or SSL issue.  The log file contained:

+ The URL which was called
+ Socket information
+ The response header information
+ The response
+ The Exception

Below are examples of the Request and Response found in the log file which would be useful in finding the reason for application failures rooted in the System.Net namespace.

+ WebRequest::Create(http://*.azurewebsites.net/404.aspx)
+ Exception in HttpWebRequest::GetResponse - The remote server returned an error: (404) Not Found.
+ [Here][LINK2] is also something I found interesting as was the cause of a difference in performance on different clients

[FIGURE1]: ../images/2014/msdn-0387.png "Figure 1, deployment"
[FIGURE2]: ../images/2014/msdn-0388.png "Figure 2, deployment"

[LINK1]: http://msdn.microsoft.com/en-us/library/ty48b824(v=vs.90).aspx
[LINK2]: http://withinwindows.com/2011/09/22/the-case-of-the-slow-webrequest-getresponse-under-system
