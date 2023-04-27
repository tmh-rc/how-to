# How To Change Port Number in Apache in Ubuntu

## 1. Open Apache Config File

Open terminal and run the following command to open Apache server configuration file.

CentOS/Fedora:

```
sudo vi /etc/httpd/conf/httpd.conf
```

Ubuntu/Debian:

```
sudo vi /etc/apache2/ports.conf
```

## 2. Change Apache Port Number

You will find the following line binding Apache to port 80

```
Listen 80
```
Change it to
```
Listen 8080
```

## 3. Open Virtual Host Configuration (for Ubuntu/Debian)

When you change port number in Apache on Ubuntu/Debian systems, you need to also change port number in virtual host configuration file

If you have configured virtual host for your website (e.g www.mysite.com) at /etc/apache2/sites-enabled/mysite.conf then you can open that file instead.

```
sudo vim /etc/apache2/sites-enabled/mysite.conf
```

Otherwise, open the default virtual host configuration file at

```
sudo vi /etc/apache2/sites-enabled/000-default.conf
```

You will find the following line

```
<VirtualHost: *:80>
```

Change it to

```
<VirtualHost: *:8080>
```

## 4. Restart Apache Server

Restart Apache Server to apply changes

```
sudo systemctl restart apache2
```
or
```
sudo service apache2 restart
```

Open browser and enter http://your_ip_adress:8080 (e.g http:192.168.1.102:8080). You will see the Apache default page

![][def]

[def]: https://ubiq.co/tech-blog/wp-content/uploads/2020/04/Apache-Default-Page-on-Debian-Ubuntu.png

If not working, you need to allow port number at firewall by following command:

```
sudo ufw allow 22
```

Output:
```
Rule added
Rule added (v6)
```
