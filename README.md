# certbot

This is just a simple step to generate SSL certificate using certbot (centos7). This step only cover on step to generate the certificate. We'll assume you already have a working web server. 
 
## Installation

### 1) Install Certbot
We need to install Certbot and enable the  **mod_ssl**  Apache module on the server. Certbot is a simple and easy to use tool that simplifies server management by automating obtaining certificates and configuring web services to use them.

By default, Certbot package is not available in the CentOS 7 default OS repository. We need to enable the EPEL repository, then install Certbot.

To add the EPEL repository run the following command:

```bash
[root@localhost ~]$ yum install epel-release
```
Once enabled, install all the required packages with the following command:
```bash
[root@localhost ~]$ yum install certbot python2-certbot-apache mod_ssl
```
### 2) Obtain and Install SSL for Your Domain

Now that Certbot is installed, you can use it to obtain and install an SSL certificate for your domain.

> Important note : letsencrypt have a Failed Validation limit. Make sure all the information is correct before generating a certificate challenges

Refer : [Lets Encrypt Rate Limit](https://letsencrypt.org/docs/rate-limits/)

Simply run the following command to obtain and install an SSL certificate for your domain:
```bash
[root@localhost ~]$ certbot --apache -d domain.com
```
We can also install a single certificate for multiple domains and subdomains hosted on the server with the ‘-d’ flag, e.g.:
```bash
[root@localhost ~]$ certbot --apache -d domain.com -d www.domain.com -d domain2.com -d *.domain2.com
```
We will be asked to provide an email address and agree to the terms of service.
```bash
[root@localhost ~]$ certbot --apache -d domain.com -d www.domain.com -d domain2.com -d *.domain2.com
```
Use the following for manual challenges - where the certificate is not generated on the same server of the web server (apache server)

## Generate certificate without webserver
```bash
# No need to specify web server (apache)
[root@localhost ~]$ certbot -d 'example.com' -d '*.example.com' --manual --preferred-challenges dns certonly
```
Make sure you have access to your domain provider A record.

you ned to add below acme challenge in your A record

```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org
Requesting a certificate for example.com and *.example.com
Performing the following challenges:
dns-01 challenge for example.com
dns-01 challenge for example.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name
_acme-challenge.example.com with the following value:

gSL3Rqel4Tiq3lZLLYrcukredjvY9arfR-ymKLXgMYQ

Before continuing, verify the record is deployed.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Press Enter to Continue

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name
_acme-challenge.example.com with the following value:

DwbxRB3S_UNyhCQB0-YCxxK3rFCME_6kqes_ANW7O4E

Before continuing, verify the record is deployed.
(This must be set up in addition to the previous challenges; do not remove,
replace, or undo the previous challenge tasks yet. Note that you might be
asked to create multiple distinct TXT records with the same name. This is
permitted by DNS standards.)

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```


We will be asked to provide an email address and agree to the terms of service.
```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator apache, Installer apache
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): admin@domain.com
Starting new HTTPS connection (1): acme-v02.api.letsencrypt.org
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
[https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf.](https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf.) You must
agree in order to register with the ACME server at
[https://acme-v02.api.letsencrypt.org/directory](https://acme-v02.api.letsencrypt.org/directory)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: A
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Starting new HTTPS connection (1): supporters.eff.org
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for domain.com
Waiting for verification...
Cleaning up challenges
Created an SSL vhost at /etc/httpd/conf.d/domain.com-le-ssl.conf
Deploying Certificate to VirtualHost /etc/httpd/conf.d/domain.com-le-ssl.conf
```
Type Y and hit [Enter], and you should see the following output:
```
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
```
Here, you need to choose any one option to continue. If you choose option 1, it will only download an SSL certificate and you need to configure Apache manually to use SSL certificate. If you choose option 2, it will automatically download and configure Apache to use SSL certificate. In this case, choose option 2 and hit [Enter]. When the installation is successfully finished, you will see a message similar to this:
```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 
- Congratulations!
You have successfully enabled [https://domain.com](https://domain.com/)
IMPORTANT NOTES: - Congratulations! Your certificate and chain have been saved at: /etc/letsencrypt/live/domain.com-0001/fullchain.pem Your key file has been saved at: /etc/letsencrypt/live/domain.com-0001/privkey.pem Your cert will expire on 2019-10-22. To obtain a new or tweaked version of this certificate in the future, simply run certbot again with the "certonly" option. To non-interactively renew *all* of your certificates, run "certbot renew" - If you like Certbot, please consider supporting our work by: Donating to ISRG / Let's Encrypt: [https://letsencrypt.org/donate](https://letsencrypt.org/donate) Donating to EFF: [https://eff.org/donate-le](https://eff.org/donate-le)
```
The generated certificate files are available in the **/etc/letsencrypt/live/domain.com** directory. You can check the newly created SSL certificate with the following command:
```bash
[root@localhost ~]$ ls /etc/letsencrypt/live/domain.com/
```
You should see the following output:
```
cert.pem chain.pem fullchain.pem privkey.pem
```
### 3) Check Your SSL Certificate
Open your web browser and type the URL https://domain.com. To check the SSL certificate in Chrome, click on the padlock icon in the address bar for https://domain.com and from the pop-up box, click on ‘Valid’ under the ‘Certificate’ prompt.
### 4) Set up Automatic Renewal
By default, Let’s Encrypt certificates are valid for 90 days, so it is recommended to renew the certificate before it expires. Ideally it would be best to automate the renewal process to periodically check and renew the certificate.

We can test the renewal process manually with the following command.
```bash
[root@localhost ~]$ certbot renew --dry-run
```
The above command will automatically check the currently installed certificates and tries to renew them if they are less than 30 days away from the expiration date.

We can also add a cronjob to automatically run the above command twice a day.

To do so, edit the crontab with the following command:
```bash
[root@localhost ~]$ crontab -e
```

Add the following line:
```
*** */12 * * * root /usr/bin/certbot renew >/dev/null 2>&1**
```
Save and close the file.

Congratulations! We have successfully installed and configured Let’s Encrypt with Apache on a CentOS 7 VPS Or  [Cloud](https://dade2.net/).

# Reference

- [HOW TO INSTALL AND CONFIGURE CERTBOT ON APACHE & CENTOS](https://dade2.net/kb/how-to-install-and-configure-certbot-on-apache-centos/)
