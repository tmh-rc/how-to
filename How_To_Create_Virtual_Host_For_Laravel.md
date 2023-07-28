```apache
<VirtualHost *:8080>
        ServerName [IP_ADDRESS]
        DocumentRoot /var/www/laravel/public
        DirectoryIndex index.php

        <Directory /var/www/laravel/public>
                #Options +FollowSymlinks
                AllowOverride All
                #Require all granted

                # Handle authentication
                AuthType Basic
                AuthName "Restricted Content"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
