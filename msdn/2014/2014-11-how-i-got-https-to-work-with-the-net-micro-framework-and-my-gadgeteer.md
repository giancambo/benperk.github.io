# How I got HTTPS to work with the .Net Micro Framework and my Gadgeteer

I have begun my pursuit of the Internet of Things (IoT) via the .NET Micro Framework and a FEZ Spider.  I am currently using the following SDKs:

+ [.NET MF 4.3 RTM (QFE1)][LINK1]
+ [.NET Gadgeteer][LINK2]
+ [NETMF and Gadgeteer Package 2014 R3][LINK3]

When I was trying to make a connection to my Azure Blob Storage, I was getting the following exception, which only happened when I used HTTPS.

```
Step into: Stepping over non-user code 'Microsoft.SPOT.Net.Security.SslStream.Authenticate'
#### Exception System.NotSupportedException - CLR_E_NOT_SUPPORTED (1) ####
#### Message:
#### Microsoft.SPOT.Net.Security.SslNative::SecureClientInit [IP: 0000] ####
#### Microsoft.SPOT.Net.Security.SslStream::Authenticate [IP: 0051] ####
#### Microsoft.SPOT.Net.Security.SslStream::AuthenticateAsClient [IP: 000c] ####
#### System.Net.HttpWebRequest::EstablishConnection [IP: 0247] ####
#### System.Net.HttpWebRequest::SubmitRequest [IP: 0019] ####
#### System.Net.HttpWebRequest::GetRequestStream [IP: 0008] ####
#### POC01_Azure.Program::insertImageintoAzureBlob [IP: 0041] ####
#### POC01_Azure.Program::camera_PictureCaptured [IP: 0052] ####
#### Gadgeteer.Modules.GHIElectronics.Camera::OnPictureCaptured [IP: 0036] ####
#### System.Reflection.MethodBase::Invoke [IP: 0000] ####
#### Gadgeteer.Program::DoOperation [IP: 001a] ####
#### Microsoft.SPOT.Dispatcher::PushFrameImpl [IP: 0054] ####
#### Microsoft.SPOT.Dispatcher::PushFrame [IP: 001a] ####
#### Microsoft.SPOT.Dispatcher::Run [IP: 0006] ####
#### Gadgeteer.Program::Run [IP: 001d] ####
A first chance exception of type 'System.NotSupportedException' occurred in Microsoft.SPOT.Net.Security.dll
Step into: Stepping over non-user code 'Microsoft.SPOT.Net.Security.SslStream.Authenticate'
A first chance exception of type 'System.NotSupportedException' occurred in System.Net.Security.dll
A first chance exception of type 'System.NotSupportedException' occurred in System.Http.dll
```

I created a simple .NET Micro Framework test HTTP request as shown in Listing 1, just to make sure the my Ethernet connection was working as expected and it did, as shown in Figure 1.

###### Listing 1, how to make an HTTP request using the .NET Micro Framework, FEZ Spider
```
void makeGenericHTTPRequest()
{
  try
  {
   string url = "https://*?.azurewebsites.net/";
   using (var req = System.Net.HttpWebRequest.Create(url))
   {
   using (var res = req.GetResponse())
   {
    Debug.Print("HTTP Response was this long: " + res.ContentLength.ToString());
   }
  }
 }
 catch (Exception ex)
 {
  Debug.Print(ex.Message);
 }}
```

![output window in Visual Studio 2012 for an HTTP request using .NET MF 4.3][FIGURE1]
###### Figure 1, output window in Visual Studio 2012 for an HTTP request using .NET MF 4.3

Anyway, the point is that I was getting an exception when using SSL, I.e. when I changed the URL to use HTTPS instead of HTTP.

## Solution

When you install Microsoft .NET Micro Framework 4.3, it includes a tool called MfDeploy.exe found in this directory, by default:  C:\Program Files (x86)\Microsoft .NET Micro Framework\v4.3\Tools

Open the tool and connect your device.  Once connected, select Target -> Manage Device Keys -> Update SSL Seed, as shown in Figure 2.

![Update the SSL Seed to make SSL/HTTPS connections using a Gadgeteer][FIGURE2]
###### Figure 2, Update the SSL Seed to make SSL/HTTPS connections using a Gadgeteer

Once the seed is successfully updated, you will receive a message similar to that in Figure 3.

![Updating SSL seed for making SSL/HTTPS using .NET MF 4.3 and Gadgeteer][FIGURE3]
###### Figure 3, Updating SSL seed for making SSL/HTTPS using .NET MF 4.3 and Gadgeteer

After doing this I was able to request a URL using HTTPS.

[FIGURE1]: ../images/2014/msdn-0470.png "Figure 1, output window in Visual Studio 2012 for an HTTP request using .NET MF 4.3"
[FIGURE2]: ../images/2014/msdn-0471.png "Figure 2, Update the SSL Seed to make SSL/HTTPS connections using a Gadgeteer"
[FIGURE3]: ../images/2014/msdn-0472.png "Figure 3, Updating SSL seed for making SSL/HTTPS using .NET MF 4.3 and Gadgeteer"

[LINK1]: http://netmf.codeplex.com/releases/view/118283
[LINK2]: http://gadgeteer.codeplex.com/releases/view/134757
[LINK3]: https://www.ghielectronics.com/support/netmf/sdks
