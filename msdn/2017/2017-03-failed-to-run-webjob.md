# Failed to run WebJob

It is possible to run an Azure WebJob even when the Azure App Service Web App is not running.  Figure 1 illustrates a running WebJob while the Web App is not running.

![running a WebJob when the Web App is stopped][FIGURE1]
###### Figure 1, running a WebJob when the Web App is stopped

Notice that there is only 1 W3WP process shown in the Process Explorer viewed from the KUDU console, explained in more detail here.  Notice that there is an SCM next to the w3wp.exe that confirms it is indeed the KUDU\SCM process that runs the WebJob.  The fact that there is no ofhter W3WP process means that the Web App is stopped or it has timed out and was shutdown.

In contrast, with Figure 2, you see that a WebJob is running in parallel with a Web App, notice the 2 W3WP processes.

![running a WebJob and a Web App at the same time][FIGURE2]
###### Figure 2, running a WebJob and a Web App at the same time

This behavior may not be desired, I.e. you might want the WebJobs to stop running when the Web App is stopped.

## Stop WebJobs from running when the Web App is stopped

Having WebJobs continue to run even when the the Web App is stopped may not be desired and can be disabled by setting the WEBJOBS_STOPPED setting as described here.

Simply stopping the Web App will not result in the WebJobs being ‘unscheduled’ or stopping.  You must add the WEBJOBS_STOPPED setting manually to stop the WebJobs.

The WEBJOB_STOPPED configuration is made to the Application Setting as shown in Figure 3.

![stop webjobs when the web app is stopped][FIGURE3]
###### Figure 3, stop webjobs when the web app is stopped

Watch out, because if you forget to set it back to 0 or remove it, WebJobs will not run and will result in the ‘Failed to run WebJob’ message as shown in Figure 4.

![failed to run WebJob][FIGURE4]
###### Figure 4, failed to run WebJob

[FIGURE1]: ../images/2017/msdn-1173.png "Figure 1, running a WebJob when the Web App is stopped"
[FIGURE2]: ../images/2017/msdn-1174.png "Figure 2, running a WebJob and a Web App at the same time"
[FIGURE3]: ../images/2017/msdn-1175.png "Figure 3, stop webjobs when the web app is stopped"
[FIGURE4]: ../images/2017/msdn-1176.png "Figure 4, failed to run WebJob"
