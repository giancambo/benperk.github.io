# Using the Azure WebJob API

I wrote an article [here][LINK1] that discusses how to troubleshoot a WebJob, during the creation of that post I found decided to figure out this technique as well (calling the WebJob API).  Originally, I was using the Azure SDK to trigger the WebJob from Visual Studio Server Explorer, as shown in Figure 1.

![trigger an Azure WebJob from Visual Studio, how to trigger a WebJob manually][FIGURE1]
###### Figure 1, trigger an Azure WebJob from Visual Studio, how to trigger a WebJob manually

However, I decided to use the details here to call the WebJob API using some C# code shown on Listing 1 and downloadable from here.

###### Listing 1, how to call the WebJob API using C#

```
string userName = "userName";
string userPassword = "userPWD";
string webJobName = "web-job-helloworld";
var unEncodedString = String.Format($"{userName}:{userPassword}");
var encodedString = Convert.ToBase64String(System.Text.Encoding.ASCII.GetBytes(unEncodedString));
string URL = "https://?.scm.azurewebsites.net/api/triggeredwebjobs/" + webJobName + "/run";
System.Net.WebRequest request = System.Net.WebRequest.Create(URL);
request.Method = "POST";
request.ContentLength = 0;
request.Headers["Authorization"] = "Basic " + encodedString;
System.Net.WebResponse response = request.GetResponse();
```

The WebJob API requires Basic Authentication using the publishing credentials, the different type of credentials are discussed [here][LINK2] and [here][LINK3].  You can download your publishing credentials from the Azure portal, as shown in Figure 2.  In my example, the Azure Web App hosting the WebJob is called web-job.

![download Azure publishing profile for use with the WebJob API][FIGURE2]
###### Figure 2, download Azure publishing profile for use with the WebJob API

Open the downloaded file and replace the userName and userPWD from with the file into the C# code.  The credentials are converted into the correct format and added to the request via an Authorization header.  Then the request is posted to the SCM URL of you Web App (I discuss this [here][LINK4]) and the WebJob is executed.

Execute the code that calls the WebJob API.

You can look in the Azure Portal to see that the WebJob is running or not, as shown in Figure 3.

![checking the status of a WebJob in the Azure portal][FIGURE3]
###### Figure 3, checking the status of a WebJob in the Azure portal

As well, you can click on the link also shown in Figure 3 which redirects you to the SCM site for more details about the WebJob, similar to that shown in Figure 4.

![viewing the WebJob status][FIGURE4]
###### Figure 4, viewing the WebJob status

Click on the specific run to view the logs in real time or also for review after a WebJob has completed, as shown in Figure 5

![view the run of a WebJob in real-time][FIGURE5]
###### Figure 5, view the run of a WebJob in real-time

I think this is pretty cool that you can execute a WebJob from any location from any authenticated client.  I haven’t actually done this but I have a web site that people have signed up to receive notifications when something specific is posted.  I was thinking I could write an EXE to send emails to that group of people when a specific scenario happened.  I had originally thought about using messaging queues and having a continuous WebJob monitoring it and act when a message is placed there, but I think this method is more friendly, if it is doable.

[FIGURE1]: ../images/2016/msdn-0600.png "Figure 1, trigger an Azure WebJob from Visual Studio, how to trigger a WebJob manually"
[FIGURE2]: ../images/2016/msdn-0601.png "Figure 2, download Azure publishing profile for use with the WebJob API"
[FIGURE3]: ../images/2016/msdn-0602.png "Figure 3, checking the status of a WebJob in the Azure portal"
[FIGURE4]: ../images/2016/msdn-0603.png "Figure 4, viewing the WebJob status"
[FIGURE5]: ../images/2016/msdn-0604.png "Figure 5, view the run of a WebJob in real-time"

[LINK1]: ../../waws/2016/2016-02-troubleshooting-a-hung-or-long-running-webjob.md
[LINK2]: https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service
[LINK3}: https://github.com/projectkudu/kudu/wiki/Deployment-credentials
[LINK4]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
