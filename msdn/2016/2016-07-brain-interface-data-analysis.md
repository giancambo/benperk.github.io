# Brain Interface data analysis

I wrote this article here that describes what I am doing.  I am in the process of creating the article that explains in complete detail how I did it.  I expect to complete it by August or September.

The best way to determine if a brain interface can really trigger an event, is by ensuring that the captured data is reflected the same way for a given scenario.  Therefore, I captured my brain waves in the following scenarios:

+ Baseline
+ Listening to music
+ Meditation
+ Reading
+ Watching Media
+ Working at Computer

The set of the 6 scenarios I called a test case and I captured 2 test cases about 1 week apart.  Figure 1 illustrates how the captured data appears when extracted from my SQL Azure database and presented via a Power BI model.

The 3 things that jump out at me are:

1. The Meditation scenarios seem to match up pretty well.
2. The Work at Computer seem to match up pretty well.
3. I need to better focus on the specifics of what I did during the 2 test cases per scenario

![two test cases of brain wave analysis having 6 scenarios, one week apart][FIGURE1]
###### Figure 1, two test cases of brain wave analysis having 6 scenarios, one week apart

The fact that #1 and #2 are similar does not surprise me.  This is because I am pretty sure that I was doing the exact same thing during both test cases for that scenario.

However, the other scenarios I am not so sure, for example:

+ As far as the baseline goes, I am not sure what I actually did the first time, but my mind was very active with thoughts in test case 2.  I cannot recall if that was the same in scenario 1.
+ Was I listening to the same kind of music in both scenarios?
+ Did I read the same kind of literature?  I remember in the first test case I read some news and in the second I read email.
+ In regards to watching media, I watched ~15 minutes of ‘The Martian’ each time, but I did get a disturbance in test case 2 that did not happen in test case 1 and I watched different parts of the movie in each scenario.
+ Was I distracted during any of the scenarios which did not happen the same in both test cases?  I think so.
+ I am pretty sure that in test case 1 that all of the brain interface sensors were not green, I made an effort to get them green at least yellow in test case 2

In both test cases, I removed the top 250 and lowest 250 rows with highest and lowest frequency values.

Two points:

+ I need to make sure the brain interface hardware delivers consistent values for a given scenario
+ For each scenario I need to make certain I am really doing the same, I.e. reading the same, listening to the same, watching the same and most importantly, have the same ‘state of mind’

Finally, the ability to control virtual and non-virtual entities with brain waves not only relies on the hardware and software brain interface capturing the data, it also, and even more so, relies on the individual using the interface and whether the same waves can be sent to match the pattern required to trigger the event.  The more precise we can get the devices to capture brain waves and the more precise we can create them, the better chance of success we have.

#EmotiveInsight #BrainInterface #AzureIoT #Brain

[FIGURE1]: ../images/2016/msdn-0940.png "Figure 1, two test cases of brain wave analysis having 6 scenarios, one week apart"

[LINK1]: https://blogs.msdn.microsoft.com/benjaminperkins/2016/06/27/uploading-my-brain-waves-to-the-cloud-azure-iot-hub-and-emotiv-brain-interface/
