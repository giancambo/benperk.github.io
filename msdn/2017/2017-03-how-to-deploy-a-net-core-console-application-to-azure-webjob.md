# How to deploy a .NET Core console application to Azure, WebJob

Check out some of my other articles I wrote in regards to ASP.NET Core and .Net Core

+ How to call an async method from a console app main method
+ How to deploy a .NET Core console application to Azure, WebJob
+ .NET Core application, where is my EXE, how to publish
+ Create a VNET and access an Azure VM hosted within it from an App Services Web App
+ Create and deploy an ASP.NET Core Web API to Azure Windows

That .NET Console application calls a Web API which I wrote about here, titled “Create and deploy an ASP.NET Core Web API to Azure Windows”.

Now, I want to deploy the .NET Core console application to an Azure App Service as a WebJob.  I wrote numerous articles about WebJos here.  However, if you do not have much experience with WebJobs read this and start with this HelloWorld example here.

As you will learn by reviewing those other articles, you see that:

+ A WebJob looks for specific file type, for example (.cmd, .bat, .exe, etc…)
+ To run a .NET Core console application you use the DOTNET command

Therefore, you need to create a file with an extension a WebJob is looking for that executes, for example: dotnet helloworldsleepy-webjob.dll for my example I named it run.cmd.  The contents of the directory which is created by a publish is shown in Figure1.

![ .NET Core console application deployment, publish package example][FIGURE1]
###### Figure 1,  .NET Core console application deployment, publish package example

To deploy the .NET Core console application to an Azure App Service Web App Web Job access the Azure portal and navigate to the Azure App Service where you will host the WebJob.  Then click on the WebJobs link and the Add button as shown in Figure 2.

![Add a .NET Core WebJob to Azure][FIGURE2]
###### Figure 2, Add a .NET Core WebJob to Azure

Clicking on the Add button renders the blade shown in Figure 3.

![adding a .NET Core WebJob to Azure, example, how to][FIGURE3]
###### Figure 3, adding a .NET Core WebJob to Azure, example, how to

The content of the ZIP file are those files shown previously in Figure 1.

Once the WebJob is successfuly uploaded, it will render in the WebJob blade.  Click on it and you will see the Run button, illustrated in Figure 4.  As this WebJob is a manually triggered job, you must click on the Run button in order for the job logic within the .NET Core console application to run.

![run a .NET Core console application as a WebJob on Azure][FIGURE4]
###### Figure 4, run a .NET Core console application as a WebJob on Azure

Also shown in Figure 4, there is a Logs button.  When you write output to the console using the WriteLine() method, it will show in the Run Details window on KUDU/SCM, which I discuss here.  After starting the .NET Core WebJob, click on the Logs link and a new browser tab is opened and you can see the most current state of the WebJob, see Figure 5.

![WebJob .NET Core logging][FIGURE5]
###### Figure 5, WebJob .NET Core logging

Lastly, you can see that the .NET Core WebJob is running within the DOTNET.EXE process by looking at the Process explorer in KUDU/SCM.  Click on the Properties button to view the command line, as seen in Figure 6.

![view the execution of the .NET Core WebJob running on Azure, example][FIGURE6]
###### Figure 6, view the execution of the .NET Core WebJob running on Azure, example


[FIGURE1]: ../images/2017/msdn-1191.png "Figure 1,  .NET Core console application deployment, publish package example"
[FIGURE2]: ../images/2017/msdn-1192.png "Figure 2, Add a .NET Core WebJob to Azure"
[FIGURE3]: ../images/2017/msdn-1193.png "Figure 3, adding a .NET Core WebJob to Azure, example, how to"
[FIGURE4]: ../images/2017/msdn-1194.png "Figure 4, run a .NET Core console application as a WebJob on Azure"
[FIGURE5]: ../images/2017/msdn-1195.png "Figure 5, WebJob .NET Core logging"
[FIGURE6]: ../images/2017/msdn-1196.png "Figure 6, view the execution of the .NET Core WebJob running on Azure, example"
