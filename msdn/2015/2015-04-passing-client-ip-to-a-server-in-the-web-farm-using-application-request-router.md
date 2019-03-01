# Passing Client IP to a server in the Web Farm using Application Request Router

When you setup a Web Farm using Application Request Router (ARR) the value for the c-ip written to the IIS log is the IP address of the ARR server and not the client or user IP address.  Click on the Server Farm from within the IIS management console, then click on the Proxy link.  As shown in Figure 1, by default, the Client or user IP address is preserved, or placed into the X-Forwarded-For header.

![passing the Client IP (c-ip) address through ARR][FIGURE1]
###### Figure 1, passing the Client IP (c-ip) address through ARR

By installing Message Analyzer, found [here][LINK1], on the ARR server I was able to confirm this was indeed happening.  Figure 2 illustrates the output of Message Analyzer and the X-Forwarded-For header. Review the “preserve  client IP in the following header” element Name [here][LINK2] for some more detail.

![Client-IP for ARR clients to Server Farm servers][FIGURE2]
###### Figure 2, Client-IP for ARR clients to Server Farm servers

That’s nice, but how can you log it?  Well, in this [post][LINK3] where I discussed “What’s new in IIS 8.5”, you can now add a custom field, such as “X-Forwarded-For” to the data which IIS logs.  On the Server Farm web servers, you need to add a custom field, similar to that shown in Figure 3.

![add custom log filed to IIS for ARR client ip][FIGURE3]
###### Figure 3, add custom log filed to IIS for ARR client ip

Once added, the client or user client IP is logged into the IIS log files and using LogParser, for example, to analyze which IP is accessing your web farm, can be utilized.

## How to do the same prior to IIS 8.5

There is a tool called ARRHelper which you can read about and download from [here][LINK4].  This installs a binary file into the “%PROGRAMFILES%\IIS\ARRHelper” directory which manages the configuration and logging of the client or user IP address.  (Install this on the ARR server).

Once this ARRHelper is installed on the ARR, the X-Forwarded-For value is stored in the C-IP column of the W3C IIS log on the IIS server configured in the Server Farm.  Similar to how it would happen if ARR was not managing/routing/balancing the requests.

***NOTE***: Install this first on a staging environment and test, reading the article that discusses this ARRHelper mentions that it changes more than just the C-IP.  For example, SSL Offloading and Client Certificates.  View the schema file “%windir%\system32\inetsrv\config\schema\arr_helper_schema.xml” to see what change to make sure you don’t have any unwanted impact.   Testing is always recommended.

***UPDATE***: If you use Dynamic IP restrictions with Application Request Router, take a look at the article [here][LINK5] for Information about that.  Basically, if you have enabled enableProxyMode, then the restriction is based on the X-Forwarded-For value and not on the C-IP.

[FIGURE1]: ../images/2015/msdn-0509.png "Figure 1, passing the Client IP (c-ip) address through ARR"
[FIGURE2]: ../images/2015/msdn-0510.png "Figure 2, Client-IP for ARR clients to Server Farm servers"
[FIGURE3]: ../images/2015/msdn-0511.png "Figure 3, add custom log filed to IIS for ARR client ip"

[LINK1]: http://www.microsoft.com/en-us/download/details.aspx?id=44226
[LINK2]: https://technet.microsoft.com/en-us/library/dd443533(v=ws.10).aspx
[LINK3]: ../2013/2013-06-whats-new-in-iis-8-5-microsoft-internet-information-services-8-5-new-features.md
[LINK4]: http://blogs.iis.net/anilr/client-ip-not-logged-on-content-server-when-using-arr
[LINK5]: http://blogs.iis.net/wadeh/dynamic-ip-restriction-proxy-mode
