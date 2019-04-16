# How I improved my chatbot

I think a common attribute of a hardcore coder is procrastination.  Up to now I have brainstormed my LadyBug but have not done much coding to support the backend.  Nonetheless, this is probably my last blog on the design of my #chatbot and will start coding it pretty soon.  If you haven’t seen my other articles on this topic, then have a look. #chatbots

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

As a review, I have discussed:

+ How I created the chatbot
+ Most common English questions and how to answer them with LIUS and C#
+ How to Publish the chatbot
+ How to Test and Debug it (hardcore)
+ Some LUIS enhancements like:
  + Phrase Lists
  + Utterance upload
  + Entities
+ Designing theBrain(LuieResult result) method (discussed in this article)

Here is what I am going to do:

+ Answer questions that match only a single Intent and no entity
+ Answer questions that match multiple (2) Intents and no entity
+ Answer questions that match only a single Intent and only a single entity (multiple Intent (2) to a single Entity)
+ Answer questions that match multiple (2) Intents and multiple entities (2)
+ Answer questions that match multiple (2) Intents, multiple entities (2) and consider the previous Intents and Entities
+ Infinity…

Let’s walk through it.

## Answer questions that match only a single Intent and no entity

As you can see in this article, I have already done that.  As shown in Figure 1, based on the single intent returned from LUIS I have an answer to the question.  This is limited, I know.

![answer questions that match only a single Intent and no entity #chatbot][FIGURE1]
###### Figure 1, answer questions that match only a single Intent and no entity #chatbot

## Answer questions that match multiple (2) Intents and no entity

As you may have learned by setting up the debugging platform discussed here, That it’s a good place to get information about how to reference the attributes of the JSON file returned from LUIS.  What I am thinking is that in the methods shown in Figure 1, will update them to check and see if the Score attribute of the second Intent within the LuisResult class is > .50-.60.   If yes, then I will use both to answer a question specific to those 2 Intents.  Within the single Intent method, I could add this statement and call theBrain() method if it matches.

```
if (result?.Intents[0]?.Intent != null && result?.Intents[1]?.Score > .60 )
{
  theResponse = theBrain(result);
}
```

Since I have 11 Intents, there can be 100 combinations.  Using Excel I simply took each of the Intents and mapped them to each of the other Intents, it worked out looking something like that in Figure 2.

![multiple LUIS intent mapping for a #chatbot][FIGURE2]
###### Figure 2, multiple LUIS intent mapping for a #chatbot

So, what needs to happen is to add some logic into theBrain() method to look at the 2 intents, see what they are and answer the question appropriately.

```
if (firstIntent == "Location" && secondIntent == "Time")
{
 theResponse = "That is a really nice place to visit, but I do not have the time.";
}
```

## Answer questions that match only a single Intent and only a single Entity

Using Excel again, I mapped all my Entities to an Intent, which resulted in the following, shown in Figure 3.  This give us another 100 possible answers to questions, now we are up to 200 total unique responses to questions asked of the #chatbot.

![LUIS Intent to Entity mappings for a #chatbot][FIGURE3]
###### Figure 3, LUIS Intent to Entity mappings for a #chatbot

Since there should always be at least one Intent, that is the place where the code needs to be added to look for the other Intents and Entities.  What can be done here is to add this check to see if the there is an Intent and an Entity.  You could also check to see if there is a second Intent.

```
if (result?.Intents[0]?.Intent != null && result?.Entities[0]?.Type != null )
{
  theResponse = theBrain(result);
}
```

Then, in theBrain() method look for all the possible scenarios and respond appropriately.

```
if (firstIntent == "Location" && firstEntitiy == "Bad")
{
 theResponse = "I have never been there, but consider someplace else.  I'm just saying.";
}
```

## Answer questions that match multiple (2) Intents and multiple entities (2)

Now it really starts rocking hardcore.  First, I mapped the Entities (2), see Figure 4 like I did with the Intents in Figure 2.

![multiple LUIS entity mapping for a #chatbot][FIGURE4]
###### Figure 4, multiple LUIS entity mapping for a #chatbot

A single Entity mapping (remember there are 10) to a single Intent mapping (remember we have 11) results in about 90 more unique responses that can be sent from the #chatbot.  Up to 290 total now.  See Figure 5.

![LUIS entity mapping to a LUIS Intent for a #chatbot][FIGURE5]
###### Figure 5, LUIS entity mapping to a LUIS Intent for a #chatbot

For each Location/Time Intent pair there will be a total of 10 relationship models like in Figure 5.  That would mean a total of 900 unique responses per Intent pair, getting us to 1190 possible unique responses.  That is for 1 Intent pair, there are 10 intent pairs, as shown in Figure 2.  Which would mean (10 x 900) + 290 = 9290 possible responses from my #chatbot.

The code from the default LUIS method could be something like this.

```
if (result?.Intents[0]?.Intent != null && result?.Intents[1]?.Score > .60 && 
    result?.Entities[0]?.Type != null result?.Entities[1]?.Score > .60)
{
  theResponse = theBrain(result);
}
```

And theBrain() method would respond like.

```
if (firstIntent == "Location" && secondIntent == "Time" && firstEntitiy == "Bad" && secondEntity == "Interesting")
{
 theResponse = "I have never been there.  You are keeping my very busy.  But what I hear is it's attociously absorbing ";
}
```

## Infinity…
It is actually a mind-blowing pattern and there is really no end, I mean with only a few mappings and relationship we can respond with almost 10000 unique responses, with many, many more options.

Just for fun I created Figure 6 that illustrates how the mapping of multiple (2) Intents to multiple Entities (2) would look like.  Note that it is only partially complete as I ran out of colors and time.  The point is that there are a lot of combinations and a lot of responses that can be sent back from the #chatbot.  I wonder if there are existing libraries or algorithms that already have these sentences…well, most likely not for the Intents and Entities I have chosen.

![LUIS multiple (2) intent and multiple (2) entity mapping to a LUIS Intent for a #chatbot][FIGURE6]
###### Figure 6, LUIS multiple (2) intent and multiple (2) entity mapping to a LUIS Intent for a #chatbot

I was hoping to find some cool hidden pattern in my map like is discussed in this TED video “Math is the hidden secret to understanding the world” or maybe a Fibonacci number or pattern, but no such luck, but I didn’t finish the mapping so maybe...  This was fun and I hope I get the chance to finish this project and actually code it.

[FIGURE1]: ../images/2017/msdn-1163.png "Figure 1, answer questions that match only a single Intent and no entity #chatbot"
[FIGURE2]: ../images/2017/msdn-1164.png "Figure 2, multiple LUIS intent mapping for a #chatbot"
[FIGURE3]: ../images/2017/msdn-1165.png "Figure 3, LUIS Intent to Entity mappings for a #chatbot"
[FIGURE4]: ../images/2017/msdn-1166.png "Figure 4, multiple LUIS entity mapping for a #chatbot"
[FIGURE5]: ../images/2017/msdn-1167.png "Figure 5, LUIS entity mapping to a LUIS Intent for a #chatbot"
[FIGURE6]: ../images/2017/msdn-1168.png "Figure 6, LUIS multiple (2) intent and multiple (2) entity mapping to a LUIS Intent for a #chatbot"
