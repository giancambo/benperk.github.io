# Troubleshooting an Azure App Service WebJob

Whenever you write a WebJob, for example in C#, you should always place your code within try{} catch{} code blocks.  If you do not manage your exceptions and your code throws an exception, it will likely terminate to process.

I wanted to find out what happened when my WebJob threw an exception and I did so in 3 different scenarios, as shown in Figure 1.

![troubleshooting a WebJob][FIGURE1]
###### Figure 1, troubleshooting a WebJob

The 3 scenarios are:

+ EXCEPTION001 – code is contained in a try{} catch {} but the exception is not written to the Console
+ EXCEPTION002 – code is contained in a try{} catch {} and the exception is written to the Console
+ EXCEPTION003 – code is not contained in a try{} catch{}

Notice also in Figure 1 that your can manually trigger and view the logs of a WebJob.

## EXCEPTION001

Here is the code for the EXCEPTION001 WebJob, just a simple program that throws an exception within a try{} catch{} code block.

```
namespace exception001
{
  class Program
  {
    static void Main(string[] args)
  {
    try
    {
      throw new System.InsufficientMemoryException("InsufficientMemoryException: Test message to see if it show up in console");
    }
    catch (Exception ex)
    {
      //Do nothing
    }
  }
 }
}
```

I triggered the WebJob from the Azure portal and looking at the logs I see the following in Figure 2.

![WebJob monitor output for troubleshooting][FIGURE2]
###### Figure 2, WebJob monitor output for troubleshooting

Notice that although there was an exception, because I did not do anything with it, there is no log that it happened.

## EXCEPTION002

Here is the code for the EXCEPTION002 WebJob, just a simple program that throws an exception within a try{} catch{} code block.

```
namespace exception002
{
  class Program
  {
    static void Main(string[] args)
    {
      try
      {
        throw new System.InsufficientMemoryException("InsufficientMemoryException: Test message to see if it show up in console");
      }
      catch (Exception ex)
      {
        WriteLine(ex.Message);
      }
    }
  }
}
```

I triggered the WebJob from the Azure portal and looking at the logs I see the following in Figure 3.

![WebJob monitor output for troubleshooting][FIGURE3]
###### Figure 3, WebJob monitor output for troubleshooting

Notice that because I caught the exception and also wrote it to the console I can see what the exception was.  This can give me some clues on what the issue is and how I can resolve it.

## EXCEPTION003
Here is the code for the EXCEPTION003 WebJob, just a simple program that throws an exception without using a try{} catch{} code block.

```
namespace exception003
{
  class Program
  {
    static void Main(string[] args)
    {
      throw new System.InsufficientMemoryException("InsufficientMemoryException: Test message to see if it show up in console");
    }
  }
}
```

I triggered the WebJob from the Azure portal and looking at the logs I see the following in Figure 4.

![WebJob monitor output for troubleshooting][FIGURE4]
###### Figure 4, WebJob monitor output for troubleshooting

Notice in Figure 4 that the exception is shown in the output, but the job failed.

## Conclusion

The primary point here is that to get an understanding of why your WebJob is not running as expected you need to take some actions in your code to write any issues or unexpected exceptions to the output or to a log.

Another learning I got while doing this action is that in both the EXCEPTION001 and EXCEPTIN002, the status of the WebJob was success even though there was an exception.  You need to realize that Success means that the WebJob finished doing what is was programed to do, it does not mean that it did it correctly.  You as the coder need to make sure you log if the WebJob completes and you as an Administrator need to check the status to determine if all is well.

[FIGURE1]: ../images/2017/msdn-1169.png "Figure 1, troubleshooting a WebJob"
[FIGURE2]: ../images/2017/msdn-1170.png "Figure 2, WebJob monitor output for troubleshooting"
[FIGURE3]: ../images/2017/msdn-1171.png "Figure 3, WebJob monitor output for troubleshooting"
[FIGURE4]: ../images/2017/msdn-1172.png "Figure 4, WebJob monitor output for troubleshooting"
