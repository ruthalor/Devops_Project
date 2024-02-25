
# Automating Load balancer Configuration With Shell Scripting




The term "Automating Load Balancer Configuration With Shell Scripting" denotes the utilization of shell scripts to streamline the configuration and administration of load balancers within a computing environment.

![](./images/bash.png)




## Automate the deployment of web servers:

Automating the configuration of load balancers empowers engineers to enhance efficiency and scalability in their network infrastructure. This minimizes the potential for human errors and enhances the overall reliability of the system. This methodology proves especially beneficial in DevOps and cloud computing contexts, where swift and uniform load balancer setup plays a crucial role in optimizing resource allocation and ensuring optimal application performance.


Create an Ec2 instance for webserver 1 and Webserver , write a shell script file to run on it.


## Deploying and Configuring the Webservers

### Steps


**Step 1:** Provide an Ec2 instance, Open port 8000 to allow traffic from anywhere using the security group

**Step 2:** Connect to the webserver using the SSH client

![](./images/02.%20Ec2%20instance%20launched.png)

**Step 3:** Open a file with the command     `sudo vi install.sh`and paste the script

```


#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2




```
![](./images/03.%20sh%20script.png)

**Step 4:** To close this file type the `esc` key then `Shift + :wqa!`

**Step 5:** Change the file permission to make it executable using the command `sudo chmod +x install.sh`

**Step 6:** Run the Script using the command
` ./install.sh PUBLIC_IP`

![](./images//04.%20script%20running.png)


![](./images/04.%20script%20running%202.png)


**Step 7** Visit http://"your-public-ip":8000 on your web browser, you should find a page like this:

![](./images/05.%20webserver1.png)




**REPEAT THIS PROCESS FOR WEBERSERVER 2**

Here we have the 2nd webserver on our publicip listening on port 8000

![](./images/06.%20webserver2.png)


# Deploying and configuring Nginx Load Balancer




![](./images/nginx.jpeg)

Having successfully deployed and configured two webservers, we will move on to the load balancing using Nginx.


Again provide an Ec2 Instance, open port 80 to receive traffic from anywhere using the security group, then connect to the load balancer via the SSH client.

**Step 1:** On your terminal, open a file `nginx.sh` using the command `sudo vi nginx.sh`

**Step 2:** Copy and paste the script below inside:

```
#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx

```

**Step 3:** Close the file using the command
`type esc the shift + :wqa!`

**Step 4:**  Change the file permission to make it executable using the command 
`sudo chmod +x nginx.sh`


**Step 5:**  Run the script with the command
`./nginx.sh PUBLIC_IP Webserver-1 Webserver-2`


### Verifying the setup

**Screenshot for webserver 1**

![](./images/07.%20ngix%20web1.png)


**Screenshot for Webserver2**

![](./images/08.%20nginx%20web2.png)

You can see server1 and server2 taking turns serving the load balancer webpage.

Here is a screenshot of the webservers and load balancer on Ec2
![](./images/09.%20servers%20on%20ec2.png)

**Congratulations on Automating Load balancer Configuration With Shell Scripting!**