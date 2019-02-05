# My Windows Phone App – Hitman Website Monitor

I was surprised at how easy it was to create and deploy a Windows Phone app.  If you are a developer then you will have no problems creating an application.  After I installed the Windows Phone 7.1 SDK alongside my Visual Studio 2010 installation, I created the project and got to work.
You can download my Windows Phone app [here][LINK1].

The application is called ‘Hitman Website Monitor’.  The free version, there is only a free version, allows you to manually monitor up to 4 websites using a response match.  For example, you can create web page that makes connections to a database or validates a transaction and if they work, write OK to the page requested from the monitor.

One of the challenges I had when creating the Windows Phone App was the creation of the local database and performing the CRUD actions against it.  I found a very good example on how to do this [here][LINK2].  I used the MVVM model, as described in the article, within the Hitman Website Monitor.

Figure 1 shows the 3 pages that make up the Windows Phone App using the 7.1 SDK.

![Hitman Website Monitor created using Windows Phone 7.1 SDK][FIGURE1]
###### Figure 1, Hitman Website Monitor created using Windows Phone 7.1 SDK

Install it and let me know what you think.

I also used the [Deployment.Current]LINK3 property to manage the transition between threads as I implemented the [BeginGetRequestStream][LINK4] and [BeginGetResponse][LINK5] methods to handle the request to the website.

***NOTE:***  This Windows Phone App is not associated or supported by Microsoft.  It was created just for fun to learn how to do it.

I am in the process of creating the same application as a Windows 8 App, Figure 2.  I mentioned above the usage of MVVM and Deployment.Current specifically.  In my Windows 8 App I was only able to store data locally using SQLite.  There may be other options but I chose this one.  I had to get fancy to bind the SQLite queries into the Sample Data Source provided when you create a Windows 8 Split App (XAML) Project. But I got it and it works fine.

![Hitman Website Monitor using the Visual Studio 2012 Split App (XAML) Project][FIGURE2]
###### Figure 2, Hitman Website Monitor using the Visual Studio 2012 Split App (XAML) Project

The other thing is that I could not find Deployment.Current.  I searched around and found that [SynchronizationContext][LINK6] worked just fine for my needs.

Once I get the Windows 8 App published I will share it via another blog.  I'll also share when I upgrade this Windows Phone App to version 8.

[FIGURE1]: ../images/2011/msdn-0180.png "Figure 1, Hitman Website Monitor created using Windows Phone 7.1 SDK"
[FIGURE2]: ../images/2011/msdn-0181.png "Figure 2, Hitman Website Monitor using the Visual Studio 2012 Split App (XAML) Project"

[LINK1]: http://www.windowsphone.com/en-us/store/app/hitman/21521bdb-dd8a-40d7-855f-4290a75365e4
[LINK2]: http://msdn.microsoft.com/en-us/library/hh286405(v=VS.92).aspx
[LINK3]: http://msdn.microsoft.com/en-us/library/system.windows.deployment.current(v=VS.95).aspx
[LINK4]: http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.begingetrequeststream.aspx
[LINK5]: http://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.begingetresponse.aspx
[LINK6]: http://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext.aspx
