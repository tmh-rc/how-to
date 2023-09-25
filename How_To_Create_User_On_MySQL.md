# How To Create User On MySQL

- [Create User](#create-user)
- [Change User Password](#change-user-password)
- [Delete User](#delete-user)

## Create User

Create a database named `example_database` and a user named `example_user`. You can replace these names with different values.

First, connect to the MySQL console using the root account:

```bash
sudo mysql
```

To create a new database, run the following command from your MySQL console:

```sql
CREATE DATABASE example_database;
```

The following command creates a new user named `example_user`. We`re defining this user`s password as `password`, but you should replace this value with a secure password of your own choosing.

```sql
CREATE USER 'example_user'@'%' IDENTIFIED BY 'password';
```

Now give this user permission over the `example_database` database:

```sql
GRANT ALL ON example_database.* TO 'example_user'@'%';
```

This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with:

```sql
exit
```

Test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

```bash
mysql -u example_user -p
```

Notice the `-p` flag in this command, which will prompt you for the password used when creating the `example_user` user. After logging in to the MySQL console, confirm that you have access to the `example_database` database:

```sql
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

```sql
ALTER USER 'example_user'@'%' IDENTIFIED BY 'new-password';
```

Finally type SQL command to reload the grant tables in the mysql database:

```sql
FLUSH PRIVILEGES;
```

## Delete User

### Step 1: Connect to MySQL

The first step is to connect to the MySQL server using the command line interface. Open your terminal or command prompt and enter the following command:

```bash
mysql -u root -p
```

This will prompt you to enter the root user’s password. After entering the correct password, you will be connected to the MySQL server.

### Step 2: List existing users
Before dropping a MySQL user, it`s a good practice to list all existing users to ensure that the user you want to drop actually exists. To list all users, enter the following command:

```sql
SELECT User,Host FROM mysql.user;
```

This will display a list of all existing MySQL users.

```
+------------------+-----------+
| User             | Host      |
+------------------+-----------+
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
| example_user     | localhost |
+------------------+-----------+
```

### Step 3: Revoke privileges

Before dropping a MySQL user, it`s important to revoke any privileges associated with the user. This ensures that the user cannot access the database anymore. To revoke privileges, enter the following command:

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'example_user'@'localhost';
```

Replace `example_user` with the username of the user you want to drop. The `localhost` indicates the location from which the user can connect to the database. If the user can connect from multiple locations, you can use `%` instead of `localhost`.

### Step 4: Drop (Delete) user

After revoking privileges, you can drop the user using the following command:

```sql
DROP USER 'user'@'localhost';
```

Replace `user` with the username of the user you want to drop. The `localhost` indicates the location from which the user can connect to the database.

```sql
DROP USER 'user'@'%';
```

If the user can connect from multiple locations, you can use `%` instead of `localhost`.

### Step 5: Verify user was dropped

To verify that the user was dropped successfully, you can list all existing users again using the same command from `Step 2`. The dropped user should no longer appear in the list.

Ref:

- https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04#step-6-testing-database-connection-from-php-optional
- https://www.cyberciti.biz/faq/mysql-change-user-password/
- https://tecadmin.net/delete-mysql-account/
