# How (I) back up my Azure Blob Storage container

I run a little website for fun and I store PNGs images into an Azure Blob Storage container. I also reference the PNGs directly from within the site. It is a small site and I have up until now about 1000 images in the container, it would be a catastrophe if I ever lost them, even though there are so few. So I need to back them up and keep them in a safe place, just in case.

Looking for a simple solution was not so easy for me, there are some serious solutions for backing up your Azure Blob Storage container for customer who have millions or billions of blobs, but I just didn’t think I need to write code or use a dedicated tool to achieve that, I only have 1000 images, after all.

What I found is that I can save the contents of my Azure Blob container using Visual Studio and the Azure SDK.
I downloaded the appropriate SDK from here. In my case, I was using Visual Studio 2013, so I chose VC 2013. Once installed I can click the menu item View -> Cloud Explorer and then navigate to the Azure Blob storage container for backing up.

When the contents of the container are listed on the container tab, click on it and press CTRL+A, to select all the blobs, see Figure 1. Press the SAVE button which will prompt you for a place to save the blobs and once you press OK, the blobs are downloaded.

![how to back up your Azure Blog Storage container, qucik, easy and simple][FIGURE1]
###### Figure 1, how to back up your Azure Blog Storage container, qucik, easy and simple

I then zipped up the contents and placed them in a safe place, like on my One Drive.

[FIGURE1]: ../images/2016/msdn-0648.png "Figure 1, how to back up your Azure Blog Storage container, qucik, easy and simple"
