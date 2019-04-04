# 500.19 Internal Server Error

I was building a new server to use while creating my [IIS labs][LINK1] here, and got this error shown in Figure 1.

![500.19 Internal Server Error][FIGURE1]
###### Figure 1, 500.19 Internal Server Error

These errors can be caused by numerous things, but in this case it was because I did not have ASP.NET installed

And as you can see, in Figure 2, when I look at the IIS Management console there are no ASP.NET features installed.

![ASP.NET features not see in the IIS Management console][FIGURE2]
###### Figure 2, ASP.NET features not see in the IIS Management console

Install the ASP.NET binaries as described in some detail [here][LINK2] and it will resolve the issue discussed in this article.

[FIGURE1]: ../images/2016/msdn-0972.png "Figure 1, 500.19 Internal Server Error"
[FIGURE2]: ../images/2016/msdn-0973.png "Figure 2, ASP.NET features not see in the IIS Management console"

[LINK1]: 2016-IISLAB-iis-debugging-labs-information-and-setup-instructions.md
[LINK2]: 2016-IISLAB-lab-2-install-the-web-platform-installer.md
