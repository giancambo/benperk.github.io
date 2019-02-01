# Installing Window 8 Beta on Hyper-V, Key Combinations

I am very excited, to say the least, to start using the preview/beta releases of Windows 8 Server, Windows 8, IIS 8 and Visual Studio 11.
I did choose to install the instance of Windows Server 8 and Windows 8 into a Hyper-V Virtual Machine.  I did this mostly just to save time and I still needed my work machine to perform my day-to-day activities.
The first thing I noticed was, whoa…where is the Start button?  In both Windows Server 8 and Windows 8, I was not able to find it.  The following two images, Figure 1 and Figure 2, show the toolbars.

![Windows Server 8 toolbar][FIGURE1]
###### Figure 1, Windows Server 8 toolbar

![Windows 8 toolbar][FIGURE2]
###### Figure 2, Windows 8 toolbar

It took a few moments to finally decide to use shortcut keys to navigate around, but because I was using Hyper-V my key combinations were being executed on the host machine and not on the virtual machine. 
It was a simple modification to configure Hyper-V so that my shortcut key combinations are executed on the virtual machine.  First, select the Hyper-V Settings… menu item from within the Hyper-V Manager as shown in Figure 3.

![Hyper-V Settings… in the Hyper-V Manager][FIGURE3]
###### Figure 3, Hyper-V Settings… in the Hyper-V Manager

Next, select Keyboard and then "Use on the virtual machine" as shown in Figure 4.

![Hyper-V Settings… Setting keyboard key combinations][FIGURE4]
###### Figure 4, Hyper-V Settings… Setting keyboard key combinations

You will need to restart the connection to any open virtual machines before the modification will take effect.  Then I was able to navigate around with ease.

[Here][LINK1] is a very good link with descriptions of Windows 8 shortcuts. 

A few of my favorites are in the following table, but they all are useful:

| Result | Key combination |
| ------ | --------------- |
| Show Desktop  | Win + D  |
| Open Windows Explorer  | Win + E  |
| Open Settings Charm  |  Win + I |
| Open Run Dialog  | Win + R  |

[LINK1]: http://blogs.technet.com/b/hritter/archive/2012/03/14/ultimate-shortcuts-guide-windows-8-consumer-preview-english.aspx

[FIGURE1]: ../images/2012/msdn-0118.png "Figure 1, Windows Server 8 toolbar"
[FIGURE2]: ../images/2012/msdn-0119.png "Figure 2, Windows 8 toolbar"
[FIGURE3]: ../images/2012/msdn-0120.png "Figure 3, Hyper-V Settings… in the Hyper-V Manager"
[FIGURE4]: ../images/2012/msdn-0121.png "Figure 4, Hyper-V Settings… Setting keyboard key combinations"
