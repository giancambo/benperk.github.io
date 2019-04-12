# Sending email from an Azure App Service using an O365 SMTP server

One of the surprises I had when I moved many of my sites from a domain hoster to an Azure Web App was that the platform only provided web services and that I had lost my SMTP services.  That meant I was not able to send or receive emails from my private domains.  I rushed to get that fixed and found my best solution was to get an Office 365 subscription.  This worked out fine and was back up and running after a few days.

I also had some email contact forms on these web sites that sent an email, again, I had been using the SMTP service provided by my previous domain hoster, but that was gone.  I quickly found SendGrid, which I signed up for and made the required configurations that got it to work and I was back in business.  I wrote an article here that describes how to configure SendGrid to send email from an Azure Web App, this is still valid and I still use it in some cases.  It’s free!

If you have an SMTP server, you can use it via the System.Net.Mail class to send email messages from an Azure Web App.

It is quit simple actually, here is the code, it can also be downloaded from here.

```
protected void sendEmailButton_Click(object sender, EventArgs e)         
 {             
  MailMessage msg = new MailMessage();
  msg.To.Add(new MailAddress("To Email Address", "Benjamin"));
  msg.From = new MailAddress("From Email Address", "You");
  msg.Subject = "Azure Web App Email using smtp.office365.com";
  msg.Body = "Test message using smtp.office365.com on Azure from a Web App";
  msg.IsBodyHtml = true;
  SmtpClient client = new SmtpClient();
  client.UseDefaultCredentials = false;
  client.Credentials = new System.Net.NetworkCredential("O365 UID", "O365 PASS");
  client.Port = 587;
  client.Host = "smtp.office365.com";
  client.DeliveryMethod = SmtpDeliveryMethod.Network;
  client.EnableSsl = true;
  try
  {
   client.Send(msg);
   statusLabel.Text = "Message Sent Succesfully";
  }
catch (Exception ex)
  {
   statusLabel.Text = ex.ToString();
  }
}
```

