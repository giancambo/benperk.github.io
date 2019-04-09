# 1000 must have utterances for your chatbot using LUIS

My #chatbot is intended only to be pleasant, not to be philosophical or answer complicated political or social questions.  As companies create #chatbot to serve a purpose, they should also maintain a level of fun and make sure the #chatbot has at least a limited personality.  Utterances are questions which can be or have been asked to your #chatbot.  These questions or utterances are what get translated into an Intent and bound to an Entity.  Sure, one sentences or utterances can get close probability scores to multiple Intents and be bound to multiple Entities.  If that is the case, your code needs to be smart enough to handle them and respond in an intelligent, fun and witty way.  The code is where the magic and creativity will happen and once I get all the configuration setup here I will share some about the code.  #AI #ArtificialIntelligence #Cognitive #BotServices #Azure

If you have not read my other posts in regards to my #chatbot, please check them out here:

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

To upload the utterances, I did the following:

+ Check to see if any utterances already need my suggestion
+ Upload the utterances
+ Make the suggestions

## Check to see if any utterances already need my suggestion

Login to your LUIS account [here][LINK1] and click on the Edit button for the application you want to add the utterances to, as shown in Figure 1.

![adding 1000 utterances to LUIS][FIGURE1]
###### Figure 1, adding 1000 utterances to LUIS

Once on the main page for the application click on Suggest and make updates.  As seen in Figure 2, there is one and I need to select the Intent from the drop down and select any Entity which the word needs to be bound to.

![updating and training LUIS Microsoft Cognitive Services][FIGURE2]
###### Figure 2, updating and training LUIS Microsoft Cognitive Services

If there are no utterances then you would receive this message.

“There are no utterances from this application's HTTP endpoint which haven't been labeled already. Choose “new utterances” to provide your own examples, or publish your application's HTTP endpoint to receive new utterances.”

## Upload the utterances

Here is a text file with 1000 questions, ok, maybe only 994 or so.  Either way, go back to the main page where the list of your application are and click on Import Utterances, as shown in Figure 3.

![import utterances into your #chatbot][FIGURE3]
###### Figure 3, import utterances into your #chatbot

Once uploaded I received this response window, Figure 4.

![importing the #chatbot utterances][FIGURE4]
###### Figure 4, importing the #chatbot utterances

Click on the Edit button as shown previously in Figure 1 and click on the Suggest tabs as shown previously in Figure 2 as suggested in Figure 4.

## Make the suggestions

After clicking on the Suggest tabs you will see a page similar to that shown in Figure 5.

![maintaining the utterances in LUIS for a #chatbot using Azure Bot Services][FIGURE5]
###### Figure 5, maintaining the utterances in LUIS for a #chatbot using Azure Bot Services

And the last action is then to update them all by selecting the Intent from the drop-down and highlighting and selecting the bound Entity/Entities, similar to that shown in Figure 6.

![#chatbot with LUIS, Microsoft Cognitive Services and Azure Bot Services][FIGURE6]
###### Figure 6, #chatbot with LUIS, Microsoft Cognitive Services and Azure Bot Services

Once you go through and set the Intent and Entities for the ~1000 utterances, Train your application (see Figure 5 –> click the Train button on the lower left), Publish it again (see Figure 6) and go code it and make your #chatbot funny, bright, canny, clever, creative, intelligent, quick, resourceful or everything your would expect an AI personality with a SMART entity to be.

![update and publish your trained AI LUIS model and change the world][FIGURE7]
###### Figure 7, update and publish your trained AI LUIS model and change the world

Happy coding.  I have one more article I will write which will share some of the C# code I wrote to manage and respond to the questions.  Then I will leave the rest to you.  Rock!

Actually, [here][LINK2] are the 1000 questions 🙂

[FIGURE1]: ../images/2016/msdn-1065.png "Figure 1, adding 1000 utterances to LUIS"
[FIGURE2]: ../images/2016/msdn-1066.png "Figure 2, updating and training LUIS Microsoft Cognitive Services"
[FIGURE3]: ../images/2016/msdn-1067.png "Figure 3, import utterances into your #chatbot"
[FIGURE4]: ../images/2016/msdn-1068.png "Figure 4, importing the #chatbot utterances"
[FIGURE5]: ../images/2016/msdn-1069.png "Figure 5, maintaining the utterances in LUIS for a #chatbot using Azure Bot Services"
[FIGURE6]: ../images/2016/msdn-1070.png "Figure 6, #chatbot with LUIS, Microsoft Cognitive Services and Azure Bot Services"
[FIGURE7]: ../images/2016/msdn-1071.png "Figure 7, update and publish your trained AI LUIS model and change the world"

[LINK1]: https://www.luis.ai/
[LINK2]: https://msdnshared.blob.core.windows.net/media/2017/05/1000.zip
