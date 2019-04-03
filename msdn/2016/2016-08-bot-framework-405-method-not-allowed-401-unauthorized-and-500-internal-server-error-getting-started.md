# Bot Framework 405 Method Not Allowed, 401 Unauthorized and 500, Internal Server Error getting started

I have been working with the Bot Framework available from [here][LINK1]. While I was testing it out I encountered 3 road blocks that I will share just to make it move along faster if you happen to encounter the same.  I followed [these][LINK2] instructions which worked just fine.  The issue I faced were during the testing of the Bot via the Emulator.  I had 3 issues:

+ 405 MethodNotAloowed
+ 401 Unauthorized
+ 500 InternalServerError

## 405 MethodNotAllowed when testing a Bot from the Emulator

As shown in Figure 1, the response from my ‘Hi’ message was a 405.  The issue was on my side and was due to the configuration/setting I had for the Bot Url. 

![405 Method Not Allowed using the Bot Framework Emulator][FIGURE1]
###### Figure 1, 405 Method Not Allowed using the Bot Framework Emulator

I wrote this article [here][LINK3] where I had received a 405 when calling a WebAPI and thought immediately that ‘I got this’.  It actually turned out to be easier than that.

The issue was that the Bot Url was going to the root directory and in the root directory of the solution there is a default.htm file which cannot handle a POST.

The fix was to update the Bot Url to include /api/messages so that the request went to the controller instead of the default.htm file, as shown in Figure 2.

![how to solve 405 using the Bot Emulator][FIGURE2]
###### Figure 2, how to solve 405 using the Bot Emulator

As you can see, however, I then received a 401 Unauthorized response.

## 401 Unauthorized when testing a Bot from the Emulator

In the web.config for the Bot default solution, you will see some default settings for the MicrosoftAppId and MicrosoftAppPassword.  For testing on your localhost only, make sure there are no values in the text boxes for Microsoft App Id and Microsoft App Password, as illustrated in Figure 3.

![app id and app password for a Bot][FIGURE3]
###### Figure 3, app id and app password for a Bot

## 500 Internal Server Error when testing a Bot from the Emulator

Two things:

+ If you want to test your published Bot with the Emulator you need to configure NGROK as per [these][LINK4] instructions
+ If you do not do that you will get this 500 error

Figure 4 shows the error you get when you try and use the Emulator without NGROK configured.

![getting a 500 internal server error with my Bot][FIGURE4]
###### Figure 4, getting a 500 internal server error with my Bot

To troubleshoot this I enabled FREB tracing on my Azure App Service Web App and it immediately made sense to me…as shown in Figure 5, the response is being sent back to the localhost address on port 9000.

![Bot exception 500][FIGURE5]
###### Figure 5, Bot exception 500

Using NGROK I was able to make the Emulator work, nothing too complicated there.

As I am writing a Skype Bot I simply added my Bot to my list of contacts and it also worked as expected.  This is really cool stuff.

![Ladybug bot][FIGURE6]
###### Figure 6, Ladybug bot

[FIGURE1]: ../images/2016/msdn-0943.png "Figure 1, 405 Method Not Allowed using the Bot Framework Emulator"
[FIGURE2]: ../images/2016/msdn-0944.png "Figure 2, how to solve 405 using the Bot Emulator"
[FIGURE3]: ../images/2016/msdn-0945.png "Figure 3, app id and app password for a Bot"
[FIGURE4]: ../images/2016/msdn-0946.png "Figure 4, getting a 500 internal server error with my Bot"
[FIGURE5]: ../images/2016/msdn-0947.png "Figure 5, Bot exception 500"
[FIGURE6]: ../images/2016/msdn-0948.png "Figure 6, Ladybug bot"

[LINK1]: https://dev.botframework.com/
[LINK2]: http://docs.botframework.com/en-us/csharp/builder/sdkreference/gettingstarted.html
[LINK3]: ../2015/2015-07-asp-net-webapi-results-in-a-405-method-not-allowed-http-response.md
[LINK4]: http://docs.botframework.com/en-us/tools/bot-framework-emulator/#using-the-emulator-with-ngrok-to-interact-with-your-bot-in-the-cloud
