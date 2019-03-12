# How to find your outgoing Azure App Service IP address

If you have ever wondered how to find the outgoing IP address of your Web App, Mobile App, API App or Logic App, look no further.

To see how to find the OUTBOUND IP ADDRESSES from the Azure portal, view Video 1 (removed).

###### Video 1, find outgoing, outbound IP addresses from the portal

You can also capture them from the Azure Resource Explorer [here][LINK1], then navigate to the information as discussed in Video 2 (removed).

###### Video 2, find outgoing, outbound IP addresses using the Azure Resource Explorer

***possibleOutboundIpAddresses*** -> if you are using Pv2 as announced [here][LINK2], there may additional outgoing IP addresses allocated.  Perhaps 1 or 2 additional.  When I scaled, I did not get them and remained at 4.  Keep that in mind if you scale from version 1 (Premium, Standard, Basic, etc...) Premium V2 that these IPs may be added added.  If they are added, you will see them in the portal or in resource explorer.

[LINK1]: https://resources.azure.com/
[LINK2]: https://azure.microsoft.com/en-us/blog/azure-app-service-premium-v2-in-public-preview/
