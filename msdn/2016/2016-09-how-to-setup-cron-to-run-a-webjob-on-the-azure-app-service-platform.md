# How to setup CRON to run a WebJob on the Azure App Service platform

I wrote an article [here][LINK0] that discussed a Hello World WebJob. The WebJob I use in this example can be downloaded [here][LINK1] and I discuss [here][LINK2]. [Here][LINK3] is an additional resource for setting up CRON to run a WebJob.

The requirements for this are:

+ Your App Service must run in either a Standard or Premium plan
+ Always On must be enabled
+ Place a setting.job file at the root of your WebJob

The following explains how to achieve the previous 3 requirements.

## App Service running either Standard or Premium

You can see the different App Service Plans [here][LINK4]. If your App is already deployed to the Azure App Service platform and it is not running in either of those plans, you must scale to that plan.

## Always On must be enabled
Enable Always On as discussed in Video 2.

## Place a setting.json file at the root of your WebJob

The following steps are required to complete this action:

+ Add the settings.job JSON file to your WebJob directory
+ Configure the CRON schedule
+ Publish the settings.job file
+ Monitor the job

Add a settings.job file to your WebJob as shown in Figure 1.

![add settings.job file to your WebJob for scheduling a CRON][FIGURE1]
###### Figure 1, add settings.job file to your WebJob for scheduling a CRON

The CRON configuration must comply to this format and is composed of 6 fields: {second} {minute} {hour} {day} {month} {day of the week}. You can get more information about this [here][LINK5]. I would like to run my job every 5 minutes and therefor my configuration looks something like that shown in Listing 1.

###### Listing 1, CRON schedule for running a WebJob every 5 minutes

```
{
    "schedule": "0 */5 * * * *"
}
```

Once complete, the project would look similar to that shown in Figure 2.

![where does the settings.job file go for scheduling a WebJob using CRON][FIGURE2]
###### Figure 2, where does the settings.job file go for scheduling a WebJob using CRON

To publish the settings.job file, right-click on the Web Site project and select Publish Web App. Once successful you can access KUDU, which I discus [here][LINK6] and see that the file is present, as shown in Figure 3.

![how to check if my settings.job JSON file exists for running a WebJob using CRON][FIGURE3]
###### Figure 3, how to check if my settings.job JSON file exists for running a WebJob using CRON

Once the CRON configuration is completed and deployed, you can view the logs as discussed in Video 3.

Once you are in KUDU, you can watch the status of the CRON execution and also click on any of the exceptions and view the logs, if you wrote any, similar to that in Figure 4.

![view logs and execution times of a scheduled WebJob][FIGURE4]
###### Figure 4, view logs and execution times of a scheduled WebJob

You can download the source code from [here][LINK7].

***NOTE***: If you schedule a WebJob to run every minute, you need to make sure that it completes within that minute to avoid contention.

[FIGURE1]: ../images/2016/msdn-0951.png "Figure 1, add settings.job file to your WebJob for scheduling a CRON"
[FIGURE2]: ../images/2016/msdn-0952.png "Figure 2, where does the settings.job file go for scheduling a WebJob using CRON"
[FIGURE3]: ../images/2016/msdn-0953.png "Figure 3, how to check if my settings.job JSON file exists for running a WebJob using CRON"
[FIGURE4]: ../images/2016/msdn-0954.png "Figure 4, view logs and execution times of a scheduled WebJob"

[LINK0]: ../2015/2015-06-helloworld-webjob-on-microsoft-azure-web-app.md
[LINK1]: https://code.msdn.microsoft.com/A-slow-WebJob-used-for-14f9ef4c
[LINK2]: ../../waws/2016/2016-02-troubleshooting-a-hung-or-long-running-webjob.md
[LINK3]: https://azure.microsoft.com/en-us/documentation/articles/web-sites-create-web-jobs/#CreateScheduledCRON^
[LINK4]: https://azure.microsoft.com/en-gb/pricing/details/app-service/
[LINK5]: https://azure.microsoft.com/en-us/documentation/articles/web-sites-create-web-jobs/#CreateScheduledCRON
[LINK6]: ../2014/2014-03-using-kudu-with-windows-azure-web-sites.md
[LINK7]: https://code.msdn.microsoft.com/Azure-App-Sample-WebJob-01161df9
