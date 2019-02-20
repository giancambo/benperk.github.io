# Make your own SSL Certificate for testing and learning

Like many technologies, they seem very complicated when you begin to work with it, but once you get into it you start to hit the boundaries of its capabilities and features.  Once you use a tool for some time you learn what it can and cannot do, and therefore when there is a problem you can quickly rule out or rule in that it is related or not to the given technology.

I have had some time now to learn about certificates on IIS and wanted to share some information about how you can create, export and import SSL certificates for use with testing how SSL certificates are installed and configured on IIS.  One the barriers I had was getting a certificate to play with, as they usually cost money, so I avoided getting one and asking management to pay for one for training me.  Nonetheless, with what I am about to explain here will help you learn some about SSL Certificates.

I initially found this article, which was the basis for my learning.  I like to find articles, learn from them and build on them, adding my experiences and understandings.

One of the initially challenging task I has was finding the MAKECERT and CERTMGR executables required to make and import the certificate.  It is part of the Windows SDK but am not about to tell you which version you need, I installed a few and ultimately found the EXEs that I was after.

The steps required to create and import an SSL certificate to IIS for testing are:

1. Create a self-signed root authority certificate and export the private key
2. Install the root certificate into the Trusted Root Certificate Store
3. Make the Server Certificate for IIS
4. Export the certificate as a .PFX file, include all properties and private key
5. Import the certificate on IIS

## Create a self-signed root authority certificate and export the private key

Certificate Authorities, companies that create real SSL certificates create paths to certificates that can have 1 or more intermediate certificates.  This is done to reduce the possibility of private keys being compromised and making all certificates generated using that
private key no long trusted.  By having intermediates, they can have multiple private keys and reduce the impact of this possible loss of integrity.  There are certificates that do not have a CA not intermediate CAs, which carry different restrictions and technicalities.

To create the certificate and export the private key, enter the following from a Command prompt running with administrative privileges, also shown in Figure 1.

```makecert -n "CN=benjamin-perkins.me" -r -sv benperkmeCA.pvk benperkmeCA.cer```

![create CA certificate and export the private key using MAKECERT][FIGURE1]
###### Figure 1, create CA certificate and export the private key using MAKECERT

When you execute the command, you will be asked to create and enter a password for the private key.  You do get the option to not have one, but don’t recommend this even for testing.  With real SSL certificates, I.e. not self-signed or test ones, you would always want to have a password, so let’s try to mimic how it would work when we really do it.  Figure 2 illustrates the password requests pop-ups.

![MAKECERT password request dialog][FIGURE2]
###### Figure 2, MAKECERT password request dialog

## Install the root certificate into the Trusted Root Certificate Store

You can do this using MMC and the Certificate Management console, but let’s use the command prompt first.  To add the CA to the Trusted Root Certificate Store, execute the following from a Command prompt running as an Administrator and as shown in Figure 3.

```certmgr.exe -add -all -c "benperkmeCA.cer" -s -r localMachine Root```

![install the CA into the Trusted Root Certificate Store using CERTMGR][FIGURE3]
###### Figure 3, install the CA into the Trusted Root Certificate Store using CERTMGR

Enter CERTMGR from the command or open the Certificate manager using MMC and look at the Trusted Root Certificate Authorities tab or folder.  You will find the certificate present in the list and shown in Figure 4 and Figure 5.

![view certificates from CERTMGR.exe][FIGURE4]
###### Figure 4, view certificates from CERTMGR.exe

![view certificates from the Certificate Manager within MMC][FIGURE5]
###### Figure 5, view certificates from the Certificate Manager within MMC

## Make the Server Certificate for IIS

This article will focus on creating a server certificate used for HTTPS on an IIS server.  You can also create client certificates using similar MAKECERT commands, but this article won’t cover that.  I do plan to write another about that soon.  To create the test server certificate for use with IIS, execute the following command and as shown in Figure 6.  See here for instructions on how to make a SHA256 certificate.

```makecert -pe -iv benperkmeCA.pvk -n "CN=benjamin-perkins.me" -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -ic BenperkmeCA.cer IIS-ServerCert-Benperk.cer```

![create a test IIS SSL Server Certificate using MAKECERT][FIGURE6]
###### Figure 6, create a test IIS SSL Server Certificate using MAKECERT

You will be prompted to enter the password you created in step 1, Figure 2.  Once entered the IIS Server Certificate is created.  Once created you can then see it in the Certificate Management Console with MMC as shown in Figure 7.

![view the IIS SSL certificate in MMC certificate manager][FIGURE7]
###### Figure 7, view the IIS SSL certificate in MMC certificate manager

I will quickly jump to IIS and show you the window you get when you want to import an SSL certificate for use.  Notice in Figure 8, which is rendered from the IIS management console that it is looking for a .PFX file.  However, if you look at the command run in Figure 6, a .CER file was created.  To get the .PFX file for import into IIS, we need to export the certificate from the certificate management console shown in Figure 7.

## Export the certificate as a .PFX file, include all properties and private key

To export the .CER file you just created as a .PFX file, right click on the certificate, All Tasks -> Export… as shown in Figure 8.

![export the certificate from the MMC certificate manager][FIGURE8]
###### Figure 8, export the certificate from the MMC certificate manager

When the Export menu item is selected, an export wizard is run, on the first window read through the information and click the next button, the window shown in Figure 9 is rendered.  Select the radio button “Yes, export the private key” and then click the next button.

![export certificate wizard, export the private key][FIGURE9]
###### Figure 9, export certificate wizard, export the private key

In the next windows, as illustrated by Figure 10, click the “Export all extended properties” check box, leave all other settings as default and click the next button.  I didn’t test this one but by doing this the export likely includes the CA which will be needed when we import the certificate into IIS.  Check it out and let me know what happens, if anything when you do not check this one.  Thanks in advance.  If this is not the case, you might see something like that shown in Figure 14, after the certificate is imported on IIS.

![export certificate wizard, export all extended properties][FIGURE10]
###### Figure 10, export certificate wizard, export all extended properties

Add a password, as shown in Figure 11 and click the next button.

![export certificate wizard, set password][FIGURE11]
###### Figure 11, export certificate wizard, set password

Define the name and location of your .PFX file and click the next button, as shown in Figure 12.

![export certificate wizard, save the PFX file][FIGURE12]
###### Figure 12, export certificate wizard, save the PFX file

Complete the wizard.  A message box is rendered stating that the export was successful.

## Import the certificate on IIS

To install the certificate on IIS, copy the .PFX file and place it in a secure location which is accessible from the server.  Then copy the .PFX file onto the server and open the IIS management console.  At the server level, click on the Serve Certificates feature as shown on Figure 13.

![the Server Certificate feature in the IIS Management console][FIGURE13]
###### Figure 13, the Server Certificate feature in the IIS Management console

Once in the  Server Certificates feature, click on the Import… link on the Action pane, as illustrated on Figure 14, fill in the certificate details and press OK to import the certificate.

![import the SSL certificate into IIS for binding requests to HTTPS][FIGURE14]
###### Figure 14, import the SSL certificate into IIS for binding requests to HTTPS

Once imported, you will see the certificate in the feature.  You can double-click on the certificate and view the details.  Click on the Certificate path and make sure the path and status show as OK.  If you see something like that shown in Figure 15, you need to install the CA certificate or any intermediate certificate required for the chain or path to be valid.  Recall from step 1, Figure 1 where we created the CA.  To get this to work you need to export the .CER file (DER) and import it onto the IIS server using the MMC Certificate Management console.

![Checking out the certificate status and path][FIGURE15]
###### Figure 15, Checking out the certificate status and path

Once you get the CA certificate created in step 1 of this article, open the certificate details and view the path and status, all is OK as shown in Figure 16.

![OK path and status certificate properties][FIGURE16]
###### Figure 16, OK path and status certificate properties

After doing this exercise, I feel comfortable with certificate and understand how to make and install them.  There is lots more to learn before I start hitting the boundaries of the technology, but I know they are there and it is only a matter of time………….and effort.


[FIGURE1]: ../images/2014/msdn-0366.png "Figure 1, create CA certificate and export the private key using MAKECERT"
[FIGURE2]: ../images/2014/msdn-0367.png "Figure 2, MAKECERT password request dialog"
[FIGURE3]: ../images/2014/msdn-0368.png "Figure 3, install the CA into the Trusted Root Certificate Store using CERTMGR"
[FIGURE4]: ../images/2014/msdn-0369.png "Figure 4, view certificates from CERTMGR.exe"
[FIGURE5]: ../images/2014/msdn-0370.png "Figure 5, view certificates from the Certificate Manager within MMC"
[FIGURE6]: ../images/2014/msdn-0371.png "Figure 6, create a test IIS SSL Server Certificate using MAKECERT"
[FIGURE7]: ../images/2014/msdn-0372.png "Figure 7, view the IIS SSL certificate in MMC certificate manager"
[FIGURE8]: ../images/2014/msdn-0373.png "Figure 8, export the certificate from the MMC certificate manager"
[FIGURE9]: ../images/2014/msdn-0374.png "Figure 9, export certificate wizard, export the private key"
[FIGURE10]: ../images/2014/msdn-0375.png "Figure 10, export certificate wizard, export all extended properties"
[FIGURE11]: ../images/2014/msdn-0376.png "Figure 11, export certificate wizard, set password"
[FIGURE12]: ../images/2014/msdn-0377.png "Figure 12, export certificate wizard, save the PFX file"
[FIGURE13]: ../images/2014/msdn-0378.png "Figure 13, the Server Certificate feature in the IIS Management console"
[FIGURE14]: ../images/2014/msdn-0379.png "Figure 14, import the SSL certificate into IIS for binding requests to HTTPS"
[FIGURE15]: ../images/2014/msdn-0380.png "Figure 15, Checking out the certificate status and path"
[FIGURE16]: ../images/2014/msdn-0381.png "Figure 16, OK path and status certificate properties"

[LINK1]: http://blogs.msdn.com/b/asiatech/archive/2012/02/06/create-certificates-via-commands-for-testing-purpose.aspx
[LINK2]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa386968(v=vs.85).aspx
[LINK3]: http://msdn.microsoft.com/en-us/library/windows/desktop/aa376553(v=vs.85).aspx
