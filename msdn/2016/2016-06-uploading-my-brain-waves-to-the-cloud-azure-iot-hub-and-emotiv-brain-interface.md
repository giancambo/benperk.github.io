# Uploading my brain waves to the cloud, Azure IoT Hub and Emotiv brain interface

I have been reading a lot about brain interfaces and that the Tesla S can be summoned with the brain and that people have started having competitions with drones controlled by brain waves.  I have recently acquired an Emotiv Insight® as shown in Figure 1 and have been doing some testing with it.

![brain interface Emotiv Insight® Microsoft Azure][FIGURE1]
###### Figure 1, brain interface Emotiv Insight® Microsoft Azure

It really does work pretty good.  I was able to get it up and running in less than an hour and accessing the online tool (cpanel.emotivinsight.com/BTLE) allowed me to fit the device on my head making sure all the sensors were connected to my brain, as shown in Figure 2.

![brain interface connectivity monitor][FIGURE2]
###### Figure 2, brain interface connectivity monitor

After being certain that all the connectors were functioning, I started watching my brain wave patterns and trying to control them, as shown in Figure 3.

![controlling my brain waves through a brain interface][FIGURE3]
###### Figure 3, controlling my brain waves through a brain interface

Notice towards the end of the graph you can see two peaks circled in green.  I meant to do that,I also meant to lower the level as well.  This means that if I can somehow get a numerical or binary output of that graph then I could code a method to capture that peak, and when it happens, simply trigger an event which runs a method.  The method could be summoning a car or controlling a drone.  And guess what, there is an SDK that does just that on GitHub, [here][LINK1].  For starters, I have been able to install and configure the SDK and output my brain activity to a console, as shown in Figure 4.

![rendering my brain waves to a command prompt][FIGURE4]
###### Figure 4, rendering my brain waves to a command prompt

That is pretty cool I must say.  I haven’t yet decided what I will control with my brain, I thought about hooking it up to a Raspberry Pi and making it change colors or I read something about household lights which you can buy that are controlled by a phone app and are connected to WiFi, I think interfacing with that and turning the lights on and off with my brain would be pretty cool.

## Using the Azure IoT Hub to load brain waves for Machine Language analysis via Stream Analytics

One thing I am actually working on is interfacing the brain interface with the cloud, specifically with Azure.  I am using the Azure IoT Hub which is discussed [here][LINK2].   The thing is, I [learned][LINK3] some stuff about myself while I slept with the band, but I only learned it because my data was shared and compared to others in the collective sense.  It is likely the collection and storage of brain activity is happening now perhaps using the Emotive CPANEL or one of their apps, but not as wide spread or in a large scale.  Microsoft is having a Brain Signal Decoding competition described [here][LINK4], also there are some videos describing some machine learning work [here][LINK5] and [here][LINK6] specific to brain waves.  Although making a device change colors and turning lights on/off is cool, I plan to focus my efforts towards the application of ML on my personal and hopefully the collected brain waves of others.

At some point in the future I will share the code for the capturing of the brain waves from the brain interface, as well as the code I used to upload it to the cloud for analysis using the machine learning technologies.

As shown in Figure 5, I logged a little over 6500 brain wave elements during the time it took to write the article,  I don’t know if that is a lot or not or how long it should take to write an article…

![my brain waves uploaded while writing this article][FIGURE5]
###### Figure 5, my brain waves uploaded while writing this article

Figure 6 shows a portion of my brain activity while writing this article.

![my brain graph while writing this article][FIGURE6]
###### Figure 6, my brain graph while writing this article

It was my brain not me! #EmotivInsight #BrainInterface #AzureIoT

-------------------------------------------------------

Originally I wrote the following and placed it at the top of the article, but I thought about the message I wanted to get across and that was, that it is pretty easy to control stuff with your mind.  The below is just some of my uneducated thoughts about brains, vibes and how we might respond to them naturally.

Have you ever walked into a room full of people and felt really positive, have you ever done the same and felt negative? Have you ever been sitting in a room and when someone walked in you felt like something bad was about to happen or in contrast, the room became electrified and full of optimism. I think these feelings are triggered by something real, something we commonly refer to as vibes. A vibe, where vibe is short for vibration, are in human context actual neural oscillations, or brain waves transmitted from one human to one or more humans. If you are getting a good vibe when someone walks into a room or getting a bad vibe when walking into a room, one or more people must be sending out that vibe. Chances are, an individual entering a room sending out good vibes contributes the vibe, because the stimulation happens at the time of entrance. In contrast, there is no way to know, when entering a room, if everyone is sending the same vibe (good or bad), if on average you are getting a larger percentage of a specific vibe or if one person has the brain capacity to send a much stronger vibe directly at you.

Take for example that a person enters a room you are in, if at the precise time that individual enters the room, you receive a positive vibe, it is highly probable that it‘s coming from the person who just entered, especially if the vibe was not present prior to the entrance. On the other hand, if you are that person entering the room, the collectiveness of the group magnifies the strength of the vibe, which explains the anxious or nervous feelings so many report when standing in front of a large group of people. Individuals feel the vibes of the group which can be deafening and inhibiting. If there is no vibe trend (random vibes) then it becomes even more complicated to interpret and respond appropriately. It can then be asked if everyone feels the vibe or just the one it’s directed at? If everyone in the room sent a positive vibe towards a specific individual would those vibes be picked up by others (the collective) turning the entire situation upwards, how about downwards? Or, will the vibe only be received by the intended individual.

My objective is to deduce the vibe into something physical or measurable, like the actual neural oscillation, which is a fluctuating electrical impulse stemming from an ionic current within the neurons of the brain. It is possible to read neural oscillations, or brain waves using an Electroencephalography (EEG), it is also possible to stimulate (write) neural oscillations using transcranial direct current stimulation (tDCS). This means that it is possible to not only measure the emotional state of individuals (read), but also to persuade them (write).

[FIGURE1]: ../images/2016/msdn-0930.png "Figure 1, brain interface Emotiv Insight® Microsoft Azure"
[FIGURE2]: ../images/2016/msdn-0931.png "Figure 2, brain interface connectivity monitor"
[FIGURE3]: ../images/2016/msdn-0932.png "Figure 3, controlling my brain waves through a brain interface"
[FIGURE4]: ../images/2016/msdn-0933.png "Figure 4, rendering my brain waves to a command prompt"
[FIGURE5]: ../images/2016/msdn-0934.png "Figure 5, my brain waves uploaded while writing this article"
[FIGURE6]: ../images/2016/msdn-0935.png "Figure 6, my brain graph while writing this article"

[LINK1]: https://github.com/Emotiv
[LINK2]: https://azure.microsoft.com/en-us/documentation/articles/iot-hub-csharp-csharp-getstarted/
[LINK3]: 2016-03-what-i-learned-from-sleeping-with-the-band.md
[LINK4]: https://gallery.cortanaintelligence.com/Competition/Decoding-Brain-Signals-2
[LINK5]: https://channel9.msdn.com/Series/FWTV-on-9/Decoding-Brainwaves-with-Azure-Machine-Learning
[LINK6]: https://channel9.msdn.com/Series/FWTV-on-9/Getting-Started-in-the-Decoding-Brain-Signals-Competition
