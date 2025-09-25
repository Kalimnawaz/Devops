xFusionCorp Industries is planning to host a WordPress website on their infra in Stratos Datacenter. 
They have already done infrastructure configuration—for example, 
on the storage server they already have a shared directory /vaw/www/html that is mounted on each app host under /var/www/html directory. 
Please perform the following steps to accomplish the task:


a. Install httpd, php and its dependencies on all app hosts.

b. Apache should serve on port 6300 within the apps.

c. Install/Configure MariaDB server on DB Server.

d. Create a database named kodekloud_db3 and create a database user named kodekloud_gem identified as password LQfKeWWxWD. 
Further make sure this newly created user is able to perform all operation on the database you created.

e. Finally you should be able to access the website on LBR link, by clicking on the App button on the top bar. 
You should see a message like App is able to connect to the database using user kodekloud_gem


On All App Hosts
Step 1: Install Apache (httpd), PHP, and dependencies

sudo yum install -y httpd php php-mysqlnd php-fpm php-json

Step 2: Change Apache port to 6300

Edit Apache config:

sudo vi /etc/httpd/conf/httpd.conf


Find the line:

Listen 80


Change it to:

Listen 6300


Also update the <VirtualHost *:80> (if present) to <VirtualHost *:6300>.

**Step 3: Start and enable Apache
sudo systemctl enable httpd
sudo systemctl start httpd


Verify:

sudo ss -tulnp | grep httpd


You should see it listening on :6300.

On DB Server
Step 4: Install MariaDB
sudo yum install -y mariadb-server
sudo systemctl enable mariadb
sudo systemctl start mariadb

Step 5: Secure MariaDB (optional but recommended)
sudo mysql_secure_installation
(set root password, remove test DB, disable remote root login, etc.)


Step 6: Create DB and user
Login to MariaDB:
mysql -u root -p
Inside MySQL shell:

CREATE DATABASE kodekloud_db3;
CREATE USER 'kodekloud_gem'@'%' IDENTIFIED BY 'LQfKeWWxWD';
GRANT ALL PRIVILEGES ON kodekloud_db3.* TO 'kodekloud_gem'@'%';
FLUSH PRIVILEGES;


Exit:

EXIT;

Step 7: Install wget
sudo yum install -y wget

Step 8: Download and configure WordPress
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
sudo chown -R apache:apache /var/www/html
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
sudo chown apache:apache /var/www/html/wp-config.php

Edit the file to update DB details:

sudo vi /var/www/html/wp-config.php


And set:

define( 'DB_NAME', 'DB server name' );
define( 'DB_USER', 'DB user name' );
define( 'DB_PASSWORD', 'DB password' );
define( 'DB_HOST', '<DB_SERVER_IP>' );



