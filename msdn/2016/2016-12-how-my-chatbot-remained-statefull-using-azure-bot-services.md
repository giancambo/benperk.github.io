# How my chatbot remained statefull using Azure Bot Services

One of the challenges I had, well maybe not a challenge, but it took some hours to think it through, with making my #chatbots #chatbot personality was coding in the ability to recall the last question, last #LUIS Intent or #LUIS Entity so that I could reference it for making better decisions on how to answer the next question.  In reality, although I store this ‘last question’ in a simple string, the questions, intents and entities could be stored into a class or a query-able collection, for example.  The questions, intents and entities could also be stored on a database and referenced, or you could require users to register and store everything based on a user id and build profiles and a #chatbot specifically designed for that person.  What can be done is only limited by you own imagination, and in some cases training and experience.

Here are a few previous articles I have written in regards to making LadyBug.ai:

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

Anyway, here is my simple way I found to store and retrieve the last question, intent and entity and hope you can improve and build upon this.

+ Modify the methods which respond based on the LUIS Intent
+ Create the Get and Set methods for retrieving and storing the previous information
+ Prove it works

Once I built the debugging environment I was able to set break points and learn about what properties where available to me.  Like what is illustrated in Figure 1.

![list out the properties for a LUIS response #chatbot, bot services][FIGURE1]
###### Figure 1, list out the properties for a LUIS response #chatbot, bot services

## Modify the methods which respond based on the LUIS Intent

I have a method that responds to a LUIS Intent called State, here is how that method looks like when I update it to store and retrieve the state:

```
[LuisIntent("State")]
public async Task State(IDialogContext context, LuisResult result)
{
   var currentIntent = result?.Intents[0]?.Intent;
   var previousIntent = GetPreviousContext(context);
   SetContext(context, result);
   await context.PostAsync($"I am very happy, even more happy now that you ask and care.  How are you?");               
   context.Wait(MessageReceived);
}
```

Notice the GetPreviousContext() method that sends the IDialogContext  and the SetContex() which is sent the IDialogContext and the LuisResult.  As shown in Figure 1 before you can look into what properties are accessible and determine for yourself if you need them.

## Create the Get and Set methods for retrieving and storing the previous information

Here is the GetPreviousContext() method.  Yes I understand that there are no entities referenced, but I think you get the point and can update the code to pass the result and walk through them as required.  Yes, I will also share the ContextConstants class further down.

```
public string GetPreviousContext(IDialogContext context)
{
  string previousIntent;
  context.ConversationData.TryGetValue(ContextConstants.LastIntent, out previousIntent);
  return previousIntent;
}
```

Here is the SetContext() method, that sets the value so I can retrieve it when the next question comes in from the client.

```
public void SetContext(IDialogContext context, LuisResult result)
{
   context.ConversationData.SetValue(ContextConstants.LastIntent, result.Intents[0].Intent);
}
```

And here is the ContextConstants class.  I learned about this class and how it works by looking at the core-State example on GitHub that I discuss [here][LINK1].  You can see the code [here][LINK2].

```
public class ContextConstants
{
    public const string LastIntent = "Intent";             //the intent
    public const string LastIntentActions = "Actions";
    public const string LastEntity = "Entity";             
    public const string LastEntityResolution = "EntityResolution";  
    public const string LastEntityRole = "EntityRole";        
    public const string LastEntityType = "EntityType";         //the entity as per LUIS
    public const string LastQuery = "Query";              //the last question
}
```

As you can see from the ContextConstants class, I am ready for the Entities, Intents and question, but I just have not coded all of them yet.  All of this so far is just proof of concept for me.

## Prove it works

I startup my debugging environment which I built following [these][LINK3] instructions and started the debughost as shown in Figure 2.

![starting the debug environment for a #chatbot #debugging][FIGURE2]
###### Figure 2, starting the debug environment for a #chatbot #debugging

Next, start the Microsoft Bot Framework Emulator, as shown in Figure 3.

![starting the Bot Emulator for debugging a chatbot][FIGURE3]
###### Figure 3, starting the Bot Emulator for debugging a chatbot

Start the debugger session in Visual Studio and enter two questions and look into the Locals to see the values ready for access and usage to better answer the questions or respond to comments.  As expected, as seen in Figure 4 the first question, when sent to LUIS returns the Intent of “State” and the seconds question Intent is “None”.

![showing the values stored in State for the #chatbot][FIGURE4]
###### Figure 4, showing the values stored in State for the #chatbot

Now I can code my chatbot a little better to respond to questions using the previous intents, entities, questions, etc.… in a hope that a personality develops that is fun.

## Conclusion

I have 2 more posts in the works on the #chatbot topic.  One is some information about how I selected my Entities and another on how I did some design to improve a specific question.  I think once the founding principles are created/set, like I have done with these ~8 articles, the rest is based simply on hard work, creativity and philosophy.  What an exciting set of features I am so excited to see how this area develops.

A point I made in my first article is that although #chatbots can and do provide a really good interface to answer simple questions and maybe even make the ordering process better, unless the #bot has a personality, then the process will become mundane, so make it useful for sure, but make it fun and keep people coming back to see how their virtual friend has evolved.  Funny thing and maybe a goal, if your #chatbot serves a purpose or sales things, it would be interesting if people chat with the bot for reason other than the purpose for which it exists.  If yes, then you can put reminders every once and a while about the objective, but still remain fun, too cool.

My LadyBug.ai will be fun and can be, one day a personality used in those other bots.

#AI #ArtificialIntelligence #Cognitive #BotServices #Azure

[FIGURE1]: ../images/2016/msdn-1061.png "Figure 1, list out the properties for a LUIS response #chatbot, bot services"
[FIGURE2]: ../images/2016/msdn-1062.png "Figure 2, starting the debug environment for a #chatbot #debugging"
[FIGURE3]: ../images/2016/msdn-1063.png "Figure 3, Feature"
[FIGURE4]: ../images/2016/msdn-1064.png "Figure 4, Feature"

[LINK1]: 2016-12-c-bot-builder-samples-on-github.md
[LINK2]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/core-State
[LINK3]: 2016-12-how-i-tested-my-chatbot-that-i-created-using-the-bot-services-on-azure.md
