# Lab 28: The impact of debug=true

## Prerequisites

+ Read [this][LINK2] about .NET Compilation
+ Read [this][LINK3] about why debug=true is not good for performance
+ Understand that ASP.NET (ASPX, ASCX, ASAX, MVC) files are compiled into a DLL and stored into c:\Windows\Microsoft.NET\Framework?\v?.?.?\Temporary ASP.NET Files

## Setup

+ Install CSharpGuitarBugs.zip onto an IIS web server as per [Lab 1][LINK1]
+ Install the ASP.NET Role

1. Open the IIS Manager Console and click the CSharpGuitarBugs website

2. Click on the .NET Compilation feature and set Debug to true, apply the changes

![The impact of debug=true][FIGURE1]
###### Figure 1, The impact of debug=true

3. Check the version which the CSharpGuitarBugs application pool is running under and then navigate to the associated ASP.NET temporary files (the application pool is configured to run in 64 bit and with .NET 4.5.2), therefore I look int e followign directory for the Temporary ASP.NET files.

![The impact of debug=true][FIGURE2]
###### Figure 2, The impact of debug=true

4. Delete all the contents…

5. Access the website and browse to a few pages, for example Full Catalog pages, View by Manufacturer, Enter Payment Information, etc…

6. Check to see how many files and the size of the created temporary files

![The impact of debug=true][FIGURE3]
###### Figure 3, The impact of debug=true

7. Delete the temporary ASP.NET files (you may need to perform an IISReset)

8. Now set Debug=false and perform the same action as before

9. Check the size and number of temporary files created now

![The impact of debug=true][FIGURE4]
###### Figure 4, The impact of debug=true

10. Take all the DLL files and copy them to the c:\temp directory

11. Open the Reflector or Just Decompile and add the DLLs

12. When Debug=False, all pages are compiled into a single DLL, view the code…

![The impact of debug=true][FIGURE5]
###### Figure 5, The impact of debug=true

[FIGURE1]: ../images/2016/msdn-0925.png "Figure 1, The impact of debug=true"
[FIGURE2]: ../images/2016/msdn-0926.png "Figure 2, The impact of debug=true"
[FIGURE3]: ../images/2016/msdn-0927.png "Figure 3, The impact of debug=true"
[FIGURE4]: ../images/2016/msdn-0928.png "Figure 4, The impact of debug=true"
[FIGURE5]: ../images/2016/msdn-0929.png "Figure 5, The impact of debug=true"

[LINK1]: 2016-IISLAB-lab-1-install-iis-and-create-a-web-site.md
[LINK2]: http://technet.microsoft.com/en-us/library/hh831642.aspx
[LINK3]: http://blogs.msdn.com/b/benjaminperkins/archive/2012/09/17/debug-true.aspx
