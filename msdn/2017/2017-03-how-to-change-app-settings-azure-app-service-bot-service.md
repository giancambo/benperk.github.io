# How to change app settings Azure App Service Bot Service

I have written a number of articles about my experience creating a Bot #Bot #chatbot.  Check them out.

Here are some other more recent articles I have written on this subject:

+ Create a Bot using an Azure App Service Bot Service
+ Most common chatbot questions and how to answer them
+ Publishing a chatbot using Bot Services and LUIS
+ How I tested / debugged my chatbot that I created using the Bot Services on Azure
+ How my chatbot remained statefull using Azure Bot Services
+ C# Bot Builder Samples on GitHub
+ Top 10 must have Phrase List Features for your chatbot or any bot LUIS
+ 1000 must have utterances for your chatbot using LUIS
+ Adding LUIS Entities to my chatbot
+ How I improved my chatbot

If you ever need to change for example the MicrosoftAppId, MicrosoftAppPassword, LuisAppId or the LuisAPIKey, then here is how you do it.

I wanted to change the LuisAppId and the LuisAPIKey so I looked in the portal and found a reference to it in the BasicLuisDialog.csx file in the message folder.  The reference is when building the constructor of the BasicLuisDialog class as shown here.

```
public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(Utils.GetAppSetting("LuisAppId"), Utils.GetAppSetting("LuisAPIKey")))){}
```

I noticed the way in which the LuisAppId and the LuisAPIKey were being captured, I.e. the Utils.GetAppSetting() method which lead me to the understanding that the values were stored in the app settings file.  As seen in Figure 1, click on the Settings tab and then the Open button for the Application Settings.

![how to set applicaton settings for a chatbot #chatbot #bot][FIGURE1]
###### Figure 1, how to set applicaton settings for a chatbot #chatbot #bot

When the Application settings blade opens you can add, update or delete your Bots application settings, including the MicrosoftAppId, MicrosoftAppPassword, LuisAppId or the LuisAPIKey,as seen in Figure 2.

![how to set applicaton settings for a chatbot #chatbot #bot, how to change LuisAPIKey, how to change LuisAppId][FIGURE2]
###### Figure 2, how to set applicaton settings for a chatbot #chatbot #bot, how to change LuisAPIKey, how to change LuisAppId

Cool stuff.

[FIGURE1]: ../images/2017/msdn-1204.png "Figure 1, how to set applicaton settings for a chatbot #chatbot #bot"
[FIGURE2]: ../images/2017/msdn-1205.png "Figure 2, how to set applicaton settings for a chatbot #chatbot #bot, how to change LuisAPIKey, how to change LuisAppId"
