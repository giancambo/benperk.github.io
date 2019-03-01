# Helloworld WebJob on Microsoft Azure – Web App

I watched this Channel9 Video [here][LINK1] and decided to reproduce it in writing.  The steps required to create this sample HelloWorld WebJob for hosting with a Web App are:

+ Create a Web Site and Console Application
+ Include the Console App into the Web Site
+ Deploy the Web Site to a Web App
+ Execute the WebJob and check the log file

## Create a Web Site and Console Application

I started by creating an empty ASP.NET Empty Web Site, then added a new Console Application project to the solution.  The website project is web-job and the Console application is web-job-helloworld.  The result is shown in Figure 1.

![setup Visual Studio projects for WebJob creation and deployment][FIGURE1]
###### Figure 1, setup Visual Studio projects for WebJob creation and deployment

The web-job-helloworld Console project contained the following code, shown in Listing 1.

###### Listing 1, WebJob helloworld source code

```
namespace web_job_helloworld
{
 class Program
 {
  static void Main(string[] args)
  {
   Console.WriteLine("WebJob HelloWorld");
  }
 }
}
```

## Include the Console App into the Web Site

Then, I added an App_Data ASP.NET folder to my Website as shown in Figure 2.

![adding an App_Data folder to a Web Site for supporting a WebJob][FIGURE2]
###### Figure 2, adding an App_Data folder to a Web Site for supporting a WebJob

Once the App_Data folder is added to the project, create the directory structure shown in Figure 3, and then add the compiled web-job-helloworld executable and supporting files into it.

![eb App (Web Site) WebJob directory structure][FIGURE3]
###### Figure 3, eb App (Web Site) WebJob directory structure

To add the executable, right-click the web-job-hellowworld directory -> Add -> Existing Item… -> Navigate to the location where the compiled web-job-hellowworld files exist and add them all.  Figure 4 shows the files added in this example.

![adding the EXE which I want to run as a WebJob on Azure][FIGURE4]
###### Figure 4, adding the EXE which I want to run as a WebJob on Azure

Once added, expand the web-job-hellowworld folder and the result should be similar to that illustrated in Figure 5.

![Web Site (Web App) Visual Studio project containing a WebJob][FIGURE5]
###### Figure 5, Web Site (Web App) Visual Studio project containing a WebJob

## Deploy the Web Site to a Web App

Deploy the Visual Studio Web Site to a Microsoft Azure Web App, as shown in Figure 6.  Right-click on the web-job Web Site project -> Publish Web App which opens the wizard shown in Figure 6 that walks you through the deployment of the Web Site.

![Deploy a Web App that includes a WebJob][FIGURE6]
###### Figure 6, Deploy a Web App that includes a WebJob

After the Web App is successfully published, login to the Azure Management Console and see that it is there, as shown in Figure 7.

![A HelloWorld WebJob deployed to a Web App][FIGURE7]
###### Figure 7, A HelloWorld WebJob deployed to a Web App

You can also check KUDU to see that the files have been deployed, as shown in Figure 8.  I wrote a short article about what KUDU is here.

![view a WebJob in SCM / KUDU][FIGURE8]
###### Figure 8, view a WebJob in SCM / KUDU

Execute the WebJob and check the log file
Click the RUN ONCE button as shown previously in Figure 7.  Once completed, the LAST RUN TIME and LAST RUN RESULT are updated as shown in Figure 9.

![running a WebJob via the Azure Management Console][FIGURE9]
###### Figure 9, running a WebJob via the Azure Management Console

Hover over the LOGS URL and copy it to your clip board, enter the URL into a browser and SCM / KUDU is opened where you can view the log of the WebJob.  The initial window shows you a list of recent job runs.  Click on the job that you just executed and you should see a page similar to the one shown in Figure 10.

![WebJob log files][FIGURE10]
###### Figure 10, WebJob log files

***UPDATE***: You can also view the WebJobs using the Azure SDK from within Visual Studio, as shown in Figure 11.

![Checking WebJobs via Visual Studio Azure SDK][FIGURE11]
###### Figure 11, Checking WebJobs via Visual Studio Azure SDK


[FIGURE1]: ../images/2015/msdn-0516.png "Figure 1, setup Visual Studio projects for WebJob creation and deployment"
[FIGURE2]: ../images/2015/msdn-0517.png "Figure 2, adding an App_Data folder to a Web Site for supporting a WebJob"
[FIGURE3]: ../images/2015/msdn-0518.png "Figure 3, eb App (Web Site) WebJob directory structure"
[FIGURE4]: ../images/2015/msdn-0519.png "Figure 4, adding the EXE which I want to run as a WebJob on Azure"
[FIGURE5]: ../images/2015/msdn-0520.png "Figure 5, Web Site (Web App) Visual Studio project containing a WebJob"
[FIGURE6]: ../images/2015/msdn-0521.png "Figure 6, Deploy a Web App that includes a WebJob"
[FIGURE7]: ../images/2015/msdn-0522.png "Figure 7, A HelloWorld WebJob deployed to a Web App"
[FIGURE8]: ../images/2015/msdn-0523.png "Figure 8, view a WebJob in SCM / KUDU"
[FIGURE9]: ../images/2015/msdn-0524.png "Figure 9, MSrunning a WebJob via the Azure Management ConsoleN"
[FIGURE10]: ../images/2015/msdn-0525.png "Figure 10, WebJob log files"
[FIGURE11]: ../images/2015/msdn-0526.png "Figure 11, Checking WebJobs via Visual Studio Azure SDK"

[link1]: https://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-103-Programming-WebJobs-in-NET-with-Pranav-Rastogi
