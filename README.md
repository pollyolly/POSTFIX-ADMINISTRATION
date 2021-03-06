### PostFix for External SMTP like Google SMTP, Mailgrid etc
```
1.$sudo apt install ssmtp
2.$sudo apt install postfix
3.$sudo apt-get install -y postfix mailutils libsasl2-2 ca-certificates libsasl2-modules

4. Setup the Mail Server
System mail name: smtp.gmail.com (usually name of user ie. michael)
Root and postmaster mail recipient: michael@ (usually just add @ after the user)
Other destinations to accept mail for (blank for none): usually the default value
Force synchronous updates on mail queue?: No
Local networks: usually the defualt value
Mailbox size limit (bytes): usually default value (0)
Local address extension character: default value (+)
Internet protocols to use: default value (All)

Note: This will generate the main.cf

5. main.cf
=== To Edit in main.cf ===
myhostname = cp1-station
relayhost =  [smtp.gmail.com]:587
=== To Add in main.cf ===
# enable SASL authentication
smtp_sasl_auth_enable = yes
# disallow methods that allow anonymous authentication.
smtp_sasl_security_options = noanonymous
# where to find sasl_passwd
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
# Enable STARTTLS encryption
smtp_use_tls = yes
# where to find CA certificates
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
===

6. Create $touch sasl_passwd
   Write this inside sasl_passwd:
      $vi sasl_passwd
      [smtp.gmail.com]:587 ilcdcp1.upd@up.edu.ph:ugrxyskeomjuomtt"
      
    Note the format: [mail.host.com]:port gmail-email:app_password" This is you create in your gmail App Password for email.
   
7. Generate the sasl_passwd.db
   $postmap /etc/postfix/sasl_passwd
   
8. service postfix restart

9. To test Postfix
   $echo "test body" | mail -s 'test subject' reciever@example.com

10. Change sender email root<myemail@gmail.com> to CP1Name
   $vi /etc/passwd then add "root:x:0:0:CP1Name:/root:/bin/bash"

Note:
If forgot to setup the Postfix use the command below, to regenerate the main.cf.
$sudo dpkg-reconfigure postfix

References:
https://www.linode.com/docs/guides/postfix-smtp-debian7/
https://netcorecloud.com/tutorials/configure-postfix-sendmail-using-external-server/
```
