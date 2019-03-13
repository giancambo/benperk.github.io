# How to delete a Web App backup

Backing up your Web App and associated database is something you need to take very seriously. You never know when something might happen that requires a restoration of a backup. For example, the deployment of some bad code or the accidental removal of numerous files. Individuals or companies running IT services without backups, are taking a huge risk which could result in loss of visitors and customers.

Overtime, as your Web App grows, so would the size and number of your backups. Some housekeeping is necessary to maintain the integrity and usability of your backups. Do you need a backup of a Web App that is multiple revisions old? If I ever had to roll back, I would only roll back to the most recent one. There may be some reasons to keep some older ones, but not so many.

[Here][LINK1] is a nice article about Web App backups.

To remove some of your Web App backups, first determine where you are storing them. As shown in Figure 1, the Web App ‘deletebackup’ is storing the backups into the ‘deletebackup’ STORAGE ACCOUNT. You can also run a one-time backup from this page, by clicking on the BACKUP NOW button.

![where is my Web App backup stored, how to take a backup of my Web App][FIGURE1]
###### Figure 1, where is my Web App backup stored, how to take a backup of my Web App

If you do decide to run a backup, you can view the operation logs accessible from the Web App DASHBOARD as shown in Figure 2.

![viewing the Operations Logs for a Web App backup][FIGURE2]
###### Figure 2, viewing the Operations Logs for a Web App backup

Clicking on that link navigates to the management services page which shows the status of the backup, as illustrated in Figure 3. Once complete, the status will show as Succeeded where operation is BackupWebSite.

![Web App backup status and operational checkup][FIGURE3]
###### Figure 3, Web App backup status and operational checkup

Once you have a backup, navigate to the Storage Account where it is stored. As shown in Figure 4, a container named ‘websitebackups’ is created. The same is visible in the new Azure Portal shown in Figure 5.

![Web App backups are stored into a container called ‘websitebackups’][FIGURE4]
###### Figure 4, Web App backups are stored into a container called ‘websitebackups’

![finding the storage account for my Web App backup][FIGURE5]
###### Figure 5, finding the storage account for my Web App backup

As shown in Figure 6 and Figure 7, clicking on the ‘websitebackups’ container renders the list of Web App backups. Perhaps you can select the oldest backups and delete them or delete the backups which you are certain are no longer needed.

![delete a Web App backup][FIGURE6]
###### Figure 6, delete a Web App backup

![delete a Web App backup in the new Azure Management portal][FIGURE7]
###### Figure 7, delete a Web App backup in the new Azure Management portal

***NOTE***: At the moment a Web App backup is limited to 10GB, this includes the database size, which is limited to 1GB. Additionally, I have seen issues when the total space taken for all Web App backups is greater than 100GB. So it behooves you to maintain the backups. Yes, you can manage the retention days for the Web App backups, however, some additional maintenance may require this type of manual intervention. I had this experience and is the reason for writing this article.

In conclusion, take backups, manage your backups, have a plan for capturing, storing and maintaining them. Take the time to practice the recovery of them on a test Web App, so when the time comes and you need a restore, you’re not learning how to do it under pressure.

[FIGURE1]: ../images/2016/msdn-0641.png "Figure 1, where is my Web App backup stored, how to take a backup of my Web App"
[FIGURE2]: ../images/2016/msdn-0642.png "Figure 2, viewing the Operations Logs for a Web App backup"
[FIGURE3]: ../images/2016/msdn-0643.png "Figure 3, Web App backup status and operational checkup"
[FIGURE4]: ../images/2016/msdn-0644.png "Figure 4, Web App backups are stored into a container called ‘websitebackups’"
[FIGURE5]: ../images/2016/msdn-0645.png "Figure 5, finding the storage account for my Web App backup"
[FIGURE6]: ../images/2016/msdn-0646.png "Figure 6, delete a Web App backup"
[FIGURE7]: ../images/2016/msdn-0647.png "Figure 7, delete a Web App backup in the new Azure Management portal"

[LINK1]: https://azure.microsoft.com/en-us/documentation/articles/web-sites-backup/
