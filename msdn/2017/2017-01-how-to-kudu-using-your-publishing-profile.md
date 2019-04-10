# How to access KUDU using your Publishing Profile

If for some reason your Microsoft or Organizational credentials are not working when accessing KUDU, which I discuss here.  You can use your Publishing Profile userName and userPWD to connect.  You may get prompted with a page similar to that shown in Figure 1.

![cannot access KUDU][FIGURE1]
###### Figure 1, cannot access KUDU

Login to the Azure Portal here, navigate to the Azure App Service Web App which you would like to access the KUDU instance for and select the …More link then Get Publish profile, as shown in Figure 2.

![download Publish Profile][FIGURE2]
###### Figure 2, download Publish Profile

Once downloaded, open the *.PublishSettings file and look for the attributes named userName and userPWD and use them for the challenge/response pop-up, Figure 3, after clicking on the PUBLISHING CREDENTIALS link as shown previously in Figure 1.  Use the address similar to this to get the challenge/response window:  https://<yourSite>.scm.azurewebsites.net/basicauth

![access KUDU using Publish Profile Credentials][FIGURE3]
###### Figure 3, access KUDU using Publish Profile Credentials

Once entered you are logged into KUDU.

![access KUDU using Publish Profile Credentials][FIGURE4]
###### Figure 4, access KUDU using Publish Profile Credentials

[FIGURE1]: ../images/2017/msdn-1096.png "Figure 1, cannot access KUDU"
[FIGURE2]: ../images/2017/msdn-1097.png "Figure 2, download Publish Profile"
[FIGURE3]: ../images/2017/msdn-1098.png "Figure 3, access KUDU using Publish Profile Credentials"
[FIGURE4]: ../images/2017/msdn-1099.png "Figure 4, access KUDU using Publish Profile Credentials"
