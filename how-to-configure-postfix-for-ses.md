Title: How To Configure Postfix for SES
Date: 2013-03-05 17:49
Author: alex
Category: Technology
Slug: how-to-configure-postfix-for-ses

I've recently set up my own email server.  I've read several tutorials
on how to install and configure Postfix and IMAP.  One thing that I
noticed is that if you send email from a server without a good
reputation, there's a high probability it will end up in the Spam/Junk
folder or worse, not even get delivered.  I decided to leverage SES as
my SMTP server instead of letting my local installation of Postfix do
all the work.

Install stunnel:

    sudo apt-get install stunnel

Add these lines to /etc/stunnel/stunnel.conf and make sure it starts
properly (you may have to edit /etc/default/stunnel so that it starts
automatically on boot):

    [smtp-tls-wrapper]
    accept = 127.0.0.1:1125
    client = yes
    connect = email-smtp.us-east-1.amazonaws.com:465

Update Postfix /etc/postfix/main.cf. There may already be smtpd versions
but you need to add the smtp version of the options as well.

    relayhost = 127.0.0.1:1125
    smtp_sasl_auth_enable = yes
    smtp_sasl_security_options = noanonymous
    smtp_tls_security_level = may
    smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd

Create/Edit /etc/postfix/sasl\_passwd file with the contents and
replacing username/password from credentials.csv

    127.0.0.1:1125 USERNAME:PASSWORD

Execute the command line:

    sudo postmap hash:/etc/postfix/sasl_passwd

A hash file is created. You can remove the original
/etc/postfix/sasl\_passwd file.

    sudo rm /etc/postfix/sasl_passwd

Restart Postfix:

    sudo /etc/init.d/postfix restart

You should also update your DNS TXT field with an SPF record to bypass
Spam/Junk folders:

    "v=spf1 a mx a:yourserver.yourdomain.com include:yourdomain.com include:amazonses.com ~all"

You should also set up DKIM to have your emails less likely to be
identified as spam.

