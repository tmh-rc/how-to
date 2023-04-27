# How to create virtual with Apache

---


## Step 1 — Update Hosts File

1. Open the system `hosts` file located at `C:\Windows\system32\dirvers\etc\hosts` in text editor.

2. Replace the following contents:

```
  # localhost name resolution is handled within DNS itself.
    127.0.0.1     example.test
```

---

## Step 2 — Create Folder For Domains

You can either create a new folder in `C:/Apache24/htdocs` or create a new folder path in `D:/`

---

## Step 3 — Update Virtual Host in Apache Configuration

1. Open virtual host configuration file located at `Apache24\conf\extr\http-vhosts.conf` in text editor.

2. Add the following `Virtual Host tag`:

```apacheconf

<VirtualHost example.test:80>
    DocumentRoot "D:/example/public"
    ServerName example.test
    <Directory "D:/example/public">
        DirectoryIndex index.php
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

```

- `<VirtualHost example.test:80>` in `example.test` is domain name & `80` is port number.
- `ServerName example.test` line specifies the server name for the website.
- `DocumentRoot` & `Directory` is your project folder path.
- `DirectoryIndex index.php` line specifies the default index file for the directory.

---

## Step 4 — Update `httpd.conf` File

1. Open `httpd.conf` file located at `C:\Apache24\conf\httpd.conf` in text editor.
2. Uncomment the following line like below example:

```

# Virtual hosts
Include conf/extra/httpd-vhosts.conf

```

---

## Step 5 — Restart Apache

1. Restart Apache in services.
2. Now you can run at browser at:

```
  example.test
```

---
