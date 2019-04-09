# C# Bot Builder Samples on GitHub

A good way to get ideas and some coding best practice is to look at some examples, there are many of them using C# hosted on GitHub here.

I wanted to walk through what they are so I can for myself find or discover some new ideas and best practices for this, as I am interested in creating a #chatbot like I discussed in these links

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

Some samples, the name and my description of them

| Title	| Location	| Description
| ---------- | --------- | -----------	|
| intelligence-SpeechToText	| SpeechToText | Upload an audio (.wav) file and the audible words are converted to text and provided in a response |
| intelligence-SimilarProducts	| SimilarProducts | Upload a picture of a blue shoe, another image of a similar blue shoe is returned |
| intelligence-LUIS	| LUIS | Send text and the Intent of it is returned |
| intelligence-ImageCaption	| ImageCaption | Upload a picture and the Computer Vision API will return some text describing the contents of the image |
| demo-Search	| Search | An interface into Azure Search for navigating through large amounts of content  |
| demo-ContosoFlowers	| ContosoFlowers | Renders a Card as a welcome message, plus other examples of dialogs and other Card types  |
| core-State | State | Useful for maintaining State which persists data relative to a user conversation |
| core-SendAttachment	| SendAttachment | In the #chatbot interface the client can send the bot an attachment, useful with SpeechToText, ImageCaption, etc…  |
| core-ReceiveAttachment | ReceiveAttachment | If the #chatbot needs to send an attachment back to the client like a .wav, .png, .gif, etc…  |
| core-MultiDialogs	| MultiDialogs | Dialogs are a conversation process, yes/no questions or selection dialog windows |
| core-GetConversationMembers	| GetConversationMembers | Detecting when people join or leave a group conversation, so you can notify others and welcome them |
| core-DirectLine	| DirectLine | Client to client direct communication, perhaps the way to get intelligent bots to speak with each other |
| core-CreateNewConversation | CreateNewConversation | Have a bot initiate the conversation with the client |
| core-ChannelData | ChannelData | Support for using channel specific capabilities like Skype, Facebook, Microsoft Teams, Slack, etc… |
| cards-RichCards	| RichCards | Cards are pictures or emoticons, which are not attachments, with headings and descriptions |
| cards-CarouselCards	| CarouselCards | Send multiple Cards with the left/right navigation arrows allowing client to scroll through |
 
As I am wanting to create a #chatbot I think about the above capabilities and have these findings:

+ If the client uploads a picture of themselves I can use the age and sentiment APIs to learn more about them and make some funny answer, use SendAttachment as well
+ If a client uploads any picture use the ImageCaption to comment on it, use SendAttachment as well
+ If the client enters something that I do not recognize, ask if the client would like me to search the internet or some content, can use Search or SimilarProducts
+ If the client asks for a picture of the #chatbot use ReceiveAttachment to send the client one
+ Use LUIS as the basis for determining the intent of the conversation with the client

These are just some ideas on using some of these examples, perhaps every 100 or so transactions you can present a RichCards to advertise or CreateNewConversation to keep the client engaged and chatting.

#AI #ArtificialIntelligence #Cognitive #BotServices #Azure

[LINK1]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp
[LINK2]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-SpeechToText
[LINK3]: https://github.com/Microsoft/BotBuilder-Samples/blob/master/CSharp/intelligence-SimilarProducts
[LINK4]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-LUIS
[LINK5]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/intelligence-ImageCaption
[LINK6]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-Search
[LINK7]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-ContosoFlowers
[LINK8]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-State
[LINK9]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-SendAttachment
[LINK10]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-ReceiveAttachment
[LINK11]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-MultiDialogs
[LINK12]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-GetConversationMembers
[LINK13]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-DirectLine
[LINK14]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-CreateNewConversation
[LINK15]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-ChannelData
[LINK16]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/cards-RichCards
[LINK17]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/cards-CarouselCards
