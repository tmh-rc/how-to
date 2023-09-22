# How To Create User On MySQL

- [Create User](#create-user)
- [Change User Password](#change-user-password)

## Create User

Create a database named `example_database` and a user named `example_user`. You can replace these names with different values.

First, connect to the MySQL console using the root account:

```bash
sudo mysql
```

To create a new database, run the following command from your MySQL console:

```mysql
CREATE DATABASE example_database;
```

The following command creates a new user named `example_user`. We’re defining this user’s password as `password`, but you should replace this value with a secure password of your own choosing.

```mysql
CREATE USER 'example_user'@'%' IDENTIFIED BY 'password';
```

Now give this user permission over the `example_database` database:

```mysql
GRANT ALL ON example_database.* TO 'example_user'@'%';
```

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with:

```mysql
exit
```

Test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

```bash
mysql -u example_user -p
```

Notice the `-p` flag in this command, which will prompt you for the password used when creating the `example_user` user. After logging in to the MySQL console, confirm that you have access to the `example_database` database:

```bash
SHOW DATABASES;
```

This will give you the following output:

```
Output
+--------------------+
| Database           |
+--------------------+
| example_database   |
| information_schema |
+--------------------+
2 rows in set (0.000 sec)
```

## Change User Password

Open the bash shell and connect to the server as root user:

```bash
mysql -u root -p
```

Run ALTER mysql command:

```mysql
ALTER USER 'example_user'@'%' IDENTIFIED BY 'new-password';
```

Finally type SQL command to reload the grant tables in the mysql database:

```mysql
FLUSH PRIVILEGES;
```

Ref:

- https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04#step-6-testing-database-connection-from-php-optional
- https://www.cyberciti.biz/faq/mysql-change-user-password/
