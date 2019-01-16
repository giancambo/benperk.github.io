# Create an IIS configuration backup
The first step to take when making changes to a system is to backup what you currently have so that if there are problems with the system after the changes have applied, you have time constraints and can’t get the system to work, then you have at least a backup of the systems state to reapply if required.

I don't generally like to rollback, none of us do, simply because of all the effort, planning and sacrifices required to get to the point where a release is possible. Nonetheless, always have a back-out plan...always.

To backup the IIS configuration using IIS 7+ machine perform the following.

Start a *Command Prompt* as an Administrator by right-clicking the icon and selecting *Run As Administrator*. Navigate to the windows working directory and then to the systems32\inetsrv and run the following command.

`appcmd add backup cleaninstall`

You should receive the below result:

![appcmd add backup cleaninstall][Figure1]

To rollback to the backed up configuration start a *Command Prompt* as an Administrator by richt-clicking the icon and selecting *Run As Administrator*. Navigate to the windows working directory and then to the system32\inetsrv directory and run the following command.

`appcmd restore backup cleaninstall`

You should receive the below result:

![appcmd restore backup cleaninstall](images/4848.2.png "Figure 2, appcmd restore backup cleaninstall")

After the restore, the IIS configuration will be in the state it was when the backup was originally taken.

[Figure1]: https://github.com/benperk/benperk.github.io/blob/master/msdn/images/5657.1.png "FIgure 1, appcmd add backup cleaninstall"
