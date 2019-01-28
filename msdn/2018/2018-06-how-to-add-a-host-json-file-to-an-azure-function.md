# How to add a HOST.JSON file to an Azure Function

I have written numerous articles about Azure Functions, view them here.  
The fact is, the HOST.JSON file is created for you when the Azure Function App is created originally/initially and it does not need to be added.  I expected to see it when I expanded the View Files for the Function, see Figure 1, but it was not there.

![How to add a HOST.JSON file to an Azure Function][FIGURE1]
###### Figure 1, How to add a HOST.JSON file to an Azure Function

```
using System.Net;
 
public static async Task<HttpResponseMessage>
                          Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info("C# TimeOutHttpTrigger entered Run() method…");
 
    try
    {
        log.Info("C# TimeOutHttpTrigger entered try…");
        await Task.Delay(8000);
        log.Info("C# TimeOutHttpTrigger success delay of 8 seconds…");
        return req.CreateResponse(HttpStatusCode.OK, "Success");
    }
    catch (System.Exception ex)
    {
        log.Info("C# TimeOutHttpTrigger entered catch...");
        return req.CreateResponse(HttpStatusCode.OK, "Error: {ex.Message}");
    }
}
```

It makes perfect sense that the HOST.JSON file is not in the ‘View Files’ blade for the Function because the HOST.JSON file is a ‘global’ file which is for all Functions running in the Function App and would therefore expect it to be 1 level higher up.  Q: But how can I get 1 level higher up?  A: KUDU/SCM

As seen in Figure 2, “1 level up” is the D:\home\site\wwwroot directory, and you can see all the Functions which are running within the Function App.

![How to add a HOST.JSON file to an Azure Function][FIGURE2]
###### Figure 2, How to add a HOST.JSON file to an Azure Function

I can then modify the file and crash my Function exaclty like I wanted, Figure 3.

![How to add a HOST.JSON file to an Azure Function, functionTimeout][FIGURE3]
###### Figure 3, How to add a HOST.JSON file to an Azure Function, functionTimeout

```
{
  "functionTimeout": "00:00:05"
}
```

And when I run this, I get a Timeout as expected, Figure 4.

![Azure Function timeout, functionTimeout, but keeps on going][FIGURE4]
###### Figure 4, Azure Function timeout, functionTimeout, but keeps on going

What I noticed, however, was that although I had my code in a try…catch… block, the exception was aparently thrown by the Microsoft.Azure.WebJobs.Host logic and the code, within the try…, which happend after the timeout still executed.  This is my conclusion because of the log.Info() call after the Task.Delay() method timed out.  “C# TimeOutHttpTrigger success delay of 8 seconds” is written to the output logs windows, Figure 4, which I did not expect.  Also, when I execute the Azure Function using CURL I get a 500 as well, instead of my HttpStatusCode.OK response, Figure 5.

![Azure Function timeout, functionTimeout, but keeps on going][FIGURE5]
###### Figure 5, Azure Function timeout, functionTimeout, but keeps on going

That was not expected, but you can read [here][LINK2] “How (I) would handle a timeout in Azure Function” how I resolved that one.

[FIGURE1]: ../images/2018/msdn-0065.png "Figure 1, how to handle a timeout in Azure Function"
[FIGURE2]: ../images/2018/msdn-0066.png "Figure 2, How to add a HOST.JSON file to an Azure Function"
[FIGURE3]: ../images/2018/msdn-0067.png "Figure 3, How to add a HOST.JSON file to an Azure Function, functionTimeout"
[FIGURE4]: ../images/2018/msdn-0068.png "Figure 4, Azure Function timeout, functionTimeout, but keeps on going"
[FIGURE5]: ../images/2018/msdn-0069.png "Figure 5, Azure Function timeout, functionTimeout, but keeps on going"

[LINK1]: tbd
[LINK2]: 2018-06-how-i-would-handle-a-timeout-in-azure-function.md
