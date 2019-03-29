# Lab 22: Deploy and create a custom Module and Handler

This is a important concept to know and it really isn’t an intuitive one, you need to get into the guts of IIS or configure some 3rd party web based application to ever see or configure modules and handlers in IIS.  When you install ASP.NET for example, the handler is installed and configured by default.

## References

+ Read “Developing Modules and Handlers with the .NET Framework” [here][LINK1]
+ Read “IIS Modules Overview” [here][LINK2]
+ Read “Creating a Synchronous HTTP handler” [here][LINK3] and [here][LINK4]
+ Read about the IHttpHandler interface [here][LINK5]
+ Read about the IHttpModule interface [here][LINK6]
+ <Add my article about an asynchronous handler ASAP>

## Prerequisites

+ Install ASP.NET as per Lab 2 [here][LINK7]
+ Review Lab 7-3 to get a better understanding of the IIS / ASP.NET request pipeline [here][LINK8]
+ Extra credit, enable Failed Request Tracing and see how the request is managed, as per Lab 4 [here][LINK9]

## In this lab you will

+ Create and configure a custom handler – How to create a custom handler using C# for IIS
+ Create and configure a custom module – How to create a custom module using C# for IIS
+ Learn the difference between a handler and a module

## Lab 22-1

1. Create a new project in Visual Studio and name it CustomManagedHandler

![Deploy and create a custom Module and Handler][FIGURE1]
###### Figure 1, Deploy and create a custom Module and Handler

2.  Rename the Class1.cs file to CustomHandler by right-clicking on the file and selecting Rename from the menu

3.  Add a reference to the System.Web module by right-clicking the References folder and selecting Add Reference.  Add the using System.Web to the CustomHandler class file as well.

![Deploy and create a custom Module and Handler][FIGURE2]
###### Figure 2, Deploy and create a custom Module and Handler

4.  The CustomeHandler.cs file should look something like this, make note of the implementation of the IHttpHandler interface.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
 
using System.Web;
 
namespace CustomManagedHandler
{
    public class CustomHandler : IHttpHandler
    {
        public bool IsReusable
        {
            get { return false; }
        }
 
        public void ProcessRequest(HttpContext context)
        {
            context.Response.Write("Hello from the CustomManagedHandler handler.");
        }
    }
}
```

The ProcessRequest() method is the place where you put all the code necessary to handle a request to the file type in the given context.  For example, later we will make a request to a file named default.benjamin, when we configure this handler in IIS, it will respond to any request for a file that has the .benjamin extension.

5.  Compile and build the handler, a file called CustomManagedHandler.dll is created in the /bin/Release folder within the project you created in step 22-1-1.

## Lab 22-2

1. Access a Windows Server running IIS and open the management console

2. Make a request to a file named default.benjamin, you will initially get a 404.0, so create one in the root directory of the Default web site C:\inetpub\wwwroot.  Once added you should get a 404.3 which means there is no MIME map configured for the file extension.  You can map that extension type to an existing handler, or to the custom one in which you just created in Lab 22-1.

![Deploy and create a custom Module and Handler][FIGURE3]
###### Figure 3, Deploy and create a custom Module and Handler

![Deploy and create a custom Module and Handler][FIGURE4]
###### Figure 4, Deploy and create a custom Module and Handler

3. Add a /bin directory to the Default web site and place the CustomManagedHandler.dll into the directory

![Deploy and create a custom Module and Handler][FIGURE5]
###### Figure 5, Deploy and create a custom Module and Handler

4.  Configure the handler mapping using the IIS management console, double-click the Handler Mappings feature

![Deploy and create a custom Module and Handler][FIGURE6]
###### Figure 6, Deploy and create a custom Module and Handler

5. Click on the Add Managed Handler link in the Actions pane and configure the CustomManagedHandler.CustomHandler to respond to any request to a *.benjamin file.

Click on the Request Restrictions button and review the contents of the tabs:

+ Mapping
+ Verbs
+ Access

![Deploy and create a custom Module and Handler][FIGURE7]
###### Figure 7, Deploy and create a custom Module and Handler

6. Access the default.benjamin file again from the browser, if you did not install ASP.NET then you will get a 500.21.

![Deploy and create a custom Module and Handler][FIGURE8]
###### Figure 8, Deploy and create a custom Module and Handler

When successful, you will see the response coded within the ProcessRequest() method.

![Deploy and create a custom Module and Handler][FIGURE9]
###### Figure 9, Deploy and create a custom Module and Handler

## Lab 22-3

1. Create a new project in Visual Studio and name it CustomManagedModule

![Deploy and create a custom Module and Handler][FIGURE10]
###### Figure 10, Deploy and create a custom Module and Handler

2.  Rename the Class1.cs file to CustomModule by right-clicking on the file and selecting Rename from the menu

3.  Add a reference to the System.Web module by right-clicking the References folder and selecting Add Reference.  Add the using System.Web to the CustomModule class file as well.

![Deploy and create a custom Module and Handler][FIGURE11]
###### Figure 11, Deploy and create a custom Module and Handler

4.  The CustomeModule.cs file should look something like this, make special attention to the implementation of the IHttpModule interface

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
 
using System.Web;
 
namespace CustomManagedModule
{
    public class CustomModule : IHttpModule
    {
        public void Init(HttpApplication app)
        {
            app.BeginRequest += new EventHandler(app_BeginRequest);
        }
 
        public void app_BeginRequest(object o, EventArgs e)
        {
            HttpContext context = HttpContext.Current;
            string requestURL = context.Request.RawUrl;
            if (requestURL.EndsWith(".ben"))
            {
                context.Response.Redirect("default.benjamin");
            }
        }
 
        public void Dispose() {}
    }
}
```

In the source code, right-click on the HttpApplication class then Go to Definition.  Review all the EventHandlers, for example, AcquireRequestState, AuthenticateRequest, AuthorizeRequest, BeginRequest, etc… these are the events into which you can bind your module to.  In this example, the module is bound to the BeginRequest event using the += operator and told to call the app_BeginRequest() method when the HttpApplication.BeginRequest() method is called.  In the app_BeginRequest() method I look at the request to see if the extension matches ‘-ben’ and if so, I redirect the request to the default.benjamin file.

5.  Compile and build the module, a file called CustomManagedModule.dll is created in the /bin/Release folder within the project you created in step 22-3-1.

## Lab 22-4

1. Access a Windows Server running IIS and open the management console

2. Add the CustomManagedModule.dll to the /bin directory of the Default web site , also add a file named default.ben to the root of the Default web site, similar to what you did for the default.benjamin file in step 22-2-2 earlier.

![Deploy and create a custom Module and Handler][FIGURE12]
###### Figure 12, Deploy and create a custom Module and Handler

3.  Configure the module using the IIS management console, double-click the Modules feature

![Deploy and create a custom Module and Handler][FIGURE13]
###### Figure 13, Deploy and create a custom Module and Handler

4.  Click on the Add Managed Module link in the Action pane to configure the module

![Deploy and create a custom Module and Handler][FIGURE14]
###### Figure 14, Deploy and create a custom Module and Handler

5.  Make a request to the default.ben and notice that it is redirected to default.benjamin as expected

![Deploy and create a custom Module and Handler][FIGURE15]
###### Figure 15, Deploy and create a custom Module and Handler

## Extra credit

A request to default.ben is made

![Deploy and create a custom Module and Handler][FIGURE16]
###### Figure 16, Deploy and create a custom Module and Handler

The request enters into the ASP.NET pipeline, the HttpApplication events start firing, specifically the BEGIN:REQUEST.  The CustomManagedModule is triggered and the redirect is performed with a 302

![Deploy and create a custom Module and Handler][FIGURE17]
###### Figure 17, Deploy and create a custom Module and Handler

The request is redirected to default.benjamin as per the code.

## Conclusion

A handler executes based on the type of file extension and is responsible for acquiring and parameters past from the client, using those parameters and sending back an HTML response.

A module hooks itself into a request pipeline and executes code when a specific event in the pipeline is triggered.  It is not responsible for managing the response.

[FIGURE1]: ../images/2016/msdn-0887.png "Figure 1, Deploy and create a custom Module and Handler"
[FIGURE2]: ../images/2016/msdn-0888.png "Figure 2, Deploy and create a custom Module and Handler"
[FIGURE3]: ../images/2016/msdn-0889.png "Figure 3, Deploy and create a custom Module and Handler"
[FIGURE4]: ../images/2016/msdn-0890.png "Figure 4, Deploy and create a custom Module and Handler"
[FIGURE5]: ../images/2016/msdn-0891.png "Figure 5, Deploy and create a custom Module and Handler"
[FIGURE6]: ../images/2016/msdn-0892.png "Figure 6, Deploy and create a custom Module and Handler"
[FIGURE7]: ../images/2016/msdn-0893.png "Figure 7, Deploy and create a custom Module and Handler"
[FIGURE8]: ../images/2016/msdn-0894.png "Figure 8, Deploy and create a custom Module and Handler"
[FIGURE9]: ../images/2016/msdn-0895.png "Figure 9, Deploy and create a custom Module and Handler"
[FIGURE10]: ../images/2016/msdn-0896.png "Figure 10, Deploy and create a custom Module and Handler"
[FIGURE11]: ../images/2016/msdn-0897.png "Figure 11, Deploy and create a custom Module and Handler"
[FIGURE12]: ../images/2016/msdn-0898.png "Figure 12, Deploy and create a custom Module and Handler"
[FIGURE13]: ../images/2016/msdn-0899.png "Figure 13, Deploy and create a custom Module and Handler"
[FIGURE14]: ../images/2016/msdn-0900.png "Figure 14, Deploy and create a custom Module and Handler"
[FIGURE15]: ../images/2016/msdn-0901.png "Figure 15, Deploy and create a custom Module and Handler"
[FIGURE16]: ../images/2016/msdn-0902.png "Figure 16, Deploy and create a custom Module and Handler"
[FIGURE17]: ../images/2016/msdn-0903.png "Figure 17, Deploy and create a custom Module and Handler"

[LINK1]: https://www.iis.net/learn/develop/runtime-extensibility/developing-iis-modules-and-handlers-with-the-net-framework
[LINK2]: https://www.iis.net/learn/get-started/introduction-to-iis/iis-modules-overview
[LINK3]: https://msdn.microsoft.com/en-us/library/ms228090.aspx
[LINK4]: https://support.microsoft.com/en-us/kb/308001
[LINK5]: https://msdn.microsoft.com/en-us/library/system.web.ihttphandler(v=vs.110).aspx
[LINK6]: https://msdn.microsoft.com/en-us/library/system.web.ihttpmodule%28v=vs.110%29.aspx?f=255&MSPPError=-2147217396
[LINK7]: 2016-IISLAB-lab-2-install-the-web-platform-installer.md
[LINK8]: 2016-IISLAB-lab-7-integrated-versus-classic-mode.md
[LINK9]: 2016-IISLAB-lab-4-install-and-configure-failed-request-tracing.md
