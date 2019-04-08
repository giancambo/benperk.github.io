# Most common chatbot questions and how to answer them

If you haven’t heard about #chatbots #chatbot then consider yourself lucky to have found this page because you are about to learn about them.  They will soon be everywhere and are actually very helpful for answering a majority of questions.  The problem I want to resolve is the baseline, in that there needs to be a standard set of questions that all chatbots can answer, and needless to say, although the chatbot needs to add value and provide the service it is intended for, the real determining factor of the success of your chatbot is that it is fun, has a sense of humor, a personality that engages the customer and tweaks their mind so they remember it and come back.

I wrote [this][LINK1] article that described how I created a #Bot using LUIS on the Azure Portal, quick and easy.  You will need to have done that before you can what I discuss in this post.  Here are some other articles I have written on this subject, just for review:

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

In this article I will describe how to configure LUIS (Language Understanding Intelligent Services) to answer a baseline of some most common chatbot questions.  I will also provide the code for which you can use to answer them.  At some point in the near future I will add the chatbot to the Web Chat channel so I can play with it and train it to answer more questions.  Check back here later to see my updates.

The top 10 questions I believe which all chatbots must answer are:

1. Are you real?
2. What is your name?
3. How old are you?
4. Where do you live?
5. How can you help me?
6. Which languages do you speak?
7. How are you? Are you doing ok?
8. What time is it?
9. What are your hobbies?
10. What do you look like?

Here are the steps I followed to create a #chatbot to answer these 10 most frequently asked questions for a chatbot, in English.

+ Create the Intent for each of the 10 questions in LUIS
+ Create the code that responds to those questions

What a short and easy list, how cool is that.

## Create the Intent for each of the 10 questions in LUIS

As described in my previous article [here][LINK1] where I created the bot, a LUIS application is created when you create a Bot using the Language understanding template.  See Figure 6 of that previous article.

First I want to decide the name of the intents which I will associate to each of the 10 questions, they are shown in the following table, Table 1.


| Question	LUIS Intent name
| ------- | ---------------- | 
| Are you real?	| Reality | 
| What is your name?	| Name |
| How old are you?	| Age |
| Where do you live?	| Location |
| How can you help me?	| Help |
| Which languages do you speak?	| Language |
| How are you? Are you doing ok?	| State |
| What time is it?	| Time |
| What are your hobbies?	| Hobby |
| What do you look like?	| Appearance |

###### Table 1, questions and associated LUIS intent

Next, access your LUIS application list here and click on the Edit link for the application which is linked to the Bot you created in the Azure Portal, similar to that in Figure 1.

![add intents to LUIS for a chatbot that answers the most frequently asked chatbot questions][FIGURE1]
###### Figure 1, add intents to LUIS for a chatbot that answers the most frequently asked chatbot questions

Add each intent by clicking on the + Intents as shown in Figure 2.  Add all the Intent names and the questions from Table 1.

![add the LUIS intent names for the chatbot most frequently asked questions][FIGURE2]
###### Figure 2, add the LUIS intent names for the chatbot most frequently asked questions

After each Intent entry you should be prompted to add the utterance, select the correct Intent, if a different one is selected that required, and press the Submit button, similar to that shown in Figure 3.

![create utterances for the LUIS intents for the chatbot that answers the top 10 most frequent chatbot questions][FIGURE3]
###### Figure 3, create utterances for the LUIS intents for the chatbot that answers the top 10 most frequent chatbot questions

Once all of the intents are added, select the Train button at the bottom of the page, like shown in Figure 4.

![train the bot to answer the chatbots most frequently asked questions][FIGURE4]
###### Figure 4, train the bot to answer the chatbots most frequently asked questions

You can test the Intents from within the LUIS portal by clicking on the Publish link, also shown in Figure 4.

Entering the question, ‘Are you real?’ as shown in Figure 5 and then clicking on the link.

![testing LUIS intents from the LUIS console for a chatbot][FIGURE5]
###### Figure 5, testing LUIS intents from the LUIS console for a chatbot

Results in a JSON response similar to the following:

```
{
  "query": "Are you real?",
  "topScoringIntent": {
    "intent": "Reality",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "Reality",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.03159651
    },
    ...
  ],
  "entities": []
}
```

Notice that the score is highest for the intent of ‘Reality’, just as expected.

## Create the code that responds to those questions

Now let’s go back to the Bot we created in the Azure Portal and add the code, right in the portal, no more Visual Studio and no more Bot Framework required.  See in Figure 6 that you simply add a LuisIntent attribute for each intent and add the response, similar to that shown in Figure 6.

![creating the Bot code for the chatbot in the Azure Portal][FIGURE6]
###### Figure 6, creating the Bot code for the chatbot in the Azure Portal

You can certainly add Prompt and Entity logic into the mix to make things more exciting for your visitor, this is just an example on how to simply answer the top 10 most frequently asked chatbot questions.  And finally, once all of the Intents are added to the Bot code, have some fun and see what happens, something like that shown in Figure 7.

![playing with the chatbot, so cool][FIGURE7]
###### Figure 7, playing with the chatbot, so cool

I am going to setup the Web Chat channel so that the chatbot can be used from a browser, I’ll write another post about that.  Check back later.

#AI #ArtificialIntelligence #Cognitive #BotServices #Azure

[FIGURE1]: ../images/2016/msdn-1037.png "Figure 1, add intents to LUIS for a chatbot that answers the most frequently asked chatbot questions"
[FIGURE2]: ../images/2016/msdn-1031.png "Figure 2, add the LUIS intent names for the chatbot most frequently asked questions"
[FIGURE3]: ../images/2016/msdn-1032.png "Figure 3, create utterances for the LUIS intents for the chatbot that answers the top 10 most frequent chatbot questions"
[FIGURE4]: ../images/2016/msdn-1033.png "Figure 4, train the bot to answer the chatbots most frequently asked questions"
[FIGURE5]: ../images/2016/msdn-1034.png "Figure 5, testing LUIS intents from the LUIS console for a chatbot"
[FIGURE6]: ../images/2016/msdn-1035.png "Figure 6, creating the Bot code for the chatbot in the Azure Portal"
[FIGURE7]: ../images/2016/msdn-1036.png "Figure 7, playing with the chatbot, so cool"

[LINK1]: 2016-11-create-a-bot-using-an-azure-app-service-bot-service.md
