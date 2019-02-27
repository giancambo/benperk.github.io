# Sending an email from Azure App Service using SendGrid

A common question I get is, “why doesn’t my SMTP email code that I wrote for my other website not work on Azure Web App?”  Additionally, I sometimes get this exception sent to me, “System.Net.Mail.SmtpException: Transaction failed. The server response was: #.#.# Service unavailable; Client host [*.*.*.*] blocked using; Blocked”.

I also wrote an article [here][LINK1] about sending emails using an SMTP server hosted with Office 365.  #O365 #Office365

The reason for the first issue is that currently, a common way to send an email from an Azure Web App is by using SendGrid.  You enable / subscribe to SendGrid via the Market Place by selecting the NEW link on the bottom left of the Azure Management console, similar to that shown in Figure 1.

![setup SendGrid for Azure App Services, SMTP not working][FIGURE1]
###### Figure 1, setup SendGrid for Azure App Services, SMTP not working

Once configured, you should see it in the list of available Market Place apps.  Clicking on the SendGrid app, will redirect you to a page similar to that illustrated in Figure 2.

![Configure SendGrid on an Azure App Services][FIGURE1]
###### Figure 1, Configure SendGrid on an Azure App Services

Clicking on the MANAGE link on the bottom navigation bar, opens the SendGrid website where you can get your user id and password.  Additionally you can find [many examples][LINK2] in many languages to get a proof of concept page working on your Azure App Services Web App site. 

Myself, being a strong C# programmer didn’t have much problem getting that example to work, but I did struggle with the PHP one.  [Here][LINK3] is a library you can use to get a PHP SendGrid solution up and running pretty quick.  Simply add the code to a PHP file, shown in Listing 1, and deploy it to root directory of the PHP library.

###### Listing 1, consume PHP SendGrid PHPMailer code, using SendGrid

```
<?php
require 'PHPMailerAutoload.php';
$mail = new PHPMailer;
//$mail->SMTPDebug = 3;                          // Enable verbose debug output
$mail->isSMTP();                                        // Set mailer to use SMTP
$mail->Host = 'smtp.sendgrid.net';             // Specify main/backup SMTP servers
$mail->SMTPAuth = true;                           // Enable SMTP authentication
$mail->Username = 'YOUR USERNAME';    // SMTP username
$mail->Password = 'YOUR PASSWORD';    // SMTP password
$mail->SMTPSecure = 'tls';                        // Enable TLS/SSL encryption
$mail->Port = 587;                                      // TCP port to connect to
$mail->From = 'email@contoso.com';
$mail->FromName = 'From SendGrid website';
$mail->addAddress('support@contoso.com', 'Support');     // Add a recipient
$mail->WordWrap = 50;                              // Set word wrap to 50 characters
$mail->isHTML(true);                                  // Set email format to HTML
$mail->Subject = 'Here is the subject';
$mail->Body    = 'This is the HTML message body <b>in bold!</b>';

if(!$mail->send()) {
  echo 'Message could not be sent.';
  echo 'Mailer Error: ' . $mail->ErrorInfo;
} 
else {
  echo 'Message has been sent';
}
?>
```

The second issue is because the IP address for your Azure App Services Web App has been blacklisted.  Some potential work arounds for this are:

+ [Configure an Azure VNet to VNet Connection][LINK4]
+ [Using VNET integration and Hybrid connections with Azure Web Apps (Websites)][LINK5]
+ [Introducing App Service Environment][LINK6]

[FIGURE1]: ../images/2015/msdn-0484.png "Figure 1, setup SendGrid for Azure App Services, SMTP not working"
[FIGURE2]: ../images/2015/msdn-0485.png "Figure 2, Configure SendGrid on an Azure App Services"

[LINK1]: https://blogs.msdn.microsoft.com/benjaminperkins/2017/01/11/sending-email-from-an-azure-web-app-using-an-o365-smtp-server/
[LINK2]: https://sendgrid.com/docs/Code_Examples/index.html
[LINK3]: https://github.com/Synchro/PHPMailer/
[LINK4]: https://channel9.msdn.com/Blogs/yungchou/Try-It-Yourself-Configure-an-Azure-VNet-to-VNet-Connection
[LINK5]: http://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/
[LINK6]: http://azure.microsoft.com/blog/2015/04/29/introducing-app-service-environment/
