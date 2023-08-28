# Deploying a LEMP Stack Application On AWS Cloud

LEMP is an open-source web application stack used to develop web applications. The term LEMP is an acronym that represents L for the Linux Operating system, Nginx (pronounced as engine-x, hence the E in the acronym) web server, M for MySQL database, and P for PHP scripting language.

##CREATING OF OUR UBUNTU SERVER
![](./images/01.%20aws%20instance%20created.png)

Run a `sudo apt update` to download package information from all configured sources.  
![](./images/02.%20sudo%20apt%20update.png)


### INSTALLING NGINX

Run `sudo apt install nginx`

![](./images/03.%20sudo%20apt%20install%20nginx.png)

To verify that nginx was successfully installed and is running as a service in Ubuntu, run:

`sudo systemctl status nginx`

![](./images/04.%20sudo%20systemctl%20status%20nginx.png)

A green text color shows that the server is active.


Accessing the default nginx web server block to see if everything works correctly. curl the local IP address of our local machine which in most case is `127.0.0.1` or the DNS name `localhost` on any web browser on our local machine. `curl http://127.0.0.1:80` or `curl http://localhost:80`

The below result shows nginx has been properly set up and we can deploy our web application.
![](./images/05.%20curl%20http%20127.png)

Also on our local browser we have nginx loading
![](./images/06.%20nginx%20loading%20on%20local%20ip.png)


### Installing MySQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments and this we shall be using in our project.

Again, use `apt` to acquire and install this software:

`sudo apt install mysql-server`

![](./images/07.%20sudo%20apt%20install%20mysql-server.png)

When prompted, confirm installation by typing `Y`, and then `ENTER`.

With mysql_server successfully configured, login into the mysql server.

`sudo mysql`
![](./images/08.%20sudo%20mysql.png)

Exit the MySQL shell with:
   `exit`
![](./images/09.%20exit%20mysql.png)


### INSTALLING PHP  

We have Nginx installed to serve our content and MySQL installed to store and manage our data. Now we can install PHP to process code and generate dynamic content for the web server.

Run `sudo apt install php-fpm php-mysql`

![](./images/11.%20sudo%20apt%20install%20php.png)


### CONFIGURING NGINX TO USE PHP PROCESSOR

To serve our webcontent on our webserver, we create a directory for our project inside the `/var/www/` directory.

Our root web directory for our domain is:
`sudo mkdir /var/www/projectLEMP`


Then we change permissions of the projectLEMP directory to the current user system

`sudo chown -R $USER:$USER /var/www/projectLEMP`

![](./images/12.%20making%20projectLEMP%20direCTORY.png)

Creating a configuration for our server block, here we will use nano:

`sudo nano /etc/nginx/sites-available/projectLEMP`

This will create a new blank file. 

The following snippets represents the configuration required for our web server block to be functional.

![](./images/13.%20nsno%20file%20of%20projectLEMP.png)

We then link the configuration file to the sites-enabled directory

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

To test our configuration for errors we run
`sudo nginx -t`

![](./images/14.%20nginx%20conf%20working.png)

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

`sudo unlink /etc/nginx/sites-enabled/default`
![](./images/15.%20unlink.png)

We then reload nginx for all configurations to take effect `sudo reload nginx`

 Create an index.html file in /var/www/projectLEMP location so that we can test that your new server block works as expected:

 ![](./images/16.%20New%20file%20created.png)
 here we have our file above.

On our local ip we have the file loading: 

![](./images/17.%20nginx%20working%20on%20local%20ip.png)


### Serving PHP Using Nginx

Create an `info.php` file inside the `/var/www/projectLEMP` directory.

![](./images/18.%20new%20php%20file%20created.png)

On a browser enter `http://<public-ip>/info.php`

![](./images/19.%20php%20file%20oepned%20on%20local%20ip.png)

### Connecting PHP with MySQL and Fetching Content

Login into our mysql-server `sudo mysql`


![](./images/20.%20sudo%20mysql.png)


Create a new database, running the following command from MySQL console:

`CREATE DATABASE <db name>;`

Create a new user and assign user a password ```CREATE USER 'db_user'@'%' IDENTIFIED WITH mysql_native_password BY 'db_password' ```


Grant the user permission over the created database ```GRANT ALL ON 'db_name'.* TO 'db_user'@'%' ```

Now exit the MySQL shell with:

`mysql> exit` 

Now login to  mysql with the new user we just created using the command: 

`mysql -u example_user -p`

![](./images/24.%20logged%20in%20to%20mysql.png)


confirm that you have access to the example_database database:

`mysql> SHOW DATABASES;
`
![](./images/25.%20database%20showing.png)

Next, we’ll create a test table named `todo_list`. From the MySQL console, run the following statement:

 `CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
`
![](./images/26.%20create%20table%20and%20insert.png)

Now we will insert a few rows of content using different values: 

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
`

To confirm that the data was successfully saved to your table, run:


![](./images/27.%20database%20working.png)


After confirming that you have valid data in your test table, you can exit the MySQL console:

`mysql> exit`

Now we can create a PHP script that will connect to MySQL and query for your content. Create a php file todo_list.php in /var/www/projectLEMP directory and paste the following code

`vi /var/www/projectLEMP/todo_list.php`

![](./images/28.%20vi%20todo%20list.png)

We can then access our webpage via a browser `http://<publicIP>/todo_list.php`

![](./images/29.%20working%20on%20local%20ip.png)



