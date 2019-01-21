# Application Pool reset after installing 3rd party program
Understanding the differences between how and where 32bit and 64bit programs can run is, at the beginning, a little tricky.  Simply, 32bit programs can run on 32bit and 64bit machines, while 64bit programs can only run on 64bit machines.
When the topic concerns IIS application pools on 64bit machines, the context is the same.  If your website must load 32bit applications only, set the “Enable 32-Bit Applications” to true.  Alternatively, if you want your website to run in 64bit mode, set “Enable 32-Bit Applications” to false.
![advanced IIS application pool settings][FIGURE1]
You can do this by using the appcmd, powershell or from within the IIS management console.  All 3 methods update the applicationHost.config file (IIS7) as shown in the following code segment:
```
<applicationPools>
  <add name="AppPoolName" enable32BitAppOnWin64="true" />
</applicationPools>
```


[FIGURE1]: imagesmsdn-0001.png "Figure 1, advanced IIS application pool settings"
