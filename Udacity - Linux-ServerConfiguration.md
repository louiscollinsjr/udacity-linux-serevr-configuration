#### Udacity - Linux-ServerConfiguration







#### udacity-linux-server-configuration





#### 1. Get Amazon Lightsail Server

#### 2. Connect using your own SSH client

You can connect to the AWS instance using the following address and user name:

- Pulbic IP: 54.145.197.176
- User name: grader
- Application URL: https://www.trott.app
- SSH Connection: ssh -i ~/.ssh/[key file] grader@54.145.197.176 -p 2200



> [keyfile] is the full path of the provided key for the grader account.



### Secure your server.

####3. Update al currently installed packages.

![carbon-3](/screenshots/carbon-3.png)

#### 4. Change the SSH port from **22** to **2200**. 

![carbon-4](/Users/louiscollins/Downloads/carbon-4.png)

- Change port from 22 to  **2200**

- On AWS Lightsail configure networking by adding additional ports: **TCP 2200**, **UDP 123**, **TCP 443**. Remove **TCP 22**.



#### 5. Configure the Uncomplicated Firewall (UFW)

Configure the Uncomplicated Firewall (UFW)** to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

![carbon-5](/Users/louiscollins/Downloads/carbon-5.png)

> *Warning:* When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.





### Give grader account access.

In order for your project to be reviewed, the grader needs to be able to log in to your server.



#### 6. Create a new user account named grader.

![carbon-6](/Users/louiscollins/Downloads/carbon-6.png)

####7. Give grade the permission to sudo.

![carbon-7](/Users/louiscollins/Downloads/carbon-7.png)

- Add the following text and save: **grader ALL=(ALL:ALL) ALL**



#### 8. Create an SSH key pair for grader using the ssh-keygen tool.



On your local device, generate new key pair:

![carbon-8](/Users/louiscollins/Downloads/carbon-8.png)

View and copy your public key:

![carbon-9](/Users/louiscollins/Downloads/carbon-9.png)

On the server, 

![carbon-11](/Users/louiscollins/Downloads/carbon-11.png)

Paste your public key  in authorized_keys and Save.

![carbon-12](/Users/louiscollins/Downloads/carbon-12.png)

Configure login to block root login and use key authorization

![carbon-13](/Users/louiscollins/Downloads/carbon-13.png)

- Set **PermitRootLogin** to **no**
- Set **PasswordAuthenication** to  **no**



Restart SSH

![carbon-14](/Users/louiscollins/Downloads/carbon-14.png)





### Prepare to deploy your project.



#### 9. Configure the local timezone to UTC.

![carbon-15](/Users/louiscollins/Downloads/carbon-15.png)

#### 10. Install and configure Apache to serve a Python mod_wsgi application.

Install Apache

![carbon-16](/Users/louiscollins/Downloads/carbon-16.png)

- If you built your project with Python 3, you will need to install the Python 3 mod_wsgi package on your server

![carbon-17](/Users/louiscollins/Downloads/carbon-17.png)

#### 11.Install and configure PostgreSQL

- Do not allow remote connections
- Create a new database user named `catalog` that has limited permissions to your catalog application database.

![carbon-19](/Users/louiscollins/Downloads/carbon-19.png)

#### 12. Install git

![carbon-18](/Users/louiscollins/Downloads/carbon-18.png)



###Deploy the Item Catalog Project



Clone and setup your **Item Catalog** project from the Github repository you created earlier in this Nanodegree program.

Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your `.git` directory is not publicly accessible via a browser!



#### 13. Clone Catalog

![carbon-20](/Users/louiscollins/Downloads/carbon-20.png)

#### 14. Complete server configuration



##### Install virtual environment (/var/www/catalog)

![carbon-21](/Users/louiscollins/Downloads/carbon-21.png)

#####Install required.txt (/var/www/catalog/catalog)

![carbon-22](/Users/louiscollins/Downloads/carbon-22.png)

![carbon-23](/Users/louiscollins/Downloads/carbon-23.png)



##### Update all currently installed packages.

![carbon-3](/Users/louiscollins/Downloads/carbon-3.png)

##### Create catalog.wsgi

![carbon-27](/Users/louiscollins/Downloads/carbon-27.png)

![carbon-26](/Users/louiscollins/Downloads/carbon-26.png)



##### Configure and Enable a New Virtual Host

![carbon-25](/Users/louiscollins/Downloads/carbon-25.png)

![carbon-24](/Users/louiscollins/Downloads/carbon-24.png)



##### Enable the catalog app virtual host site and disable 000-default.conf

![carbon-39](/Users/louiscollins/Downloads/carbon-39.png)

##### Rename application-pep8.py to __ init __.py

![carbon-28](/Users/louiscollins/Downloads/carbon-28.png)



##### Update __ init __.py with absolute paths

Change __ init __.py to reflect  absolute paths and enable virtual site.

![carbon-29](/Users/louiscollins/Downloads/carbon-29.png)

![carbon-30](/Users/louiscollins/Downloads/carbon-30.png)

- Change `engine = create_engine('sqlite:///category.db')` to `engine = create_engine('postgresql://catalog:catalog@localhost/catalog')`
- Change `client_secrets.json`  to `/var/www/catalog/catalog/client_secrets.json`



##### Monitoring apache logs

![carbon-35](/Users/louiscollins/Downloads/carbon-35.png)

![carbon-34](/Users/louiscollins/Downloads/carbon-34.png)

##### Configure https for facebook login

facebook login requires https and DNS; so trott.app was configured in AWS and Certbot was used to confiure the appropriate certificates... configuration is already noted in previous screen shots.



Cerbot: https://certbot.eff.org/lets-encrypt/ubuntubionic-apache



Install Certbot

![carbon-36](/Users/louiscollins/Downloads/carbon-36.png)

![carbon-37](/Users/louiscollins/Downloads/carbon-37.png)



### Test the web application

Browse to the public ip address of the server and the web application should start up.
