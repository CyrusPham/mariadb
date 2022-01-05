# MariaDB
MariaDB is an open-source relational database management system, commonly used as an alternative for MySQL as the database portion of the popular LAMP (Linux, Apache, MySQL, PHP/Python/Perl) stack. 
It is intended to be a drop-in replacement for MySQL.

## Install MariaDB
The short version of this installation guide consists of these three steps:
- Update your package index using apt
- Install the mariadb-server package using apt. The package also pulls in related tools to interact with MariaDB
- Run the included `mysql_secure_installation` security script to restrict access to the server
```bash
sudo apt update
sudo apt install mariadb-server
sudo mysql_secure_installation
```
Late, you need set root password. On Ubuntu, the root account for MariaDB is tied closely to automated system maintenance, so we should not change the configured authentication methods for that account.
## Creating an mariadb user
Open MySQL as the root user.
```bash
mariadb -u root -pPASSWORD
```
Open MySQL as the root user.
```sql
CREATE USER 'USERNAME'@'%' IDENTIFIED BY 'PASSWORD';
```
Once you create USERNAME, check its status by entering:
```sql
SELECT User FROM mysql.user;
```
*If you need to remove a user, you can employ the DROP statement:*
```sql
DROP USER 'USERNAME'@'%';
```
## Grant Privileges to MariaDB User
To grant privileges only for yourDB, type the following statement:
```sql
GRANT ALL PRIVILEGES ON 'YOURDB'.* TO 'USERNAME'@'%';
```
It’s crucial to refresh the privileges once new ones have been awarded with the command:
```sql
FLUSH PRIVILEGES;
```
The user you have created now has full privileges and access to the specified database and tables.

Once you have completed this step, you can verify the new user1 has the right permissions by using the following statement:
```sql
SHOW GRANTS FOR 'USERNAME'@'%';
```
*To revoke the grant status:*
```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM USERNAME;
```
**To create a superuser, you need to log in with a superuser account, then run this command:**
```sql
GRANT ALL PRIVILEGES ON *.* TO 'USERNAME'@'%' WITH GRANT OPTION;
```
## Configure MariaDB Remote Access
Config bind-address to: 0.0.0.0.
On Ubuntu systems with MariaDB database server installed, its default configuration file is located at: /etc/mysql/mariadb.conf.d/50-server.cnf
Simply run the commands below to open MariaDB configuration file.
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
When the file is opened, search for a line that begins with bind-address as shown below. It default value should be 127.0.0.1.
What you need to do is change the default value 127.0.0.1 to 0.0.0.0 as shown below:
```init
# this is read by the standalone daemon and embedded servers

# this is only for the mysqld standalone daemon
[mysqld]

#
# * Basic Settings
#
user            = mysql
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
port            = 3306
basedir         = /usr
datadir         = /var/lib/mysql
tmpdir          = /tmp
lc-messages-dir = /usr/share/mysql
skip-external-locking

# Instead of skip-networking the default is now to listen only on
# localhost which is more compatible and is not less secure.
bind-address            = 0.0.0.0

#
# * Fine Tuning
```
## Open/Allow incoming firewall port on Ubuntu
```bash
sudo ufw allow from any to any port 3306 proto tcp
```
After making the change above, save the file and run the commands below to restart the server.
```bash
sudo systemctl restart mariadb.service
```
