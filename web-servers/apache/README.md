### Basic Virtual Host with SSL

Same for https://javascript.cs.lmu.edu/

```xml
<VirtualHost \*:80>
ServerName xlg.cs.lmu.edu

                DocumentRoot /apps/xlg.cs.lmu.edu

RewriteEngine on
RewriteCond %{SERVER_NAME} =xlg.cs.lmu.edu
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

<IfModule mod_ssl.c>
        <VirtualHost *:443>
                ServerName xlg.cs.lmu.edu

                ServerAdmin webmaster@localhost
                DocumentRoot /apps/xlg.cs.lmu.edu

SSLCertificateFile /etc/letsencrypt/live/xlg.cs.lmu.edu/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/xlg.cs.lmu.edu/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```
