# How (I) would handle a timeout in Azure Function

I wrote this article about adding a host.json file to an Azure Function “How to add a HOST.JSON file to an Azure Function” and while I was breaking it, I saw a behavior which I didn’t expect.
The behavior was that my code within the try…catch… block continued to execute after the timeout, the code in my catch did not execute and a 500 was rendered to my consumer.  I experienced what I see in Figure 1.

![how to handle a timeout in Azure Function][FIGURE1]
###### Figure 1, how to handle a timeout in Azure Function

Adding the log.Info() method into your Azure Function is helpful to see what code is executed and when.  in Figure 1 I see that the log.Info() method triggered even after the Timeout happened eventhough a 500 was returned to the consumer.
I partially resolved this by adding a cancellation token to my Azure Function.  See [here][LINK2] for more on the structure of System.Threading.CancellationToken.
Compare this code with my code I originally wrote [here][LINK3] “How to add a HOST.JSON file to an Azure Function”.

```
using System.Net;
using System.Threading;
 
public static async Task<HttpResponseMessage> 
    Run(HttpRequestMessage req, CancellationToken cancellationToken, 
            TraceWriter log)
{
    log.Info("C# TimeOutHttpTrigger entered Run() method...");
 
    try
    {
        log.Info("C# TimeOutHttpTrigger entered try...");
        await Task.Delay(8000, cancellationToken);
        log.Info("C# TimeOutHttpTrigger success delay of 8 seconds...");
        return req.CreateResponse(HttpStatusCode.OK, "Success");
    }
    catch (System.Exception ex)
    {
        log.Info("C# TimeOutHttpTrigger entered catch...");
        return req.CreateResponse(HttpStatusCode.OK, $"Error: {ex.Message}");
    }
}
```

Notice the following:

+ I added the using directive “System.Threading”
+ I added a CancellationToken to the paramters of my Run() method
+ I added the cancellationToken to my Task.Delay() method

Then when I ran my Azure Function, the code within the try… did stop executing and proceeded into the catch…, as seen in Figure 2.

![how to handle a timeout in Azure Function][FIGURE2]
###### Figure 2, how to handle a timeout in Azure Function

As you can see that the log.Info() method which came after the Task.Dely() method did not run, instead, the log.Info() method within the catch… was run.
The interesting point is that I still received a 500 when I consumed it eventhough in my catch… I wanted it to return an HttpStatusCode.OK which would have been a 200.  I can work with this for now, if someone knows how to get my desired response back, please leave a comment.

[FIGURE1]: ../images/2018/msdn-0063.png "Figure 1, how to handle a timeout in Azure Function"
[FIGURE2]: ../images/2018/msdn-0064.png "Figure 2, how to handle a timeout in Azure Function"

[LINK1]: tbd
[LINK2]: https://msdn.microsoft.com/en-us/library/system.threading.cancellationtoken(v=vs.110).aspx
[LINK3]: tbd
