# Creating a Wi-Fi connection to my Lumia 640XL, with the Gadgeteer

My first Gadgeteer project I wrote about [here][LINK1] used an Ethernet connection.  However, this limited the mobility of my device and therefore wanted to configure the device to connect to a Wi-Fi network.  I must mention that another coolness factor with this Wi-Fi project was I used the Wi-Fi internet sharing capability on my Microsoft Lumia 640XL.  That means that my device can go anywhere and connect to the internet from any place my mobile phone can get a mobile connection.  Yes, GHI Electronics, the makers of the Gadgeteer, do have a GSM module to place a SIM card into, but I didn’t have the module nor an extra SIM card laying around.  This was a cool project, cool because it was pretty simple and it simply worked!

As shown in Figure 1, I used a FEZ Raptor as the main board and the wifiR21 module.  I didn’t add the module to my existing project, because I wanted to keep it simple, then I can migrate the code to its final place once tested and functional.  Plus I think Raptor is cool name and had to try it out.

![FEZ Raptor using the wifiR21][FIGURE1]
###### Figure 1, FEZ Raptor using the wifiR21

Additionally, I used the button to trigger my connection and an LED to provide visual feedback of the progression of code execution through my program.

I did have a small problem getting the FEZ Raptor to work with an usbClientSP, USB Client SP 1.3 module, for some reason my PC would not recognize the device.  I changed to the usbClientDP and it worked out.

Additionally, as I used numerous devices and change them out pretty fast, I got an error while deploying, shown in Figure 2.  The error was “Unable to communicate with device USB:Gadgeteer” or “Unable to communicate with device USB:G400”.

![Gadgeteer deployment, Unable to communicate with device][FIGURE2]
###### Figure 2, Gadgeteer deployment, Unable to communicate with device

I resolved the issue by setting the device name correctly in the .NET Micro Framework tab of the Visual Studio 2013 project properties page.

I found this link which described how to make a Wi-Fi Connection [here][LINK2], however it did not work so well in my case.  There are lots of good examples on that site, which is why I share it, it did help me get my code to work and was very helpful.  Instead, I used the code shown in Listing 1.

###### Listing 1, how to make a Wi-Fi connection using the Gadgeteer with wifiRS21

```
private void initializeNetwork()         
{
 try             
 { 
  wifiRS21.NetworkInterface.Open();                 
  wifiRS21.NetworkInterface.EnableDhcp();                 
  wifiRS21.NetworkInterface.EnableDynamicDns();                 
  wifiRS21.NetworkInterface.Join("BENPERK640XL", "*****");              
  while (wifiRS21.NetworkInterface.IPAddress == "0.0.0.0")   
  {                  
   Debug.Print("Waiting for DHCP");         
   Thread.Sleep(250);
  }
  Thread.Sleep(1000);
 }
 catch (Exception ex)             
 {
  Debug.Print(ex.Message);                 
  multicolorLED.BlinkRepeatedly(GT.Color.Red);
 }
}
```

That’s it, it was much easier than expected.

And like I mentioned, I connect the device to my Lumia 640XL, as shown in Figure 3.

![connecting a Gadgeteer device to a mobile phone via Wi-FI][FIGURE3]
###### Figure 3, connecting a Gadgeteer device to a mobile phone via Wi-FI

Basically, you enable the INTERNET SHARING capability within the setting feature.  Notice that the broadcast name is BENPERK640XL which is the same as the SSID used as a parameter with the wifiRS21 Join() method.  After my device is instructed to make the connection to my 640XL, you can see that the number of connections increases to 1 and that I can have a total of 8 Wi-Fi connections.

Here is a picture of my device, Figure 4.

![the Gadgeteer, a wifiRS21 and a Lumia 640XL][FIGURE4]
###### Figure 4, the Gadgeteer, a wifiRS21 and a Lumia 640XL

After I completed this project I remember a video I watched where the speaker said we can save Rainforests with a mobile phone / cell phone.  The video is here.  I hope one day to contribute my skills to a project similar to that which has an impact on the planet or its’ inhabitants.

Here is the source code project, just for review and example.  HTH

The exception I got when using the example on the Gadgeteer community site.  See the code in Listing 1 which show how I got the connection to work.

```
Step into: Stepping over non-user code 'GHI.Networking.WiFiRS9110.WiFiRS9110'
Step into: Stepping over non-user code 'GHI.Networking.WiFiRS9110.WiFiRS9110'
Step into: Stepping over non-user code 'GHI.Networking.BaseInterface.BaseInterface'
Step into: Stepping over non-user code 'GHI.Networking.BaseInterface.BaseInterface'
Step into: Stepping over non-user code 'GHI.Networking.BaseInterface.BaseInterface'
Step into: Stepping over non-user code 'System.InvalidOperationException.InvalidOperationException'
Step into: Stepping over non-user code 'System.SystemException.SystemException'
Step into: Stepping over non-user code 'System.Exception.Exception'
Step into: Stepping over non-user code 'System.Exception.Exception'
Step into: Stepping over non-user code 'System.SystemException.SystemException'
Step into: Stepping over non-user code 'System.InvalidOperationException.InvalidOperationException'
Step into: Stepping over non-user code 'GHI.Networking.BaseInterface.BaseInterface'
    #### Exception System.InvalidOperationException - 0x00000000 (1) ####
    #### Message: This interface type is already created.
    #### GHI.Networking.BaseInterface::.ctor [IP: 0032] ####
    #### GHI.Networking.WiFiRS9110::.ctor [IP: 0005] ####
    #### GHI.Networking.WiFiRS9110::.ctor [IP: 000e] ####
    #### RaptorWifi.Program::initializeNetwork [IP: 0009] ####
    #### RaptorWifi.Program::button_ButtonPressed [IP: 0010] ####
    #### Gadgeteer.Modules.GHIElectronics.Button::OnButtonEvent [IP: 0057] ####
    #### System.Reflection.MethodBase::Invoke [IP: 0000] ####
    #### Gadgeteer.Program::DoOperation [IP: 001a] ####
    #### Microsoft.SPOT.Dispatcher::PushFrameImpl [IP: 0054] ####
    #### Microsoft.SPOT.Dispatcher::PushFrame [IP: 001a] ####
    #### Microsoft.SPOT.Dispatcher::Run [IP: 0006] ####
    #### Gadgeteer.Program::Run [IP: 001d] ####
A first chance exception of type 'System.InvalidOperationException' occurred in GHI.Networking.dll
Step into: Stepping over non-user code 'GHI.Networking.BaseInterface.BaseInterface'
Step into: Stepping over non-user code 'GHI.Networking.BaseInterface.BaseInterface'
Step into: Stepping over non-user code 'System.Exception.Message.get'
```

This interface type is already created.

[FIGURE1]: ../images/2015/msdn-0512.png "Figure 1, FEZ Raptor using the wifiR21"
[FIGURE2]: ../images/2015/msdn-0513.png "Figure 2, Gadgeteer deployment, Unable to communicate with device"
[FIGURE3]: ../images/2015/msdn-0514.png "Figure 3, connecting a Gadgeteer device to a mobile phone via Wi-FI"
[FIGURE4]: ../images/2015/msdn-0515.png "Figure 4, the Gadgeteer, a wifiRS21 and a Lumia 640XL"

[LINK1]: ../2014/2014-12-upload-an-image-to-an-azure-blob-container-using-the-gadgeteer-part-1.md
[LINK2]: https://www.ghielectronics.com/docs/30/networking
