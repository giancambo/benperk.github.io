# Using Postman to call Azure REST APIs

I had a need to call an Azure REST API and thought I would document that here.  Here are the steps required:

+ Get the Postman app
+ Get your Bearer Token for authentication
+ Configure Postman for calling the Azure Rest API

## Get the Postman app

In this instance I used Chrome and installed the app.  Once installed I saw the following, Figure 1 in the browser.

![Postman for calling Azure REST APIs][FIGURE1]
###### Figure 1, Postman for calling Azure REST APIs

Next I clicked on Postman to open the console which resulted in something like the following, Figure 2.

![open Postman for sending REST API requests to Azure][FIGURE2]
###### Figure 2, open Postman for sending REST API requests to Azure

## Get your Bearer Token for authentication

There are numerous ways to get this but I chose to use Fiddler.  After enabling the application to decrypt HTTPS traffic, as shown in Figure 3 I was able to capture that.

![enable HTTPS decryption in Fiddler][FIGURE3]
###### Figure 3, enable HTTPS decryption in Fiddler

Once HTTPS decryption was enabled, I accessed the Azure portal here and navigated to the location I wanted to call the REST API for.  For example, I wanted to get some information about one of my Web Apps.  Sometimes you just have to know what you are looking for, I found lots of information on how to do this on other post, but like always I like to document and share my experiences.  I knew I needed to call https://management.azure.com so this is what I looked for in the Fiddler traces, and I found many, as seen in Figure 4.

![get the Bearer Authentication Token for calling an Azure REST API][FIGURE4]
###### Figure 4, get the Bearer Authentication Token for calling an Azure REST API

Select the Authorization: Bearer token and copy/paste it into notepad, don’t share this with anyone and store it only in a secure location.

## Configure Postman for calling the Azure Rest API

Go back into Postman as shown previously in Figure 2, and make the following configurations, also shown in Figure 5:

+ Add the URL
+ Add the Authorization and Content-Type header
+ Press the Send button

## Add the URL

The URL I used was:

+ Hostname:   https://management.azure.com (public azure cloud) or https://management.microsoftazure.de (Azure Germany Cloud)
+ Path:  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Web/sites/<appName>?api-version=2015-08-01

## Add the Authorization and Content-Type header

In Postman, select the Headers tab and add the 2 headers (Authentication and Content-Type).  Use the Bearer token you got in the previous section as the value of the Authentication header, be sure to include the word ‘Bearer’ itself along with the big long string of random looking characters.  Also add the application/json value for the Content-Type header.

## Press the Send button

Once the configurations are made, hit the Send button and you will get a JSON result, just as expected, also seen in Figure 5.

![GET some information from managment.azure.com using a REST API from Postman][FIGURE5]
###### Figure 5, GET some information from managment.azure.com using a REST API from Postman

You could also execute some POST commands to perform configuration operations that would normally be performed within the client, but that is for later posts.

[FIGURE1]: ../images/2017/msdn-1081.png "Figure 1, FeaPostman for calling Azure REST APIsture"
[FIGURE2]: ../images/2017/msdn-1082.png "Figure 2, open Postman for sending REST API requests to Azure"
[FIGURE3]: ../images/2017/msdn-1083.png "Figure 3, enable HTTPS decryption in Fiddler"
[FIGURE4]: ../images/2017/msdn-1084.png "Figure 4, get the Bearer Authentication Token for calling an Azure REST API"
[FIGURE5]: ../images/2017/msdn-1085.png "Figure 5, GET some information from managment.azure.com using a REST API from Postman"
