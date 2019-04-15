# Adding LUIS Entities to my chatbot

I have written a number of articles about my #chatbot #chatbots using LUIS and the Azure Bot Service, see the following:

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

This is about adding the LUIS Entities.  I made the decision on these entities after looking around for the most common words used and then took their synonyms.

Bad -	Good
Interesting - Boring
Smart -	Stupid
Important - Irrelevant
Unsure - Certain

There is currently a limit to 10 entities on LUIS but that should be enough for now, especially because we can add so many more words using Phrase Lists as I discussed here.

To add the LUIS Entities, click on the Entities + as shown in Figure 1.

![adding Entities to LUIS for a #chatbot][FIGURE1]
###### Figure 1, adding Entities to LUIS for a #chatbot

Enter the name of the Entity and then click save, you will see something similar to that shown in Figure 2.

![adding Entities to LUIS for a #chatbot][FIGURE2]
###### Figure 2, adding Entities to LUIS for a #chatbot

You can click on the Include children checkbox and add either Hierarchical or Composite attributes.  You can learn more about those here, I am not using them in this example, they see real cool thought.

Once they are all added, add a new utterance that contains the defined Entity as shown in Figure 3, and notice that it is identified and automatically highlighted.

![adding Entities and utterances to LUIS for a #chatbot][FIGURE3]
###### Figure 3, adding Entities and utterances to LUIS for a #chatbot

Submit the utterance by selecting the Submit button.  Select the Train link on the lower left corner of the windows, also shown in Figure 3.  Once complete select the Publish link to test the API.  After clicking Publish you will see a pop-up window similar to that shown in Figure 4.

![testing added Entities and utterances in LUIS for a #chatbot][FIGURE4]
###### Figure 4, testing added Entities and utterances in LUIS for a #chatbot

Press the Enter key and the request is sent to the API and you should receive a response in another window or tab, like the following.

```
{
  "query": "That is very interesting",
  "topScoringIntent": {
    "intent": "Reality",
    "score": 0.830892861
  },
  "intents": [
    {
      "intent": "Reality",
      "score": 0.830892861
    },
    {
      "intent": "None",
      "score": 0.5086148
    },
    ...
],
  "entities": [
    {
      "entity": "interesting",
      "type": "Interesting",
      "startIndex": 13,
      "endIndex": 23,
      "score": 0.8820625
    }
  ]
}
```

Now you can use the Intents and the Entity to respond to the question sent to your #chatbot.

[FIGURE1]: ../images/2017/msdn-1139.png "Figure 1, adding Entities to LUIS for a #chatbot"
[FIGURE2]: ../images/2017/msdn-1140.png "Figure 2, adding Entities to LUIS for a #chatbot"
[FIGURE3]: ../images/2017/msdn-1141.png "Figure 3, adding Entities and utterances to LUIS for a #chatbot"
[FIGURE4]: ../images/2017/msdn-1142.png "Figure 4, testing added Entities and utterances in LUIS for a #chatbot"
