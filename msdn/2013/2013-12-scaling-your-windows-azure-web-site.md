# Scaling your Microsoft Azure App Service Web App

Having spent many years supporting web sites running on IIS one of the most challenging aspects was managing load.  I was able to plan for near-term growth by tracking the CPU, Memory, total number of requests, etc… over time and once the utilization of these elements exceeded my threshold for what I considered manageable, it was time to add a new server to the web farm.  This activity took some time, so its instigation started some months before it was going to be actually needed.

Adding physical servers have a considerable cost, not only from the acquisition, but also from the ongoing maintenance and configuration.  Therefore, it was never an option to simply add the maximum amount of possible hardware and wait for the users and customers to start using it.  We had to wait until we needed it before we could request and implement it.

Although, I was pretty good at planning and getting hardware in place to manage near-term growth, the management of bursts was always very challenging.  It became apparent that I needed more capacity between 09:30-12:00 and 13:30-18:30 than I need at any other point in the working day, because those are the time I found that most people using my systems were working.  If there was only a way to increase my server capacity during those time periods where usage was high and then reduce the server capacity when usage was lower.  This, at least while I was doing system administration was not available…  This is why I am so amazed by the scaling capabilities available with a Windows Azure Web Site.

In the Windows Azure management console, click on one of your Web Sites and then on the SCALE view or click on the Configure Autoscale link from the DASHBOARD.  If you Web Site is in Standard mode you will see the configuration options shown in Figure 1.

![default scaling options for Microsoft Azure Web App][FIGURE1]
###### Figure 1, default scaling options for Microsoft Azure Web App

There are 2 options for scaling as shown in Figure 1.

+ Based on a Schedule
+ Based on CPU usage

## Scaling based on a Schedule

To enable the scaling using based on a Schedule, click on the ‘set up schedule times’ button shown in Figure 1 and you are presented a windows similar to that shown in Figure 2.

![Scheduled scaling time in Microsoft Azure App Service Web App][FIGURE2]
###### Figure 2, Scheduled scaling time in Microsoft Azure App Service Web App

***Recurring schedules*** - These options let you define different scale settings during nights or weekends. After you choose the different schedules that you want, click the checkmark. If you want to change your schedules later, click Set up schedule times.

***Different scale settings for day and night*** - This creates two schedules. The first schedule runs from the start of the day to the end of the day. The second schedule runs from the end of one day to the start of the next day, and uses the options below to define times and the time zone.

***Different scale settings for weekdays and weekends*** - This creates a separate profile that starts on Friday evening and ends on Monday morning. Use the options below to define start times, end times, days, and time zone.

***Specific dates*** - Define special events that override your other scale settings.

This is very cool and something I was always looking for as a system administrator.  With these settings you can scale your system when you know your users and customers are going to be using it.  As well, if you know that a new program is being rolled out or a marketing campaign is happening that will increase load on a specific day, then you can plan for that specific event.

## Scaling based on CPU utilization

You can also scale based on the CPU utilization.  As shown in Figure 3, after clicking on the CPU button, the ability to select the number of instances to scale to when the CPU utilization on your web sites hits between a certain amount are enabled.

![Scale a Microsoft Azure Web App based on CPU utilization][FIGURE3]
###### Figure 3, Scale a Microsoft Azure Web App based on CPU utilization

Based on your expected load for the given event or during a specific time period, you can increase the number of instances your website will run with, when the utilization of the CPU hits the Target CPU threshold.

You can view the Autoscale Operation Logs, the link is provided on the DASHBOARD to see how your system is scaling.  This is helpful for fine tuning the Instances and Target CPU settings.

## Conclusion

This scaling feature is something that can save a lot in terms of cost and customer satisfaction.  No longer do you need to purchase and rollout hardware yourself and no longer will you have expensive hardware running idle or underutilized, waiting for a burst of traffic.  You can now be ready for large amounts of traffic with just a simple configuration.

[FIGURE1]: ../images/2013/msdn-0316.png "Figure 1, default scaling options for Microsoft Azure Web App"
[FIGURE2]: ../images/2013/msdn-0317.png "Figure 2, Scheduled scaling time in Microsoft Azure App Service Web App"
[FIGURE3]: ../images/2013/msdn-0318.png "Figure 3, Scale a Microsoft Azure Web App based on CPU utilization"
