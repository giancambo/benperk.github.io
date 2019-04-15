# Azure CLI ‘config’ is not an azure command

I was writing this article here where I tested out installing Azure CLI, logging in and some Azure App Service commands and many of the examples I found were using the CONFIG command when configuring an App Service Plan.

I received this error when I execute a command with CONFIG:

```
error: 'config' is not an azure command.  See 'azure help'.
The current mode is: are (Azure Resource Manager).
```

![error: 'config' is not an azure command.  See 'azure help'.][FIGURE1]
###### Figure 1, error: 'config' is not an azure command.  See 'azure help'.

I looked at the help as suggested using this Azure CLI command:

```azure appserviceplan -h```

![See 'azure help'. how to get some help with the Azure CLI][FIGURE2]
###### Figure 2, See 'azure help'. how to get some help with the Azure CLI

And sure enough, the CONFIG command no longer or doesn’t exists.  Perhaps it got replaced with the SET command.  To find that out I checked by issuing the following command:

```azure appserviceplan set -h```

![See 'azure help'. how to get some help with the Azure CLI][FIGURE3]
###### Figure 3, See 'azure help'. how to get some help with the Azure CLI

And it does include the options I needed to successfully execute the command I was attempting here.

[FIGURE1]: ../images/2017/msdn-1143.png "Figure 1, error: 'config' is not an azure command.  See 'azure help'."
[FIGURE2]: ../images/2017/msdn-1144.png "Figure 2, See 'azure help'. how to get some help with the Azure CLI"
[FIGURE3]: ../images/2017/msdn-1145.png "Figure 3, Feature"
