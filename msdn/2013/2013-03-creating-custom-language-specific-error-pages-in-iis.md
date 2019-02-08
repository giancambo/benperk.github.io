# Creating custom, language specific error pages in IIS

Some time ago I wrote an [article][LINK1] that provided information about how to implement customer error pages into IIS.

That article was focused only on providing a custom error page in a single language.  This article will cover instructions on how to configure IIS to use the language setting in Internet Explorer to choose a language specific error page.

The most important point is that you need to install a language pack onto the server for the language you want to support.  This is covered in some detail below.

First, select the Error Pages icon in the Features pane, as shown in Figure 1.  Notice that focus is at the Web Server level and these setting are applied to all web sites hosted on the Web Server.  If you prefer to implement this only on a specific web site, then select the web site and then the Error Pages icon.

![Web Site Error Pages Configuration][FIGURE1]
###### Figure 1, Web Site Error Pages Configuration

By default, the error pages are located in the %SystemDrive%\inetpub\custer\<LANGUAGE-TAG>\ directory, as shown in Figure 2.

![IIS Error Pages feature][FIGURE2]
###### Figure 2, IIS Error Pages feature

Let’s focus on the 404 Status Code and get this to work with a custom language.  If you open Windows Explorer and check that path you will notice that by default the path is C:\inetpub\custerr\en-US.  The default language code is ***en*** and the geographic region is ***US***.  Let’s implement the following languages, shown in Table 1.

| Language code | Geographical Location |
| ------------- | --------------------- |
| German | de-DE |
| French | fr-FR |
| Itialian | it-IT |

###### Table 1, Languages to be implemented

As already mentioned, you need to install the language pack onto the server for the languages you wish to support.

## Download and install Language Pack. 

To find the correct Language Pack search for “Windows Server 2012 Language Pack” or “Windows Server 2008 Language Pack” using Bing and you will most likely get the correct link to download.  If you have an MSDN subscription you may want to check in there too. 

For this example I downloaded and installed “Windows Server 2012 Language Pack (x64) – DVD (Multiple Languages) with a file name of mu_windows_server_2012_language_pack_x64_dvd_917551.iso

To install the language pack for French, open a command prompt and enter: lpksetup as shown in Figure 3.

![running lpksetup from a Command Prompt][FIGURE3]
###### Figure 3, running lpksetup from a Command Prompt

Running this command will result in Figure 4 being rendered.

![install display languages from a language pack, lp.cab][FIGURE4]
###### Figure 4, install display languages from a language pack, lp.cab

Select Install display languages and browse to the directory that contains the fr-FR/lp.cap file.  Figure 5 represents this window.

![select the language pack lp.cab file ][FIGURE5]
###### Figure 5, select the language pack lp.cab file 

Once found and selected, click the Next button and accept the license terms to continue.  After accepting the terms and clicking Next, the language pack is installed.  Status is provided as shown in Figure 6.

![Installing display language, language pack for language custom specific IIS errors][FIGURE6]
###### Figure 6, Installing display language, language pack for language custom specific IIS errors

Perform the same for de-DE and it-IT.  Once complete, open Windows Explorer and navigate to the C:\inetpub\custerr directory and view the directories created by the installation of the language packs as shown in Figure 7.

![Windows explorer showing languages IIS can support for custom Error Pages][FIGURE7]
###### Figure 7, Windows explorer showing languages IIS can support for custom Error Pages

Now that the language pack installations are complete open Internet Explore and check to see which language your browser is set to.  To do this select Tools -> Internet Options -> General tab -> select the Languages button.  Figure 8 shows that my browser is set to German, therefore when I access the website and trigger a 404 error, I expect the error message to be in German.

![Internet Explorer Language Preference][FIGURE8]
###### Figure 8, Internet Explorer Language Preference

But before checking the custom error result, let’s start Fiddler so we can see what is sent from Internet Explorer to IIS.  Figure 9, shows a request where my Internet Explorer browser setting is set to de-DE, I changed it to fr-FR and the Accept-Language value is indeed changed.

![Fiddler Accept-Language header value][FIGURE9]
###### Figure 9, Fiddler Accept-Language header value

After setting the language back to German in Internet Explorer and making a request for a file that does not exist on the server, the response is indeed in German as shown in Figure 10.

![Custom IIS Language Error Page - 404 - Datei oder Verzeichnis wurde nicht gefunden][FIGURE10]
###### Figure 10, Custom IIS Language Error Page - 404 - Datei oder Verzeichnis wurde nicht gefunden

Lastly, you may also want to host the custom language files in a location other than the default.  For example, D:\ErrorPages.  To achieve this, copy the directories shown in Figure 7, previously to the D:\ErrorPages, open the IIS management console and double-click the Error Pages feature.
For this example, double-click on the Status Code 404, then click the Set… button and navigate to the D:\ErrorPages, as shown in Figure 11.

![Setting the localized Custom error path][FIGURE11]
###### Figure 11, Setting the localized Custom error path

Click OK twice and you will see the following result within the Error Pages feature as illustrated with Figure 12.

![Storing custom language specific error pages in a non-default location][FIGURE12]
###### Figure 12, Storing custom language specific error pages in a non-default location

Test the modifications as previously defined and you have done it.  Good work! 

[FIGURE1]: ../images/2013/msdn-0213.png "Figure 1, Web Site Error Pages Configuration"
[FIGURE2]: ../images/2013/msdn-0214.png "Figure 2, IIS Error Pages feature"
[FIGURE3]: ../images/2013/msdn-0215.png "Figure 3, running lpksetup from a Command Prompt"
[FIGURE4]: ../images/2013/msdn-0216.png "Figure 4, install display languages from a language pack, lp.cab"
[FIGURE5]: ../images/2013/msdn-0217.png "Figure 5, select the language pack lp.cab file "
[FIGURE6]: ../images/2013/msdn-0218.png "Figure 6, Installing display language, language pack for language custom specific IIS errors"
[FIGURE7]: ../images/2013/msdn-0219.png "Figure 7, Windows explorer showing languages IIS can support for custom Error Pages"
[FIGURE8]: ../images/2013/msdn-0220.png "Figure 8, Internet Explorer Language Preference"
[FIGURE9]: ../images/2013/msdn-0221.png "Figure 9, Fiddler Accept-Language header value"
[FIGURE10]: ../images/2013/msdn-0222.png "Figure 10, Custom IIS Language Error Page - 404 - Datei oder Verzeichnis wurde nicht gefunden"
[FIGURE11]: ../images/2013/msdn-0223.png "Figure 11, Setting the localized Custom error path"
[FIGURE12]: ../images/2013/msdn-0224.png "Figure 12, Storing custom language specific error pages in a non-default location"

[LINK1]: ../2012/2012-05-custom-error-pages-http-error-500-19-internal-server-error.md
