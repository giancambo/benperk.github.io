# Azure Functions – Event Hub

The labs contained in this article show how to create, configure, code and monitor an Azure Function with an Event Hub.  There is a detailed document [here][LINK1] “Azure Event Hubs bindings for Azure Functions” which discusses the Event Hub trigger in detail so I will not readdress that content.  There is also a helpful article [here][LINK2] “Choose between Azure services that deliver messages” that discusses some difference between Event Grid ([X][LINK3]), Event Hub ([X][LINK4]) and Service Bus ([X][LINK5]).  As per that article, use an Event Hub when the following are essential.

| Purpose | Type | When to use |
| ------- | ---- | ----------- |
| Big data pipeline | Event streaming (series) | Telemetry and distributed data streaming| 

I have written an AzureFunctionConsumer program which I host on GitHub [here][LINK6].  You can use this code to consume the Azure Functions discussed in [this][LINK7] series of articles.

## Create an Event Hub

***NOTE*** – to complete this lab you will need an Event Hub Namespace and an Event Hub, follow [these][LINK8] instructions to create the namespace and hub.  For this lab, I have created something similar to that shown in Figure 1.

![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger


![Figure 1, how to create an Azure Function with an Event Hub trigger][FIGURE1]
###### Figure 1, how to create an Azure Function with an Event Hub trigger








[LINK1]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LINK2]: https://docs.microsoft.com/en-us/azure/event-grid/compare-messaging-services
[LINK3]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-grid
[LINK4]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-event-hubs
[LINK5]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-service-bus
[LINK6]: https://github.com/benperk/AzureFunctionConsumer
[LINK7]: azure-functions-labs-information-and-setup-instructions.md
[LINK8]: https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create

[FIGURE1]: ../images/2019/azure-0031.png "Figure 1, how to create an Azure Function with an Event Hub trigger"
[FIGURE2]: ../images/2019/azure-0032.png "Figure 2, how to create an Azure Function with an Event Hub trigger"
[FIGURE3]: ../images/2019/azure-0033.png "Figure 3, how to create an Azure Function with an Event Hub trigger"
[FIGURE4]: ../images/2019/azure-0034.png "Figure 4, how to create an Azure Function with an Event Hub trigger"
[FIGURE5]: ../images/2019/azure-0035.png "Figure 5, how to create an Azure Function with an Event Hub trigger"
[FIGURE6]: ../images/2019/azure-0036.png "Figure 6, how to create an Azure Function with an Event Hub trigger"
[FIGURE7]: ../images/2019/azure-0037.png "Figure 7, how to create an Azure Function with an Event Hub trigger"
[FIGURE8]: ../images/2019/azure-0038.png "Figure 8, how to create an Azure Function with an Event Hub trigger"
[FIGURE9]: ../images/2019/azure-0039.png "Figure 9, how to create an Azure Function with an Event Hub trigger"
[FIGURE10]: ../images/2019/azure-0040.png "Figure 10, how to create an Azure Function with an Event Hub trigger"
[FIGURE11]: ../images/2019/azure-0041.png "Figure 11, how to create an Azure Function with an Event Hub trigger"
[FIGURE12]: ../images/2019/azure-0042.png "Figure 12, how to create an Azure Function with an Event Hub trigger"
