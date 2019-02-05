# Application Request Routing (ARR) – HTTP Error 400.0 – Bad Request

I was working on setting up an ARR solution the other day and I ran into this error, and shown in Figure 1.

The request cannot be routed because it has reached the Max-Forwards limit.  The server may be self-referencing itself in request routing topology.

![HTTP Error 400.0 - Bad Request][FIGURE1]
###### Figure 1, HTTP Error 400.0 - Bad Request

The message pretty much explains the issue.  I had added the server that was hosting my ARR instance to the list of servers that were going to manage the load.  Figure 2 shows that on my server WIN-99*** where I had installed ARR, that I had included that same server in the Servers list.

![ARR Servers list][FIGURE2]
###### Figure 2, ARR Servers list

I removed the WIN-99*** from the list and the request was routed to the other server.  I later added additional servers to the Servers list so that the load is balanced across more than a single instance.

[FIGURE1]: ../images/2012/msdn-0169.png "Figure 1, HTTP Error 400.0 - Bad Request"
[FIGURE2]: ../images/2012/msdn-0170.png "Figure 2, ARR Servers list"
