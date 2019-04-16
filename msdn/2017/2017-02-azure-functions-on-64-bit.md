# Azure Functions on 64 bit

Until this is updated in the portal to allow this, you can read how to enable that at the below links:

+ Allow 64 bit to be enabled in the UI
+ Set Azure Function App on Consumption Plan to 64 bit

In summary:

+ Access resource manager here
+ Navogate to the Function App –> subscription/resourceGroups/resourceGroupName/providers/Microsoft.Web/sites/functionaAppName/config/web/use32BitWorkerProcess
+ Set that value to ‘false’

As seen in Figure 1, set the Azure Resource Explorer into Read/Write mode and then click the Edit button.  It will change to the support a PUT verb.

![how to make a Function App 64 bit][FIGURE1]
###### Figure 1, how to make a Function App 64 bit

Then set the use32BitWorkerProcess to false, as seen in Figure 2.

![how to make a Function App 64 bit][FIGURE2]
###### Figure 2, how to make a Function App 64 bit

As mentioned on the GitHub page this will probably be enabled from the portal in the coming time, but wanted to share this any way, I kind of like the Azure Resource Explorere and take any opportunity to document my learning about it.

At the moment you can indeed see that the option is disabled from the portal,Figure 3.

![how to make a Function App 64 bit][FIGURE3]
###### Figure 3, how to make a Function App 64 bit

[FIGURE1]: ../images/2017/msdn-1159.png "Figure 1, how to make a Function App 64 bit"
[FIGURE2]: ../images/2017/msdn-1160.png "Figure 2, how to make a Function App 64 bit"
[FIGURE3]: ../images/2017/msdn-1161.png "Figure 3, Feature"
