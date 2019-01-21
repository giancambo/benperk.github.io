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
The preceding code, if set on a 64bit operating system, means that the worker process or processes will serve the application pool in WOW64 (Windows on Windows64) mode.  This means that only 32bit processes will be loaded.
Sometimes, when installing a 3rd party program, modifications to the applicationHost.config file, similar to the following are made.
```
<applicationPoolDefaults enable32BitAppOnWin64="true">
  <processModel identityType="ApplicationPoolIdentity" />
</applicationPoolDefaults>
```
What this would mean is that if you have application pools on the 64bit machine which need to load a 64bit program, that it will no longer work, unless you have previously specifically set the enable32BitAppOnWin64 to false.  Most of the time this is not the case because default (false) is used, no setting exists and therefore when the applicationPoolDefault is set, then things start to go unexpected.
If this happens, simply access the application pools which need to run in 64bit mode and manually set the enable32BitAppOnWin64 to false, or remove the default setting and set the individual application pools which need to run in 32bit mode to true.

[FIGURE1]: images/msdn-0001.png "Figure 1, advanced IIS application pool settings"
