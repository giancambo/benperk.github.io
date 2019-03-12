# HTTP 500 Azure App Service Web App

When accessing your Azure App Service Web App, if you receive an HTTP 500 response, similar to that shown in Figure 1, first step is to add or modify the customErrors attribute and set the mode property to Off, as shown in Figure 2.

![Azure Web App returns a 500][FIGURE1]
###### Figure 1, Azure Web App returns a 500

![change the customErrors attribute mode property to Off][FIGURE2]
###### Figure 2, change the customErrors attribute mode property to Off

Then, refresh the page an you will likely get the line on which the exception is being thrown and you can debug from there.

[FIGURE1]: ../images/2016/msdn-0617.png "Figure 1, Azure Web App returns a 500"
[FIGURE2]: ../images/2016/msdn-0618.png "Figure 2, change the customErrors attribute mode property to Off"
