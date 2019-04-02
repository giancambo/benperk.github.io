# How to get started with using LUIS, Language Understanding Intelligent Service, Skype Bot

This article is about consuming the LUIS API from a C# console app as seen in Figure 1.  Instructions on how to configure and setup LUIS can be found [here][LINK1].

***NOTE:*** You might want to look at Microsoft.Bot.Builder.Luis and Microsoft.Bot.Builder.Luis.Models which provide a nice interface to LUIS.

The source code can be downloaded [here][LINK2] as well as the exported LUIS App. You can import it and change the keys to get up and running.

![getting started and consuming a LUIS API for a BOT][FIGURE1]
###### Figure 1, getting started and consuming a LUIS API for a BOT

LUIS returns a JSON file which I dump out in the console seen in Figure 1.  This example is a single scenario where I want to check if there were any reported platform issues for a given Microsoft Azure App Service Web App.  The questions/comments follow:

***Q:  My web app was down.***  – from a support perspective this would not be enough to check and see if there was a platform issue or not.  Therefore I need to configure LUIS to have some actionParameters, such as WhenWasIt and WhatRegion, shown in Figure 1 in red.  Notice that the values of those actionParameters are NULL.  This tells LUIS to ask for them or that they are required before LUIS can consider the question complete and mark it as finished as shown in orange.

***A: When did you experience the issue?*** – there are numerous built in entities, here I use the datetime entity which trains LUIS to understand the aspects of time.  Enter *last Tuesday’ and LUIS can translate into a specific date.  Pretty cool.

***Q: What region was it it?*** – now I need to know where the possible downtime happened and entered the region of the Web App.  As soon as I get FINISHED, then I can access my data source to see if there were any reported downtimes and also provide some next steps to the person or BOT communicating with my API.

You can then put the API into a Skype Bot as shown in Figure 2.  How to create a Skype Bot is described [here][LINK3].

![interfacing LUIS with a Skype Bot][FIGURE2]
###### Figure 2, interfacing LUIS with a Skype Bot

## Back to the C# code.

In the Main() method of my console application, it took me some time to figure it out, but is became apparent that I had to concatenate the answers to my questions and then resend the entire question back to LUIS for it to mark my actionParameters as true.  That was the SPARK that moved my progress forward and made the project successful.  Here was the pseudo data I sent to LUIS:

Q:  My web app was down – SENT TO LUIS:  my web app was down

Q:  When did you experience this issue?  A: last Monday – SENT TO LUIS: my web app was down last Monday

Q:  What region was it in?  A: in AM2 – SENT TO LUIS: my web app was down last Monday in AM2

This was the way I got my scenario to finish, perhaps there is a different way, but this one worked for me.  I am interested if you find other ways.  If you do, please share it with me and leave a message, below.

See the following code snippet, also included in the downloadable code from here.

Code Snippet:

```
while (luisResponse?.dialog?.prompt?.Length > 0)
{
  Write(luisResponse.dialog.prompt + " ");
  contextId = luisResponse.dialog.contextId;
  question = question + " " + ReadLine();
  Task.Run(async () =>
  {
    WriteLine();
    luisResponse = await askLUIS(question, contextId);
    WriteLine(JsonConvert.SerializeObject(luisResponse));
    WriteLine();
  }).Wait();
}
```

[FIGURE1]: ../images/2016/msdn-0941.png "Figure 1, getting started and consuming a LUIS API for a BOT"
[FIGURE2]: ../images/2016/msdn-0942.png "Figure 2, Feature"

[LINK1]: https://www.luis.ai/Help
[LINK2]: https://code.msdn.microsoft.com/LUIS-Language-Understanding-acf9720a
[LINK3]: https://docs.botframework.com/en-us/skype/getting-started/
