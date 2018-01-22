***********************
Lab2 - Blueprint (LAMP)
***********************

.. contents::

Connectivity Instructions
*************************

+------------+----+
| IP         |    |
+------------+----+
| Username   |    |
+------------+----+
| Password   |    |
+------------+----+

Lab Overview
************

Welcome to the Calm Hands-On-Lab - Intermediate Blueprint What we’re
going to do here is extend a basic blueprint:  

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image1.png

This is a basic LAMP Stack (Linux Apache MySQL PHP). In
this lab we’ll build on the previous 1x simple service MySQL
blueprint and evolve this into the multi-stack application you see
above.

**Calm Glossary**

**Service**: One tier of a multiple tier application. This can be made
up of 1 more VMs (or existing machines) that all have the same config
and do the same thing

**Application (App):** A whole application with multiple parts that are
all working towards the same thing (for example, a Web Application might
be made up of an Apache Server, a MySQL database and a HAProxy Load
balancer. Alone each service doesn’t do much, but as a whole they do
what they’re supposed to).

**Macro:** A Calm construct that is evaluated and expanded before being
ran on the target machine. Macros and Variables are denoted in the
@@{[name]}@@ format in the scripts.

Part 1: Accessing and Navigating Calm
*************************************

Getting Familiar with the Tools

1. Connect to https://<HPOC.PC:9440>

2. Login to Prism Central using the credentials specified above (use
   these credentials unless specified otherwise throughout this lab 

3. Click on the Apps tab across the top of Prism

Welcome to Calm! Upon accessing this page you will now notice a new
ribbon along the left - this is used to navigate through Calm.

You are, by default, dropped into the Applications tab and can see all
the instances of applications that have been launched from a blueprint.

For now, let’s step through each tab:

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image2.png

Part 2: Creating a Web Server
*****************************

In this step we’ll add a second tier and connect it to the DBService created from Lab #1 MySQL Blueprint.

Adding App Service
===================

Create the Service was follows.

1. Click the + sign next to **Services** in the **Overview** pane.
2. Notice there are now 2 service block icons in the workspace.
3. Rearrange the icons to your liking, then click on the new Service 2.
4. Name your service **AppService** in the *Service Name* field.
4. The Substrate section is the internal Calm name for this Service. Name this **AppSubstrate.**
5. Make sure that the Cloud is set to **Nutanix** and the OS set to **Linux** 
6. The Service should look as follows:

.. figure:: http://s3.nutanixworkshops.com/calm/lab1/image27.png


Configure the VM
================

Update the VM Configuration section to match the following:

.. figure:: http://s3.nutanixworkshops.com/calm/lab1/image28.png

Configure Network
=================

Scroll to the bottom and add a NIC attached to the **SQLDB** network

.. figure:: http://s3.nutanixworkshops.com/calm/lab1/image22.png


Configure Credentials
=====================

Configure the **Credentials** at the bottom to use the credentials **CENTOS** created earlier.

.. figure:: http://s3.nutanixworkshops.com/calm/lab1/image24.png

Package Configuration
=====================

- Scroll to the top of the Service Panel and click **Package**.
- Here is where we specify the installation and uninstall scripts for this service.
- Nme install package **AppPackage**,
- Set the install script to **shell** and select the credential **CENTOS** created earlier. 
- Copy the following script into the *script* field of the **install** window:

.. code-block:: bash

   #!/bin/bash
   sudo yum update -y
   sudo yum -y install epel-release
   rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
   sudo yum install -y nginx php56w-fpm php56w-cli php56w-mcrypt php56w-mysql php56w-mbstring php56w-dom git
   mkdir -p /var/www/laravel
   echo "server {
         listen   80 default_server;
         listen [::]:80 default_server ipv6only=on;
         root /var/www/laravel/public/;
         index index.php index.html index.htm;
         location / {
             try_files \$uri \$uri/ /index.php?\$query_string;
         }
         # pass the PHP scripts to FastCGI server listening on/var/run/php5­fpm.sock
         location ~ \.php$ {
                  try_files \$uri /index.php =404;
                  fastcgi_split_path_info ^(.+\.php)(/.+)\$;
                  fastcgi_pass 127.0.0.1:9000;
                  fastcgi_index index.php;
                  fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
                  include fastcgi_params;
         }
   }" |sudo tee /etc/nginx/conf.d/laravel.conf
   sed -i 's/80 default_server/80/g' /etc/nginx/nginx.conf
   if `grep "cgi.fix_pathinfo" /etc/php.ini` ; then
      sed -i 's/cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php.ini
   else
      sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php.ini
   fi
   
   #sudo php5enmod mcrypt
   sudo systemctl restart php-fpm
   sudo systemctl restart nginx
   if [ ! -e /usr/local/bin/composer ] then
      curl -sS https://getcomposer.org/installer | php
      mv composer.phar /usr/local/bin/composer
      chmod +x /usr/local/bin/composer
   fi
   
   git clone @@{App_git_link}@@ /var/www/laravel
   sed -i 's/DB_HOST=.*/DB_HOST=@@{DBService.address}@@/' /var/www/laravel/.env
   sudo su - -c "cd /var/www/laravel; composer install ; php artisan migrate"
   
   chown -R nginx:nginx /var/www/laravel
   chmod -R 777 /var/www/laravel/
 
   systemctl restart php-fpm
   systemctl restart nginx
   sudo yum install firewalld -y
   sudo service firewalld start
   sudo firewall-cmd --add-service=http --zone=public --permanent
   sudo firewall-cmd --reload
   sleep 2


Here you see variables like before, but also something new:

@@{MySQL.address}@@

This is a **Calm Macro**. What this does it get the IP address from the **MySQL** server and replaces that in this script. Using the Macro,  it doesn’t matter what IP the DB comes up with, the PHP server will always know where it’s DB is. 

There are many more native macros and can be foundin the the Calm Deep-Dive sections...

Fill in the uninstall script with the same basic exit as before:

.. code-block:: bash

   #!/bin/bash
   echo "goodbye!"

Since we need the DB IP Address to bring up the AppServer, we need to add a **Dependency**.

- Click on the . **AppService** service, 
- Click on the Arrow icon that appears right above it,
- Click on the **MySQL** service.

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image5.png

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image6.png

This tells Calm to hold running the script until the **MySQL** service is up. 

**Save** the blueprint, then click on the **Create** action from the **Overview** pane to see this.

Part 3: Scale-out AppService and Load Balancer
**********************************************

In this part we’re going to complete the provisioning of the blueprint.  

1. Click on the **AppService** service. 
2. Click on the **Service** tab. 
3. Change **Number of replicas** under **Deployment Config** from 1 to 2.  

This service will now deploy 2 VMs with the same configuration rather than just 1

Load Balancer
=============

Now that we've added redundancy or load balancing capacity to the AppServer we need something to actually perform the load balancing.

1. Add another Service. This will be our load balancer, so name the Service **HAProxy**.
2. Name the **Substrate** AppProxy
3. Name the VM **ProxyVM** and configure the rest of the service.
2. Remember to configure the NIC and credentials at the bottom

Under **Package** configure the following install script

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image7.png

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image4.png

Under **Package** configure the following install script:

.. code-block:: bash

   #!/bin/bash
   set -ex
 
   sudo yum update -y
   sudo yum install -y haproxy
   echo "global
         log 127.0.0.1 local0
         log 127.0.0.1 local1 notice
         maxconn 4096
         quiet
         user haproxy
         group haproxy
         defaults
               log     global
               mode    http
               retries 3
               timeout client 50s
               timeout connect 5s
               timeout server 50s
               option dontlognull
               option httplog
               option redispatch
               balance  roundrobin
         # Set up application listeners here.
         listen stats 0.0.0.0:8080
         mode http
         log global
         stats enable
         stats hide-version
         stats refresh 30s
         stats show-node
         stats uri /stats
         listen admin
         bind 127.0.0.1:22002
         mode http
         stats uri /
         frontend http
         maxconn 2000
         bind 0.0.0.0:80
         default_backend servers­http
         backend servers-http" | tee /etc/haproxy/haproxy.cfg
         sudo sed -i 's/server host-/#server
         host-/g' /etc/haproxy/haproxy.cfg
         hosts=$(echo "@@{AppService.address}@@" | sed 's/^,//' | sed 's/,$//' | tr "," "\n")
         port=80
         for host in $hosts do
            echo "  server host­${host} ${host}:${port} weight 1 maxconn
            100 check" | tee ­a /etc/haproxy/haproxy.cfg
         done
         
         sudo systemctl daemon­reload
         sudo systemctl restart haproxy
         sudo yum install firewalld -y
         
         sudo service firewalld start
         sudo firewall-cmd -add-service=http --zone=public --permanent
         sudo firewall-cmd --add­port=8080/tcp --zone=public --permanent
         sudo firewall-cmd --reload
 
 
Notice we’re using **@@{PHP.address}@@** here just like before, but
putting it in a loop to get both PHP servers added to the HAProxy
config. Add the **Dependency** arrow like before.

Add the following uninstall script

.. code-block:: bash

   #!/bin/bash
   echo "goodbye!"

Your blueprint should now look like this:

.. figure:: http://s3.nutanixworkshops.com/calm/lab2/image8.png

Save the blueprint, and launch it.



.. |image0| image:: lab2/media/image1.png
.. |image1| image:: lab2/media/image2.png
.. |image2| image:: lab2/media/image3.png
.. |image3| image:: lab2/media/image4.png
.. |image4| image:: lab2/media/image5.png
.. |image5| image:: lab2/media/image6.png
.. |image6| image:: lab2/media/image7.png
.. |image7| image:: lab2/media/image4.png
.. |image8| image:: lab2/media/image8.png
.. |image9| image:: lab2/media/image9.png
.. |image10| image:: lab2/media/image10.png
