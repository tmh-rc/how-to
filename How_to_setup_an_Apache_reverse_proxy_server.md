# How to setup an Apache reverse proxy server

## Prerequisites

To follow this tutorial, you will need:

- One Ubuntu server set up with this initial server setup tutorial, including a sudo non-root user and a firewall.
- Apache 2 installed on your server by following Steps 1 and 2 of [How To Install the Apache Web Server on Ubuntu 20.04][def].

[def]: https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04

## Step 1 — Enabling Necessary Apache Modules

To enable these four modules, execute the following command:

```
sudo a2enmod proxy proxy_http proxy_balancer lbmethod_byrequests
```

To put these changes into effect, restart Apache:

```
sudo systemctl restart apache2
```

## Step 2 — Creating Backend Test Servers

Create `index.js` and put following:
```js
const express = require('express')
const app = express()
const port = 4000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

Create `package.json` by following command:
```
npm init -y
```

Install dependencies
```
npm install express --save
```

Start background server on port `4000`
```
node app.js
```

Now you can test if the server are running by using the `curl` command. 
```
curl http://127.0.0.1:4000/
```

Output:
```
Hello World!
```

## Step 3 — Modifying the Default Configuration to Enable Reverse Proxy

Open the default Apache configuration file using your preferred text editor:
```
sudo vim /etc/apache2/sites-available/000-default.conf
```

Replace all the following contents
```apacheconf
<VirtualHost *:80>
        ProxyPreserveHost On
        RewriteEngine On

        # Upgrade connections to WebSockets
        RewriteCond %{HTTP:Upgrade} =websocket [NC]
        RewriteRule /(.*) ws://localhost:4000/$1 [P,L]

        # Redirect http to https
        RewriteCond %{HTTPS} off
        RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

        # Everything else forwards as HTTP to the node app.
        ProxyPass / http://127.0.0.1:4000/
        ProxyPassReverse / http://127.0.0.1:4000/
</VirtualHost>
```

Once you’re done adding this content, save and exit the file.
To put these changes into effect, restart Apache:

```
sudo systemctl restart apache2
```

If you access `http://your_server_ip` in a web browser, you will see your backend servers’ responses instead of the standard Apache page.

Ref:
- https://www.digitalocean.com/community/tutorials/how-to-use-apache-http-server-as-reverse-proxy-using-mod_proxy-extension-ubuntu-20-04#step-1-enabling-necessary-apache-modules
