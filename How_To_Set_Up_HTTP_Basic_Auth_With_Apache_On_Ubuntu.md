# How To Set Up HTTP Basic Auth with Apache on Ubuntu

## Step 1 — Installing the Apache Utilities Package

First, update your server’s package index:

```console
sudo apt update
```
Then install the Apache utilities package:

```console
sudo apt install apache2-utils
```

## Step 2 — Creating the Password File
The `htpasswd` command allows you to create a password file that Apache can use to authenticate users. You’ll create a hidden file for this purpose called `.htpasswd` within your `/etc/apache2` configuration directory.

The first time you use this utility, you need to add the `-c` option to create the specified `.htpasswd` file. Here, we specify a username (`sammy` in this example) at the end of the command to create a new entry within the file:

```console
sudo htpasswd -c /etc/apache2/.htpasswd sammy
```
You will be asked to supply and confirm a password for the user.

Leave out the `-c` argument for any additional users you wish to add, as in the following example, so you don’t overwrite the file:

```console
sudo htpasswd /etc/apache2/.htpasswd another_user
```

If you check the contents of the file, it will contain the username and the encrypted password for each record:

```console
cat /etc/apache2/.htpasswd
```

Output:

```console
sammy:$apr1$eponJaBR$9uyVIRpDpbHoseI.hS1cq/
another_user:$apr1$dDXiQxte$RGn3CVfFLQOPf5lSJgNvV1
```

## Step 3 — Configuring Apache Password Authentication

Open up the config file with a command-line text editor such as `vim`
```console
sudo vim /etc/apache2/sites-available/your_domain.conf
```
Inside, insert following config and replace your domain or ID in `your_domain` :
```apacheconf
<VirtualHost *:80>
    ServerName your_domain
    DocumentRoot /var/www/your_domain
    DirectoryIndex index.php
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <Directory "/var/www/your_domain">
            #Options +FollowSymlinks
            AllowOverride All
            #Require all granted

            #Handle authentication
            AuthType Basic
            AuthName "Restricted Content"
            AuthUserFile /etc/apache2/.htpasswd
            Require valid-user
    </Directory>
</VirtualHost>
```
Before restarting the web server, you can check the configuration with the following command:

```console
sudo apache2ctl configtest
```
If everything checks out and you get `Syntax OK` as output, you can restart the server to implement your password policy:

```console
sudo systemctl restart apache2
```
Since systemctl doesn’t display the outcome of all service management commands, use the the status command to be sure the server is running:

```console
sudo systemctl status apache2
```

Output:

```console
apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor prese>
     Active: active (running) since Fri 2022-04-29 17:12:24 UTC; 4s ago
       Docs: https://httpd.apache.org/docs/2.4/
    Process: 4493 ExecStart=/usr/sbin/apachectl start (code=exited, status=0/SU>
   Main PID: 4514 (apache2)
      Tasks: 55 (limit: 9508)
     Memory: 5.8M
     CGroup: /system.slice/apache2.service
             ├─4514 /usr/sbin/apache2 -k start
             ├─4516 /usr/sbin/apache2 -k start
             └─4517 /usr/sbin/apache2 -k start
```

## Step 4 — Confirming Password Authentication
To confirm that your content is protected, try to access your restricted content in a web browser by navigating to `https://your_domain_or_server_IP`.

You will be presented with a username and password prompt that looks like the following:

![Confirming Password Authentication][def]

[def]: https://assets.digitalocean.com/articles/set-pwauth-apache-20.04/websign-in.PNG

If you enter the correct credentials, you will be allowed to access the content. If you enter the wrong credentials or hit “Cancel”, you will receive the “Unauthorized” error page:
![Unauthorized error page][def2]

[def2]: https://assets.digitalocean.com/articles/set-pwauth-apache-20.04/webunauthorized-msg.PNG

Ref: 
- https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-apache-on-ubuntu-20-04
