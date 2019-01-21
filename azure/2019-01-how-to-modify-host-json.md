# How to modify the host.json file for an Azure Function
It is not recommended to set the fileLoggingMode to always for very long as it will consume a lot of compute and storage space.  Use this only when troubleshooting.

![Figure 1, how to modify host.json for an Azure Function][FIGURE1]
 
 + Check out an older blog I wrote on this [here][LINK1] out too.
 + [Here][LINK2] is an example of a host.json file with all possible options.

[FIGURE1]: images/azure-0001.png "Figure 1, how to modify host.json for an Azure Function"

[LINK1]: https://blogs.msdn.microsoft.com/benjaminperkins/2018/06/12/how-to-add-a-host-json-file-to-an-azure-function/
[LINK2]: https://docs.microsoft.com/en-us/azure/azure-functions/functions-host-json#sample-hostjson-file
