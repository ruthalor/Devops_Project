# DEPLOYING A LAMB STACK PROJECT IN AWS

A LAMP stack is a bundle of four different software technologies that developers use to build websites and web applications. LAMP is an acronym for the operating system, Linux; the web server, Apache; the database server, MySQL; and the programming language, PHP

## Setting up Apache Web Server

```

   # update a list of packages in package manager

 $ sudo apt update
 $ sudo apt install apache2

```

![sudo apt upgrade](./images/sudo%20apt%20update.png)

![sudo apt install apache2](./images/sudo%20apt%20install%20apache.png)

To verify that apache2 is running as a Service in our OS, use following command:

`sudo systemctl status apache2
`
![sudo systemctl status apche2](./images/confirm%20apache2.png)


If it shows a green text, it meaks everything is done correctly.

Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet.

Open a web browser of your choice and try to access following url

`http://<Public-IP-Address>:80`

The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.

![apache2 working](./images/curl%20public%20ip.png)


If you see above page, then your web server is now correctly installed and accessible through your firewall.

## Installing MySQL

MySQL is a tool used to manage databases and servers. 

We will install mysql using the `apt` package command

`$ sudo apt install mysql-server
`
![install mysql server](./images/sudo%20apt%20install%20mysql.png)

When the installation is finished, log in to the MySQL console by typing:

`$ sudo mysql
`
![mysql running](./images/sudo%20mysql%20running.png)

Use the `sudo mysql_secure_installation` command to remove insecure default settings and enable protection for the database.
![](./images/sudo%20mysql_secure_installation.png)

## INSTALLING PHP


PHP is a programming language used to script websites that are dynamic and interactive.  It is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, we need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. We also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.


To install these 3 packages at once, run:

`sudo apt install php libapache2-mod-php php-mysql`

![installing php](./images/Installing%20php.png)

Once the installation is finished, you can run the following command to confirm your PHP version:

`php -v`
![confirm php](./images/php%20vewrsaion.png)

## Deploying Our Site on Apache's VirtualHost

To test your setup with a PHP script, it’s best to set up a proper Apache Virtual Host to hold your website’s files and folders. Virtual host allows you to have multiple websites located on a single machine and users of the websites will not even notice it.

![Virtual host(1)](./images/VirtualHost%20(1).png)


### CREATING A VIRTUAL HOST/WEB DOMAIN FOR YOUR WEBSITE USING APACHE

In this project, we will set up a domain called `projectlamp`

Create the directory for projectlamp using `mkdir` command as follows:

` sudo mkdir /var/www/projectlamp`

![mkdir for projectlamp](./images/mkdir%20projectlamp%20dir.png)


Next, we assign ownership of the directory with your current system user:
![assign permission](./images/grant%20permission.png)

Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using `vi` or `vim`. They are the same command. 

`sudo vi /etc/apache2/sites-available/projectlamp.conf
`
This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

![](./images/vi%20into%20projectlamp.png)

Run `esc :wq`  to save and terminate vi editor.

You can use the ls command to show the new file in the sites-available directory

`sudo ls /etc/apache2/sites-available
`
![](./images/run%20sudo%20ls%20to%20view.png)

You will see something like this;

`000-default.conf  default-ssl.conf  projectlamp.conf
`
To make sure your configuration file doesn’t contain syntax errors, run:

`sudo apache2ctl configtest
`
![](./images/sudo%20config%20test.png)

Finally, reload Apache so these changes take effect:

`sudo systemctl reload apache2`

![](./images/reloading%20apache.png)

Create an index.html file inside the `/var/www/projectlampstack`

Go to the broswer and open the webpage `http://<public_ip_address>:80`


![](./images/index%20of.png)


### ENABLE PHP ON THE WEBSITE

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file.

To change this behaviour, we n need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

`sudo vim /etc/apache2/mods-enabled/dir.conf
`
![](./images/editing%20vim.png)

After saving and closing the file, you will need to reload Apache so the changes take effect:

`sudo systemctl reload apache2`
![](./images/RELOAD%20APACHE2%20AGAIN.png)

Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.

Create a new file named `index.php` inside your custom web root folder:

`vim /var/www/projectlamp/index.php`

This will open a blank file. Add the following text, which is valid PHP code, inside the file:

`<?php
phpinfo();`


![](./images/vim%20into%20php.png
)

When you are finished, save and close the file, refresh the page and you will see a page similar to this:
![](./images/valid%20php%20file.png)

