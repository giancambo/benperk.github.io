# Web Farm Framework Architecture

I am very excited about the Web Farm Framework (WFF).  Having administered many web farms in my past, this technology resolves a lot of the activities which where a burden.  This blog will show a standard architectural configuration of a WFF environment.  Figure 1 shows the controller server, the primary server and the multiple secondary servers.

![WFF example environment configuration][FIGURE1]
###### Figure 1, WFF example environment configuration

The controller server is the server where the Web Farm Framework is installed onto.  It provides the administrator with an overview interface, accessed via the Internet Information Services Management console, of the current status of the web farm. The primary server is the server which is used to build/mirror all of the other servers in the web farm.  Changes made to the primary server will be replicated to all the secondary servers.
The secondary servers are mirror images of the primary server.  These servers are there to help manage the user load, where the load on a web application is too great to be processed by a single server.

[FIGURE1]: ../images/2012/msdn-0054.png "Figure 1, WFF example environment configuration"
