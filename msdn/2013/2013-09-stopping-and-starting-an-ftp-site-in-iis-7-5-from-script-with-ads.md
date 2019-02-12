# Stopping and starting an FTP site in IIS 7.5+ from script with ADSI

There have been many changes over the years with managing the IIS configuration using scripting.  In IIS 6 using WMI which relies on the ADSI model is the recommended approach.  However, with the introduction of IIS 7.5 and now IIS 8 there is a new way of doing things.  This new way is via the Microsoft.ApplicationHost.AdminManager object.

Although some of the IIS manageability features are still available through the metabase compatibility feature, FTP is not one of those features.  As this feature is not supported on IIS 7.5/8 the below script, shown in Snippet 1, that works on IIS 6 will not work on IIS 7.5 or IIS 8.

```
Dim objWmiProvider
Dim colItems

Set objWMIService = _
	GetObject("winmgmts:{authenticationLevel=pktPrivacy}\\.\root\microsoftiisv2")
Set colItems = objWMIService.ExecQuery("Select * from IIsFtpServer")

For Each objItem in colItems
  if (objItem.Name + "MSFTPSVC/46") Then
	WScript.Echo objItem.Name
	objItem.Start
	If (Err.Number <> 0) Then
	  WScript.Echo Err.Number
	End If
  End If
Next
```
###### Snippet 1, Start an FTP site in IIS 6

To start an FTP site on an IIS 7.5 or IIS 8 instance, use the code shown in Snippet 2 instead.

```
Set adminManager = createObject("Microsoft.ApplicationHost.AdminManager") 

Set sitesSection = adminManager.GetAdminSection("system.applicationHost/sites", "MACHINE/WEBROOT/APPMOST") 
Set sitesCollection = sitesSection.Collection 

For siteCount = 0 To CInt(sitesCollection.Count)-1 
  isFtpSite . False 
  Set siteElement sitesCollection(siteCount) 
  Set bindingsCollection = siteElement.ChildElements.Item("bindings").Collection 
  For bindingsCount = 0 To CInt(bindingsCollection.Count)-1 
    Set bindingElement = bindingsCollection(bindingsCount) 
      If StrComp(CStr(bindingElement.Properties.Item("protocol").Value),"ftp",vbTextCompare).0 Then 
        isFtpSite = True 
        Exit For
      End If
  Next 
  If isFtpSite = True Then 
    Set ftpServerElement = siteElement.ChildElements.Item("ftpServer") 
    Set stopFtpSite = ftpServerElement.Methods.Item("Stop").CreateInstance() 
    Set startFtpSite ftpServerElement.Methods.Item("Start").CreateInstance() 
    stopFtpSite.Execute() 
    startFtpSite. Execute() 
  End If 
Next
```
###### Snippet 2, Start all FTP websites on IIS 7.5 or IIS 8

If you wanted to stop only a single website based on the name of the FTP site, create a FindElement function, for example which will search through sitesCollection and return the FTP sites position.  Then you can stop only that FTP site.  Snippet 3 is a sample of the method and the parameters.

```
siteElementPos = FindElement(siteCollection, "site", Arrzy("name", ftp.contoso.com"))
```
###### Snippet 3, Sample example of a FindElement method, stop a single FTP site in IIS 7.5 or IIS 8
