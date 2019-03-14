# Setting Application Request Routing – ARR Affinity for your Azure App Service

ARR Affinity is enabled on your Azure App Service by default. This means that if you are running multiple instance of your App Service, clients will be directed to the same instance for each request. [Here][LINK1] is an article that contains more information on the subject. However, now it is possible to perform the enable or disable of this feature from within the Azure portal instead of making a manual configuration change to the web.config file.

Navigate to the App Service where you want to make the change and select Settings –> Application Settings.  As shown in Figure 1, you can now enable or disable ARR Affinity directly via the portal.

![set ARR Affinity within the portal][FIGURE1]
###### Figure 1, set ARR Affinity within the portal

I have seen some reports from customers where they are performing some performance testing and see slow or limited improvement after increasing the number of ASP instances.  In many cases it is because the client remains on the same single instance due to the default ARR Affinity setting.  If your App Service is state-less then you can disable ARR Affinity and would then get different results.

At the same time, the App Service front ends do balance using * algorithm, but as soon as a client is affinitized, it remains in that state until the session ends, new clients would/could be redirected to the other instances.

[FIGURE1]: ../images/2016/msdn-0659.png "Figure 1, set ARR Affinity within the portal"

[LINK1]: https://azure.microsoft.com/en-us/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
