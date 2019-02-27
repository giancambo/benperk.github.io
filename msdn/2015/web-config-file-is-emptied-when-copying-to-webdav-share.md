# Web.config file is emptied when copying to WebDAV share

When you configure a WebDAV site, it is not only accessible via the NET USE command to map the drive in Windows Explorer, it is also accessible via a browser.  Because of this it is not safe to allow users or customers to upload web.config files that would possibly impact the availability, functionality or security of the WebDAV IIS site.  Therefore, when copying a web.config file to a WebDAV share, the contents are validated and can be removed to protect the integrity of the website.  This is by design.

I have read a few conversations about this where some touch on the [enableConfigurationOverride][LINK1] property, but this does not resolve the issue.  This parameter, similar to the [allowSubDirConfig][LINK2], instruct IIS to not use the contents of the web.config when building the [effective configuration].  It is possible to configure IIS so that the contents of the web.config are not used, however it is not possible to stop the validation of its contents, and if the validation process finds something invalid, then the contents are emptied.

Here is one example of how I found and resolved this issue. 

I received a web.config file that was being emptied and created a reproduction.  After some trial and error I ultimately found which parameter was invalid, shown in Listing 1.

###### Listing 1, web.config with invalid setting

```
<?xml version="1.0"?>
<configuration xmlns="http :// schemas.microsoft.com/.NetConfiguration/v2.0">
  <appSettings />
  <system.web>
    <webServices>
      <soapExtensionTypes>
        <add type="Namespace.Common.WebService.Extension, Namespace.Common.WebService, 
             Version=1.0.0.0, Culture=neutral, PublicKeyToken=**??**??**??****?"
             priority="1" group="0" />
      </soapExtensionTypes>
    </webServices>
  </system.web>
</configuration>
```

The issue is where group=”0”, and in contrast, as shown in Figure 1, the valid values are either High or Low.

![valid values for the invalid web.config, WebDAV validation][FIGURE1]
###### Figure 1, valid values for the invalid web.config, WebDAV validation

When I changed the “0” to Low or High, the validation passed and the web.config was no longer emptied.  Success.

I thought some more on this, thinking about where IIS is getting the 'group' values from.  I.e. where does it say that “High” and “Low” are the only allowed values, the answer is, for this case, they are in the C:\Windows\System32\inetsrv\config\schema\ASPNET_schema.xml file, as shown in Listing 2.

###### Listing 2, setting values for properties via the Configuration Editor.

```
<element name="soapExtensionTypes">
  <collection addElement="add" removeElement="remove" clearElement="clear">
    <attribute name="group" isCombinedKey="true" type="enum" defaultValue="Low">
      <enum name="High" value="0" />
      <enum name="Low" value="1" />
    </attribute>
    <attribute name="priority" isCombinedKey="true" type="int" 
               validationType="integerRange" validationParameter="0,2147483647" 
               defaultValue="0" />
    <attribute name="type" isCombinedKey="true" type="string" />
  </collection>
</element>
```

I thought about [extending][LINK4] the ASPNET_schema, but in the end I decided it was a bad idea because the source code is most likely looking for High or Low, and even if I were to get '0' to work, it would likely not function as expected.  As I read it, the extension functionality is for custom properties and not IIS or related elements and attributes

[FIGURE1]: ../images/2015/msdn-0483.png "Figure 1, valid values for the invalid web.config, WebDAV validation"

[LINK1]: http://msdn.microsoft.com/en-us/library/aa347554(v=vs.90).aspx
[LINK2]: http://msdn.microsoft.com/en-us/library/ms689463(v=vs.90).aspx
[LINK3]: ../2012/2012-09-debug-true.md
[LINK4]: http://www.iis.net/learn/develop/extending-iis-configuration
