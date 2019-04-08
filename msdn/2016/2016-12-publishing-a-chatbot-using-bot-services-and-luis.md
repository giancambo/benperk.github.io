# Publishing a chatbot using Bot Services and LUIS

I wrote an article about how to build a #Bot using the Bot Service in Azure [here][LINK1] and another article about how to answer the 10 most common #chatbot questions [here][LINK2].  These are questions that all #chatbots need to answer, that is a very short baseline of what a chatbot needs to respond to.

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

I setup the Web Chat interface here.  It does not do anything yet but respond to those 10 questions.  What I will need to do next is save the contextid and create some LUIS entities so that the response from the question can be properly responded to, but that is for the future, i will document them.  To be successful at that, I am hoping to get some usage on the LadyBug.ai chatbot to learn more questions which are being asked or follow up / follow on questions.  If that happens, then I will share what I see in LUIS and the actions I take to improve the 'intelligence' of LUIS and additionally, how I utilize this intelligence in my chatbot from a C# code perspective.

The Web Chat channel is already enabled, you just need to click on the Channels tab and then the Edit button as shown in Figure 1,.

![enable the Web Chat channel for a chatbot using Bot Services in Azure][FIGURE1]
###### Figure 1, enable the Web Chat channel for a chatbot using Bot Services in Azure

When you click the Edit button it will open a new page like that shown in Figure 2.  You simply need the Secret Key and the iframe code which you can use in an HTML page to reference your chatbot.

![enable a chatbot using the Bot Services in Azure][FIGURE2]
###### Figure 2, enable a chatbot using the Bot Services in Azure

Once the iframe is embedded into your HTML page you can chat with it, which would look something similar to Figure 3.

![create a chatbot in under 1 hour][FIGURE3]
###### Figure 3, create a chatbot in under 1 hour

That’s it for now, once I get more time I will turn LadyBug.ai into a real friendly chatbot.

#AI #ArtificialIntelligence #Cognitive #BotServices #Azure

[FIGURE1]: ../images/2016/msdn-1000.png "Figure 1, enable the Web Chat channel for a chatbot using Bot Services in Azure"
[FIGURE2]: ../images/2016/msdn-1000.png "Figure 2, enable a chatbot using the Bot Services in Azure"
[FIGURE3]: ../images/2016/msdn-1001.png "Figure 3, create a chatbot in under 1 hour"

[LINK1]: 2016-11-create-a-bot-using-an-azure-app-service-bot-service.md
[LINK2]: 2016-11-most-common-chatbot-questions-and-how-to-answer-them.md
