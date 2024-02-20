# IMPLEMENTING LOAD BALANCERS WITH NGINX


Load balancing refers to efficiently distributing incoming network traffic across a group of backend servers, also known as a server farm or server pool. In this context, Nginx serves as our load balancer. 

In other words, 

Load balancing is the method of distributing network traffic equally across a pool of resources that support an application. Modern applications must process millions of users simultaneously and return the correct text, videos, images, and other data to each user in a fast and reliable manner. To handle such high volumes of traffic, most applications have many resource servers with duplicate data between them. A load balancer is a device that sits between the user and the server group and acts as an invisible facilitator, ensuring that all resource servers are used equally and in this context, Nginx serves as the load balancer.



# Setting Up a Basic Load Balancer

I am going to provide two Ec2 instances and install apache2 in them. These will be our webserver.

Next I will provide another Ec2 instance, this time install Nginx, configure it to act as a load balancer distributing traffic across the webservers.

# Lets proceed:

Connect to an Ec2 instance, note: Port 8000 has to be open to allow traffice from anywhere.

![](./images/01.%20connected%20to%20our%20ec2%20instance.png)
Now have the Ec2 instance running, 

Run the command below

`sudo apt update -y &&  sudo apt install apache2 -y`

This command updates the Ec2 server and also installs apache2.
![](./images/02.%20update%20and%20install%20apache2.png)

Run the command
`sudo systemctl status apache2`
This verifies that apache is running 
![](./images/03.%20confirm%20apache2%20is%20running.png)

Now I have to configure apache to serve content on port 8000. To do this, run the command 

`sudo vi /etc/apache2/ports.conf `

Add a new Listen directive for port 8000 then save the file. To save file, run the command `:wq`
![](./images/04.%20edit%20listen.png)

Next open the file on apache default config and change the port 80 on the virtualhost to 8000.
Run the command `sudo vi /etc/apache2/sites-available/000-default.conf`

![](./images/05.%20edit%20apache%20.png)

close this file with command
`:wqa!`

Restart apache to load the new configuration with the command
`sudo systemctl restart apache2`

Next we create a new html file with the command
`sudo vi index.html`

Paste the html file 

`        <!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: YOUR_PUBLIC_IP</p>
        </body>
        </html>
`

Note: the content of the body on this html file has to be changed to match with your public ip of this Ec2 instance/Server.
![](./images/06.%20editing%20index.png)

Change the file ownership of the html file with the command
`sudo chown www-data:www-data ./index.html`

Next I have to override the default html file of apache webserver to this new html file we just edited.
This I will have to run the command
`sudo cp -f ./index.html /var/www/html/index.html`

Next, restart the websever to load the new configuration using the command
`sudo systemctl restart apache2
`
Here is my result
![](./images/07.%20my%20public%20ip%20for%20web1.png)

This process has to be repeated on the 2nd webserver.
Here is my result for the webserver 2.
![](./images/07ii.%20public%20ip%20for%20web2.png)


# Configuring Nginx as a Load Balancer

I am going to provide a new Ec2 instance, Open the port 80 to accept traffic from anywhere.

![](./images/08.%20new%20ec2%20instance.png)

Here my Ec2 instance is up and running.

Next will install Nginx into the instance with the command
`sudo apt update -y && sudo apt install nginx -y`
This will update the server and also install nginx here

![](./images/09.%20update%20and%20install%20nginx.png)


Verify that Nginx is installed with the command
`sudo apt update -y && sudo apt install nginx -y`

![](./images/10.%20confirm%20nginx%20is%20running.png)

Open Nginx configuration file with the command 
`sudo vi /etc/nginx/conf.d/loadbalancer.conf`

Paste this config file and edit the webserver 1 and webserver 2 to the public ip address of the server you have previously installed apache on.
Also edit the server name listening on port 80 to the server ip of your load balancer.
`
        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server 127.0.0.1:8000; # public IP and port for webserser 1
            server 127.0.0.1:8000; # public IP and port for webserver 2

        }

        server {
            listen 80;
            server_name <your load balancer's public IP addres>; # provide your load balancers public IP address

            location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
    
`
Here is mine

![](./images/11.%20edit%20nginx%20conf.png)

Now text the configuration file with the command
`sudo nginx -t`
![](./images/12.%20text%20the%20conf.png)

If no errors, restart Nginx to load the new configuration with the command
`sudo systemctl restart nginx`

Paste the ip address of the Nginx load balancer on a web browser and this should be your result   for webserver 1

![](./images/13i.%20output.png)

I refresh the web page and another result for the webserver2 ![](./images/13ii.%20output.png)





