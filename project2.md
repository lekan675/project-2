### Project-2 Documentation

## Step 1

*Download updates for ubuntu*

`sudo apt update`

![Download updates for ubuntu](./images/apt-update.png)

*Install ngnix*

`sudo apt install ngnix`

![Install ngnix](./images/install%20ngnix.jpg)

*Verify that nginx was successfully installed*

`sudo systemctl status nginx`

![Verify that nginx was successfully installed](./images/verify%20ngnix.jpg)


*Check ubuntu locally*

`curl http://127.0.0.1:80`

![Check ubuntu locally](./images/access%20ubuntu%20locally.jpg)

*Check on the browser with the public ip address*

`http://<Public-IP-Address>:80`


![Check on the browser with the public ip address](./images/Ngnix%20on%20the%20web.jpg)





## Step 2


*Install MySql*

`sudo apt install mysql-server`

![Install mysql](./images/install%20mysql-server.jpg)


*Log in to the MySQL console*

`sudo mysql`

![log in to the MySQL console](./images/Login%20to%20mysql.jpg)

*Define the root user's password*

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

![Define the root user's password](./images/remove%20insecure%20settings.jpg)


*Start the interactive script by running and validate password plugin*

`sudo mysql_secure_installation`

![Validate Password Plugin](./images/validate%20password%20plugin.jpg)

*Test MySQL login to console*

![Test MySQL login to console](./images/test%20msql%20console%20login.jpg)


## Step 3


*Install PHP PHP fastCGI process manager and PHP-MySQL*

`sudo apt install php-fpm php-mysql`

![Install PHP PHP fastCGI process manager and PHP-MySQL](./images/install%20php-fpm%20and%20php-mysql.jpg)

## Step 4 CONFIGURING NGINX TO USE PHP PROCESSOR

*Create the root web directory for your_domain*
*Assign ownership of the directory with the $USER environment variable*
*open a new configuration file in Nginx’s sites-available*

`sudo mkdir /var/www/projectLEMP`
`sudo chown -R $USER:$USER /var/www/projectLEMP`
`sudo nano /etc/nginx/sites-available/projectLEMP`

![Create the root web directory for your_domain](./images/create%20root%20web%20directory.jpg)

*Paste in the following bare-bones configuration*

![Create the root web directory for your_domain](./images/config%20file.jpg)

*Activate your configuration by linking to the config file from Nginx’s sites-enabled directory*

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

![Activate your configuration by linking to the config file from Nginx’s](./images/activate-config.jpg)

*Test your configuration for syntax errors*

`sudo nginx -t`

![Activate your configuration by linking to the config file from Nginx’s](./images/Tell_ngnix-to-use-config-next-time.jpg)

*Disable default Nginx host that is currently configured to listen on port 80*

`sudo unlink /etc/nginx/sites-enabled/default`

![Disable default Nginx host that is currently configured to listen on port 80](./images/disable_default_ngnix_host.jpg)

*Reload Nginx to apply the changes*

`sudo systemctl reload nginx`

![Reload Nginx to apply the changes](./images/reload%20ngnix.jpg)

*Create an index.html file in that location /var/www/projectLEMP*

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

![creates an index.html](./images/create-an-index.html-file.jpg)

*On your browser, go to the website URL*

`http://<Public-IP-Address>:80`
![creates an index.html](./images/open-the-websit-on-the-browser.jpg)

## Step 5


*Testing PHP with Nginx*

`sudo nano /var/www/projectLEMP/info.php`

*Paste the following lines*

`<?php
phpinfo();`

![Configure Nginx to Use PHP Processor](./images/create-a-test-php-file.jpg)

*Access the page via a browser*

`http://server_domain_or_IP/info.php`

![Access the page via a browser](./images/acess-php-with-ngnix-config-on-the-browser.jpg)


*Remove the file because it contains sensitive information*

`sudo rm /var/www/your_domain/info.php`

![Remove the file because it contains sensitive information](./images/remove%20php%20sensitive%20information.jpg)


## Step 6 Retrieving data from MySQL database with PHP

*Connect to the MySQL console using the root account*

`sudo mysql`

![Connect to the MySQL console](./images/connecting%20to%20mysql.jpg)


*Create a new database from MySQL console*

`CREATE DATABASE `example_database`;`
![Create a new database from MySQL console](./images/create_database.jpg)

*creates a new user named example_user, using mysql_native_password as default*

`CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

![creates a new user named example_user, using mysql_native_password as default](./images/creates%20new%20use%20with%20password%20policy.jpg)

*Give this user permission over the example_database database, then exit*

`GRANT ALL ON example_database.* TO 'example_user'@'%';`

![Give this user permission over the example_database database](./images/give%20the%20user%20permissions%20on%20the%20example_database.jpg)

*Test new user's permissions*

`mysql -u example_user -p`

![Test new user's permissions](./images/test-new-users-permission.jpg)

*Show databases*

`SHOW DATABASES;`

![Show databases](./images/show_databases.jpg)


*Create a test table named todo_list*

`CREATE TABLE example_database.todo_list (
item_id INT AUTO_INCREMENT,
content VARCHAR(255),
PRIMARY KEY(item_id)
);`

![Create a test table named todo_list](./images/create%20test%20table%20todolist.jpg)

*Insert a few rows of content in the test table*

 `INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

![Create a test table named todo_list](./images/inset%20a%20few%20rows.jpg)

*Confirm that the data was successfully saved to the table, then exit*

 `SELECT * FROM example_database.todo_list;`
 `exit`

![Confirm that the data was successfully saved to the table](./images/output.jpg)

*Create a PHP script that will connect to MySQL and query for your content*

 `nano /var/www/projectLEMP/todo_list.php`

 ![Create a PHP script that will connect to MySQL and query for your content](./images/output.jpg)

*Create a new PHP file in your custom web root directory, copy this content into your todo_list.php script, save and close*

`<?php`
`$user = "example_user";`
`$password = "password";`
`$database = "example_database";`
`$table = "todo_list";`

`try {`
  `$db = new PDO("mysql:host=localhost;dbname=$database", $user, ``$password);`
  `echo "<h2>TODO</h2><ol>";`
  `foreach($db->query("SELECT content FROM $table") as $row) {`
    `echo "<li>" . $row['content'] . "</li>";`
  `}`
  `echo "</ol>";`
`} catch (PDOException $e) {`
    `print "Error!: " . $e->getMessage() . "<br/>";`
   `die();`
`}`

 

 *Access this page in your web browser by visiting the public ip address*

 `http://<Public_domain_or_IP>/todo_list.php`


![Access this page in your web browser by visiting the public ip address](./images/browser%20page%20content.jpg)