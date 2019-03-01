# How to add a Server Core IIS server to a domain

I have been messing around with Server Core, this is a much friendlier version when I need to closely manage physical hardware resources like CPU and memory.  Plus, there are fewer updates that I need to install with the much more limited OS foot print.

As I buld my IIS servers to test a reproduction of an issue, I need to add them to a domain so that I can perform some of the necessary configuration.  Here is how I did it.

First as shown in Figure 1, I needed to find out the name of my machine, once it had been built.  I entered the command ‘***hostname***’ which returned the required value.  It is possible to rename the host, but wanted to avoid any conflicts and left the default name.

![getting the IIS server name, server core, hostname][FIGURE1]
###### Figure 1, getting the IIS server name, server core, hostname

Next, I used the ‘***netdom join***’ command, providing the hostname, domain and a user id and password of an account with the permission to add machines to the domain, as shown in Figure 2.

![dding a server core machine to a domain][FIGURE2]
###### Figure 2, dding a server core machine to a domain

Lastly, it is required then to perform a reboot to complete the process.  I entered ‘shutdown /r’ to reboot the server core machine, as shown in Figure 3.

![how to restart/reboot a server core server][FIGURE3]
###### Figure 3, how to restart/reboot a server core server

That was it.

If I ever wanted to remove the server core server from the domain, I would use the ‘***netdom remove***’ command as shown in Figure 4.

![ow to remove a server core server from a domain][FIGURE4]
###### Figure 4, ow to remove a server core server from a domain

[FIGURE1]: ../images/2015/msdn-0569.png "Figure 1, getting the IIS server name, server core, hostname"
[FIGURE2]: ../images/2015/msdn-0570.png "Figure 2, dding a server core machine to a domain"
[FIGURE3]: ../images/2015/msdn-0571.png "Figure 3, how to restart/reboot a server core server"
[FIGURE4]: ../images/2015/msdn-0572.png "Figure 4, ow to remove a server core server from a domain"
