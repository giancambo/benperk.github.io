# Using HTTP ETW tracing to troubleshoot HTTP issues

I wrote [this][LINK1] article here in regards to an issue that I was having after installing an SSL certificate on an IIS web server.

One of the tools I used was the HTTP ETW trace, also known as LOGMAN.

As you notice from reading the other previous article, I was receiving a 404 HTTP Status code and it was only being written to the HTTP Error log, which is by default located here:  C:\windows\system32\logfiles\httperr.  As the error logged into the file is not very helpful and we know that the issue is happening at the HTTP level because there is no entry into the IIS log, the right tool for the job is the HTTP ETW trace.

To enable the HTTP Trace enter the following command and as shown on Figure 1.

***logman start httptrace -p Microsoft-Windows-HttpService 0xFFFF -o httptrace.etl –ets***

![Enable LOGMAN for HTTP Tracing][FIGURE1]
###### Figure 1, Enable LOGMAN for HTTP Tracing

Once entered, the logging is running.  Reproduce the problem and the entry should be logged for analysis.  Enter the following command, as show below and in Figure 2 to stop the logging and write the data to file.

***logman stop httptrace –ets***

![Stop LOGMAN for HTTP Tracing and write the file][FIGURE2]
###### Figure 2, Stop LOGMAN for HTTP Tracing and write the file

The data is written into an ETL file, I like to convert it to XML so that I can then open it in Visual Studio for analysis.  To convert the ETL file to XML execute the following command, as shown below and in Figure 3.

***tracerpt.exe httptrace.etl -of XML -o httptrace.xml***

![Convert ETL LOGMAN HTTP Trace to XML format][FIGURE3]
###### Figure 3, Convert ETL LOGMAN HTTP Trace to XML format

As I was getting a 404, I went through the file searching for a 404 and found the entry show in Figure 4.

![404 entry in an HTTP ETW trace][FIGURE4]
###### Figure 4, 404 entry in an HTTP ETW trace

The important piece of information in Figure 4 is the RequestId.  Copy that to your clipboard and do another search for it.  In my trace it resulted as shown in Figure 5.

![404 entry in an HTTP ETW trace with RequestId Message = UrlGroupLookupFailed][FIGURE5]
###### Figure 5, 404 entry in an HTTP ETW trace with RequestId Message = UrlGroupLookupFailed

The error message associated to the RequestId is:  ***Request (request ID 0xE9…) rejected due to reason: UrlGroupLookupFailed.***

After the fact it turned out to be a simple binding issue, but finding out where the 404 was coming from and how to troubleshoot that kind of issue was something of value.  I was able to further analyze and find the cause by looking at the source code, something most people cannot do.  If you get to this point and can’t take it further, you would need to raise a support case with Microsoft.

[FIGURE1]: ../images/2014/msdn-0338.png "Figure 1, Enable LOGMAN for HTTP Tracing"
[FIGURE2]: ../images/2014/msdn-0339.png "Figure 2, Stop LOGMAN for HTTP Tracing and write the file"
[FIGURE3]: ../images/2014/msdn-0340.png "Figure 3, Convert ETL LOGMAN HTTP Trace to XML format"
[FIGURE4]: ../images/2014/msdn-0341.png "Figure 4, 404 entry in an HTTP ETW trace"
[FIGURE5]: ../images/2014/msdn-0342.png "Figure 5, 404 entry in an HTTP ETW trace with RequestId Message = UrlGroupLookupFailed"

[LINK1]: ../2014/2014-02-http-404-after-installing-an-ssl-certificate.md
