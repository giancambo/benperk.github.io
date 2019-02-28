# Connection Timeout (Timeout Expired) on Azure Web App Site / SQL Azure

I not only support Azure Web Apps, formerly Azure Websites, but I use it too.  I had a site that was getting the exception shown in Figure 1.  The error was “Timeout expired.  The timeout period elapsed prior to obtaining a connection from the pool.  This may have occurred because all pooled connections were in use and max pool size was reached”.

![timeout expired Azure Web App and SQL Azure][FIGURE1]
###### Figure 1, timeout expired Azure Web App and SQL Azure

As this site is a hobby and not work related, I wanted to get the quickest solution implemented as possible.  I do know that there are database optimizations and pool size changes I could make, but not wanting to spend too much time on it, I chose another path.  NOTE: I do create me SQL Connection within a USING{} code block, so as least from this perspective, I coded the creation and finalization of my connection just fine, additionally, I resolved the issue a number of times by simply recycling the IIS worker process.

What I needed to find was the HTTP status code generated when this exception happened.  I had setup the storage of my IIS logs for this site as described [here][LINK1], so downloading them was not a problem.  I then used Log Parser Studio, read about it [here][LINK2], to find it.  As seen in Figure 2, the HTTP Status for this exception shown in Listing 1, is 500.

![Log Parser Studio for IIS log file analysis][FIGURE2]
###### Figure 2, Log Parser Studio for IIS log file analysis

I know that an exception would not result in a 200, 304 or a 404.0 so I assumed this was the case.  Additionally, however I checked the result of my specific page request, using a different logparser query, and this confirmed that the result was indeed a 500.  The code in Listing 1 is recovered from the EVENTLOG.XML which is in the /LogFiles directory when you access via FTP or from a Console via [KUDU][LINK3].

###### Listing 1, timeout expired exception stack

```
<Data>http ://www.??*.com/archiveList.aspx?YEAR=2011&amp;MONTH=June</Data>
<Data>/archiveList.aspx</Data>
<Data>IP Address</Data>
<Data></Data>
<Data>False</Data>
<Data></Data>
<Data>IIS APPPOOL\appPoolName</Data>
<Data>49</Data>
<Data>IIS APPPOOL\appPoolName</Data>
<Data>False</Data>
<Data> at System.Data.ProviderBase.DbConnectionFactory.TryGetConnection(DbConnection owningConnection, TaskCompletionSource`1 retry, DbConnectionOptions userOptions, DbConnectionInternal oldConnection, DbConnectionInternal&amp; connection) at System.Data.ProviderBase.DbConnectionInternal.TryOpenConnectionInternal(DbConnection outerConnection, DbConnectionFactory connectionFactory, TaskCompletionSource`1 retry, DbConnectionOptions userOptions) at System.Data.ProviderBase.DbConnectionClosed.TryOpenConnection(DbConnection outerConnection, DbConnectionFactory connectionFactory, TaskCompletionSource`1 retry, DbConnectionOptions userOptions) at System.Data.SqlClient.SqlConnection.TryOpenInner(TaskCompletionSource`1 retry) at System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry) at System.Data.SqlClient.SqlConnection.Open() at TBCSPITW.ConnectionManager.GetConnection() at TBCSPITW.PostsDAC.GetArchiveByMonth(String year, String month) at archiveList.Page_Load(Object sender, EventArgs e)
...</Data>
```

My objective was to resolve this as quickly as possible and get the site back up and running when the exception started happening.  Up to this point I would just think about accessing the Azure Web App,  experience the excpetion and then manually restart the Azure Web App if required.  I therefore decided to implement Auto Healing as described [here][LINK4].  Scenario 3 describes the requirements for recycling the IIS worker process based on an HTTP status code.  Basically, I added the configuration shown in Listing 2 to my websites’ web.config file.

###### Listing 2, recycle IIS worker process based on HTTP status code

```
<system.webServer>
  <monitoring>
    <triggers>
      <statusCode>
        <addstatusCode="500"subStatusCode="0"win32StatusCode="0" count="10"timeInterval="00:00:30" />
      </statusCode>
    </triggers>
    <actionsvalue="Recycle" />
  </monitoring>
</system.webServer>
```

Now, when 10, 500.0.0 HTTP status codes get logged within 30 seconds, the IIS worker process will get recycled.  For the past few months, this has been working nicely and have not personally experienced a Timeout while navigating the site hosted on the Azure Web Apps platform.

For an enterprise or mission critical system this would not be the most optimal solution, as you would be frustrating your users from time to time and perhaps loose some business…  So if you want to solve the issue, can debug it deeper and/or create a support case with Microsoft and we can work with you to find the reason.  As this Azure Website is just a hobby one, it works ok.

[FIGURE1]: ../images/2015/msdn-0500.png "Figure 1, timeout expired Azure Web App and SQL Azure"
[FIGURE2]: ../images/2015/msdn-0501.png "Figure 2, Log Parser Studio for IIS log file analysis"

[LINK1]: ../2014/2014-02-how-to-store-and-access-your-iis-logs-on-windows-azure-web-site.md
[LINK2]: http://blogs.technet.com/b/exchange/archive/2012/03/07/introducing-log-parser-studio.aspx
[LINK3]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK4]: http://azure.microsoft.com/blog/2014/02/06/auto-healing-windows-azure-web-sites/
