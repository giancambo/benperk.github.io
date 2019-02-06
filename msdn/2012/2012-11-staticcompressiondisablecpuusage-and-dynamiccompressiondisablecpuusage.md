# StaticCompressionDisableCpuUsage and DynamicCompressionDisableCpuUsage

Performance problems are generally difficult to find the root cause of and fix.  There are so many ‘touch points’ that where to start looking much less finding the root cause while the issue is happening, can be a challenge.

I found these 4 Compression settings very interesting and a system administrator should review them and make sure they are set to values which will avoid performance problems.  Or most importantly, if you are breaching these thresholds, it’s time to add more power to your hardware.  You can find a nice description of each of the below [here][LINK1].

+ staticCompressionDisableCpuUsage
+ staticCompressionEnableCpuUsage
+ dynamicCompressionDisableCpuUsage
+ dynamicCompressionEnableCpuUsage

Figure 1, shows the default settings for these values and where they can be modified if you need to.

![Disable or Enable static or dynamic compression based on CPU usage][FIGURE1]
###### Figure 1, Disable or Enable static or dynamic compression based on CPU usage

The point I want to make is that if you hit or breach the Disable threshold, compression will not be turned back on unless the average CPU usages breaks the Enable threshold…or you recycle the worker process. You may consider increasing the Enable value.

If you are experiencing performance problems which are resolved after you recycle the worker process, take a look at these values and compare them to your system usage statistics to see if there is something that needs tweaking.

As I mentioned before, if you are breaching these threshold, it’s time to look at adding some additional capacity to the server.

[FIGURE1]: ../images/2012/msdn-0188.png "Figure 1, Disable or Enable static or dynamic compression based on CPU usage"

[LINK1]: http://msdn.microsoft.com/en-us/library/aa347461(v=VS.90).aspx
